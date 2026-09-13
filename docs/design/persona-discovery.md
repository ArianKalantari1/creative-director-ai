# Persona discovery

**Status:** partly shipped. Four of the six code changes proposed below are now on
`senpai-reel@main`; the mechanism section has been rewritten because the original
version had the correction loop backwards. See [Corrections to this document](#corrections-to-this-document).
**Supersedes:** the per-client taxonomy fix discussed under #15/#21 — that became one piece of this.

---

## The problem

`analysis/taxonomy.py` hardcoded eleven topics — `ATS`, `Resume`, `Interview`, `Visa` — as a
module constant, injected straight into the extraction prompt. It was not client-scoped.

Point a second client at a different niche and every extracted idea gets classified against
*resume and interview* categories. Almost everything lands in `General`, and three features
degrade at once: topic search returns one useless bucket, the content gap map loses its axis,
and Content Studio's grounding becomes effectively random.

Multi-client support (#11) scoped the **data** but not the **vocabulary**. That was the immediate
bug, and it is now fixed. Fixing only that would have missed the more interesting thing underneath it.

## The thing underneath

The Jobs-AU topic list exists because its author already knew who he was being: someone who
helps people apply for jobs in Australia. Categories first, scraping second. That order is
correct and it is how the tool is actually used.

But it assumes the answer to a question most people cannot answer.

Someone who wants to start posting usually does not know their niche. They have raw material —
a degree, a job they left, a thing they built, a system they had to learn the hard way — and no
idea which part of it is worth anything to anyone else. Every content tool assumes that question
is already settled. The ones that don't just present a list to pick from, which is the same
assumption wearing a hat.

**A worked example.** Someone arrives in Australia from Japan and cannot write an Australian
résumé, because the Japanese format is different in ways nobody explains. They do not think
*"I have valuable knowledge."* They think *"I am confused."* The valuable thing is exactly the
gap between two systems that only they can see from both sides.

No market-gap analysis surfaces that, because the gap is not in the market. It is in them.

## What the system can and cannot do

**It cannot tell you who you are.** Worth stating plainly, because the opposite claim is
seductive and would make the product worse.

**It can find where what you already have meets what nobody covers well.** That is a matching
problem, and matching is tractable.

The distinction decides output quality. Topics proposed purely from market gaps produce gaps the
person has no standing to fill, and content written from unoccupied authority reads as hollow
immediately. Readers detect it faster than writers do.

So the intersection is three-way, not two:

```
   what you have evidence for
              ×
   what the niche actually discusses
              ×
   what nobody covers well
```

The content gap map already computes the second and third. **The missing input is the first**,
and no amount of scraping produces it.

---

## The mechanism

This section replaces the original proposal, which described the wrong loop. The corrected
version is not a guess: **it is a description of something that already works.**

### The reference implementation already exists

The `linkedin-post-ari-voice` skill is a working instance of this mechanism, built by hand over
several months of ordinary use. It is worth reading before designing anything, because it
settles by demonstration several questions this document previously treated as open.

Its structure:

| Section | What it holds |
|---|---|
| Voice rules | lexical and tonal constraints — Australian spelling, sentence rhythm, emoji policy |
| Forbidden phrases | a verbatim never-generate list |
| *Mistakes Claude has made on past drafts* | one line per correction, stated as a rule |
| Step 7: when Ari gives feedback | the correction protocol itself |
| Iteration log | V1 → V1.1 → V1.2, each entry naming what triggered the change |

### The loop, correctly stated

```
system writes a FULL DRAFT
        ↓
person rewrites it
        ↓
diff generated against corrected
        ↓
distil the difference into a RULE
        ↓
rule joins the rulebook; the raw pair is discarded
```

Four things about this differ from the original proposal, and each one matters.

**1. The unit of correction is a draft, not a topic.** The original had the system proposing
topics and the person accepting or rejecting them. That throws away almost all the signal. A
rejected topic label tells you one bit. A rewritten sentence tells you about register, stance,
rhythm, what the person will and will not claim, and who they are willing to sound like. Every
entry in the mistakes log came from a draft, not from a label:

> **Wrote "I had the wrong audience in mind"** — implicitly blamed the audience. Ari called this
> arrogant. Always frame as: he didn't tell the right story for the room.

No topic-acceptance UI would ever have surfaced that.

**2. The output is a distilled rule, not a stored pair.** The original said to store the
replacement alongside what it replaced. The working implementation does not do that. It stores
*"always optimise for clarity over cleverness"* — the generalisation — and discards the draft
that produced it.

This is better on three counts. Rules are **reviewable** (a person can read their own rulebook
and disagree with it; nobody can audit a pile of diffs). Rules are **portable** across drafts,
where a substitution pair only fires on a near-exact match. And rules do not **accumulate a
corpus of someone's rejected writing**, which is the difference between a preferences file and
a surveillance record.

**3. Rules are not all the same kind.** The working skill has at least four types, which emerged
rather than being designed:

| Type | Example from the live rulebook |
|---|---|
| Lexical | "learnt" not "learned" — Australian spelling |
| Tonal | never position the writer above the reader |
| Factual | verify timeline specifics before stating them |
| Structural | establish the post's goal *before* drafting |

Structural rules are the interesting ones, because they change the process rather than the text.
*"Drafted without asking the goal"* became a new step in the skill, not a phrase on a banned
list. A rulebook schema that only models lexical substitution cannot represent that, and would
have quietly dropped the most valuable correction in the log.

**4. Voice comes first; identity accrues as a by-product.** Nobody interviewed Ari about his
identity. The self-description section of that skill — career changer, Sydney, Japanese, the
projects, the audience — accumulated *because it was needed to fix drafts*. Identity was the
residue of the loop, not its input.

### The interview is optional, and already was

The original made the interview Flow step 1 — a gate before anything else could happen.

The working skill contradicts this directly. Its Mode 2 says:

> **Mode 2: Refinement mode** — Ari has a draft (his or yours) and wants it tightened or
> rewritten. **Skip interviewing.** Go straight to drafting using his voice rules.

Interviewing runs in Mode 1 only, and only when there is no draft to work from. The design
should match: **the interview is an accelerator for a cold start, not a prerequisite.** A person
who arrives with something they have already written should never be made to answer questions
first.

This dissolves original open question 1 ("how much interview is too much?"). It was answered
months ago, in practice, by making it conditional.

---

## Persona

The `clients` table is already most of a persona record — `name`, `niche`, `target_audience`,
`brand_voice_notes`, `notes`, plus its own competitor account list. It is a persona wearing the
wrong noun.

A persona is an identity you post *as*, on a platform, for a purpose. One person holds several:
a recruiter voice on LinkedIn, an AI-practitioner voice elsewhere. Each implies different
accounts to watch and a different vocabulary.

Still missing from `clients` (verified against `core/db.py` on `main`):

| Field | Why | Status |
|---|---|---|
| `platform` | LinkedIn, TikTok, Instagram — format and convention differ | not built |
| `goal` | authority · leads · community · job search. Changes which gaps are worth filling, and the live skill shows it also sets **length budget** | not built |
| rulebook | the distilled rules above. The load-bearing part | not built |
| evidence inventory | what you can credibly speak to, and why | not built |
| topics | per-persona rows, replacing the module constant | **shipped** |

### Evidence inventory and rulebook are two things, not one

The original ran these together. They are different and want different storage.

The **evidence inventory** answers *what may this persona claim?* — things that happened: what
you built, what broke, what you get asked repeatedly, what you learned by being confused. This
project already has a working instance: `ari-facts.md`, verified facts plus a DO NOT SAY list,
where an unverified row blocks a claim from shipping. It is append-mostly and each row is
independently checkable.

The **rulebook** answers *how does this persona speak, and what must the process do?* It is
distilled, revisable, and rules supersede each other — V1 said "default zero emojis", V1.1
replaced it with "minimum 3 functional emojis". That is a rule being *overturned*, which an
append-only evidence store handles badly and a versioned rulebook handles naturally.

Keep them separate. Conflating them produces a store that is wrong for both.

## Flow

```
0. Cold start?   If the person arrives with existing writing, skip to 3.
                 The interview exists for people who have nothing to diff against.

1. Interview     (optional) background · what you have built · what people ask you ·
                 what you explain repeatedly · what frustrated you enough to learn

2. Sample scrape a few hundred reels, not five thousand. Cheap, and enough
                 to see the shape of a niche

3. Draft         the system writes something real and complete — not a topic list,
                 not a suggestion. A draft is the only artefact rich enough to correct

4. Correct       the person rewrites it. The diff is the signal

5. Distil        the difference becomes a rule, typed (lexical/tonal/factual/structural).
                 The raw pair is discarded once the rule is written

6. Commit        topics get stable IDs. Full scrape now points somewhere
```

Sampling before committing costs a few dollars and **dissolves the intent-first versus
content-first question** rather than answering it — you get intent, then content, in that order.
The original framing treated a sequence as a fork.

## Reversibility

A taxonomy decision must not be a one-way door, because the whole premise is that people change
their minds once they see what is out there.

Two guarantees make changing direction cheap, and **both are now built**:

**Transcripts are retained and extraction runs from stored text.** Re-classifying does not
re-scrape or re-transcribe. At the project's documented GPT-4o-mini rate of ~$0.0003/reel,
re-extracting 5,000 reels costs roughly **$1.50** and one queue run. The expensive stages —
Apify and transcription — are untouched.

**Taxonomies are versioned, never overwritten** (`taxonomy_versions`, `taxonomy_id` pinned per
extraction batch). Old assignments stay; new ones are added alongside. Without this, switching
destroys the historical comparison that makes a large corpus worth having: you could no longer
tell whether a gap closed or a label moved.

With both, intent-first is a draft rather than a commitment.

## Boundary with the existing content skills

`ari-content-strategist` reads `ari-context.md` (the spine, the reader, the pillars) and
`ari-facts.md` (verified facts, DO NOT SAY) and picks a concept. It **assumes the persona already
exists** — it cannot run for someone who has not written those files.

That is the clean split:

```
Senpai Reel  ──produces──>  persona + evidence + rulebook + topics  ──consumed by──>  ari-content-strategist
              (who am I, what do I have,                                 (what do I post
               how do I sound, what does this niche discuss)              about it this week)
```

Discovery upstream, strategy downstream. **Do not build a second interviewer** without first
checking whether the strategist's questions can be reused — duplicated elicitation logic that
drifts apart is worse than either copy alone.

Note what this boundary implies: the skills are the **hand-run prototype** of what Senpai Reel
should automate. `linkedin-post-ari-voice` is one persona's rulebook maintained by hand. The
product is that file, for ten clients, maintained by the loop instead of by memory.

## What changes in the code

| Change | Status |
|---|---|
| Topics move from `analysis/taxonomy.py` constant to per-persona rows | **shipped** — `core/taxonomy.py:topics_for_prompt(client_id)` |
| `taxonomy_versions` with stable topic IDs | **shipped** — `core/db.py:415` |
| Extraction prompt takes the persona's topics | **shipped** — `analysis/extraction.py:build_system_prompt(client_id)` |
| Content Studio topic dropdown reads persona topics | **shipped** — `pages/Content_Studio.py:91`, `topic_names(client_id)` |
| Persona gains `platform`, `goal`, rulebook, evidence inventory | **outstanding** — `clients` has none of these |
| Orphan reporting | **outstanding** — the `orphan_posts` in `core/clients.py` is client-post linkage, unrelated |

`content_type` (`tip`, `warning`, `stat`, `myth`, `story`, `hook`, `cta`) stays **shared**. It
describes the shape of an idea, not its subject, and shape is niche-independent. Note that
senpai-reel #27 adds a second axis (`unit_role`) beside it to separate subject from technique;
that is orthogonal to persona and does not change this.

Orphan reporting is where the system tells you something you did not know — a subject the niche
discusses that you had not thought to name. It only works if existing topics hold still enough
to define what "does not fit" means, which is another argument for pinning.

## Open questions

1. ~~How much interview is too much?~~ **Answered** by the live skill: interview runs in Mode 1
   only and is skipped entirely when a draft exists.
2. ~~Does rejection get recorded, and is that surveillance-shaped?~~ **Answered** by distillation.
   Rules are kept; raw rejected drafts are not. A person can read and edit their own rulebook,
   which is the opposite of a surveillance record.
3. **How many rounds until a persona stabilises?** Still open, but now *measurable from existing
   data* rather than hypothetical: the iteration log went V1 → V1.1 → V1.2 within a single day
   of use, then stopped. Whether that is convergence or just a pause is exactly the question. The
   metric is corrections-per-draft over time.
4. **Sample size for step 2.** A few hundred reels is a guess. Worth measuring at what point the
   proposed topics stop changing as more data arrives.
5. **Multiple personas, one corpus.** Two personas in overlapping niches may watch the same
   accounts. `client_posts` already models shared posts; do they share extractions too, or does
   each persona re-extract under its own vocabulary? Sharing is cheaper; separate is more honest.
6. **NEW — can rule distillation be automated, or does it need a person?** Every rule in the live
   rulebook was written by a human reading a correction and generalising it. Whether a model can
   do that reliably is untested, and the failure mode is bad: a wrongly generalised rule silently
   corrupts every future draft. Suggested posture — the system *proposes* a rule from the diff,
   the person approves it before it enters the rulebook. Cheap to do, and it keeps the rulebook
   something the person actually agrees with.
7. **NEW — do rules conflict, and who resolves it?** V1.1 overturned V1's emoji rule. That was a
   person noticing. At thirty rules across ten personas, contradictions will not be noticed by
   anyone. No answer yet.

## What this does not solve

It does not make the product sellable. The acquisition-rights blocker (#16, #20) is untouched —
this is about what the tool is *for*, not who may pay for it.

It does not replace judgement. The system proposes; the person decides. A tool that decided
identity on someone's behalf would be both wrong and unpleasant.

## Later, not now: learning the labels instead of assigning them

Topic assignment is currently one LLM call per transcript against a fixed vocabulary. That is
classification, and it is the simplest thing that works.

A different approach becomes available once corrections accumulate: treat them as **weak
supervision** — noisy, cheap labels a model learns from in aggregate rather than trusting
individually. Labelling functions vote, their disagreements are modelled, a label model resolves
them.

**There is a real tension here and the original document papered over it.** It claimed that
storing corrections with their substitutions "keeps that door open, and it costs nothing to do
now." That is wrong on both halves. Retaining raw correction pairs is exactly the corpus the
distillation step is designed *not* to accumulate, and it does cost something: it turns a
preferences file into a record of everything someone rejected about their own writing.

So the honest position is a trade-off, not a free option:

- **Distil and discard** (recommended) — the rulebook stays small, legible and owned by the
  person. Weak supervision over raw pairs becomes unavailable. Rules themselves can still be
  mined later; there are just far fewer of them.
- **Retain pairs** — keeps the weak-supervision door open, at the cost of accumulating sensitive
  data for a technique nobody has committed to and which needs a corpus that does not exist.

Recommendation: distil and discard, and revisit only if rule quality plateaus in a way that more
data would plausibly fix. Do not retain data now to buy an option nobody has priced.

---

## Corrections to this document

Recorded rather than silently edited, because the errors are instructive.

- **The correction loop was backwards.** The original had the system proposing *topics* and the
  person accepting or rejecting them. The real loop corrects *drafts* and distils *rules*. This
  was not a subtle difference: the original design would have collected roughly one bit per
  interaction where the working version collects a paragraph.
- **The interview was made a gate.** It was already optional in practice, and had been for
  months. Written into the doc as Flow step 1 anyway.
- **"Store the substitution — it costs nothing"** was wrong. It costs privacy, and it
  contradicts the discard step that makes the rulebook safe to keep.
- **Evidence inventory and rulebook were conflated.** They have different shapes: one is
  append-mostly and independently verifiable, the other is distilled and supersedable.
- **Four of the six proposed code changes were already built** when this was written as a
  proposal. Verified against `senpai-reel@main` before this revision; marked inline above.

*Still written to be disagreed with. Open questions 6 and 7 are the weakest parts and deserve
the most argument.*
