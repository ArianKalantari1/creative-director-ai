# Persona discovery

**Status:** design proposal, not built. Written 2026-09-11 to be argued with.
**Supersedes:** the per-client taxonomy fix discussed under #15/#21 — that becomes one piece of this.

---

## The problem

`analysis/taxonomy.py` hardcodes eleven topics — `ATS`, `Resume`, `Interview`, `Visa` — as a
module constant, injected straight into the extraction prompt. It is not client-scoped.

Point a second client at a different niche and every extracted idea gets classified against
*resume and interview* categories. Almost everything lands in `General`, and three features
degrade at once: topic search returns one useless bucket, the content gap map loses its axis,
and Content Studio's grounding becomes effectively random.

Multi-client support (#11) scoped the **data** but not the **vocabulary**. That is the immediate
bug. Fixing only that would miss the more interesting thing underneath it.

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
and no amount of scraping produces it. It has to be elicited.

## Persona

The `clients` table is already most of a persona record — `name`, `niche`, `target_audience`,
`brand_voice_notes`, `notes`, plus its own competitor account list. It is a persona wearing the
wrong noun.

A persona is an identity you post *as*, on a platform, for a purpose. One person holds several:
a recruiter voice on LinkedIn, an AI-practitioner voice elsewhere. Each implies different
accounts to watch and a different vocabulary.

What is missing:

| Field | Why |
|---|---|
| `platform` | LinkedIn, TikTok, Instagram — format and convention differ |
| `goal` | authority · leads · community · job search. Changes which gaps are worth filling |
| **evidence inventory** | the first term of the intersection. See below |
| topics | per-persona rows, replacing the module constant |

### The evidence inventory is the load-bearing part

Not aspiration. Not a bio. **Things that happened**: what you built, what broke, what you get
asked repeatedly, what you find yourself explaining, what you learned by being confused.

Each proposed topic should be traceable to a specific piece of evidence, so the system can say
*"you can speak to this because X"* rather than *"this topic is underserved"*. A gap you cannot
credibly fill is not an opportunity.

This project already has a working instance of the pattern: `ari-facts.md` holds verified facts
with a DO NOT SAY list, and unverified rows block a claim from shipping. That is an evidence
inventory with a provenance gate. The generalisation is to make every persona have one.

### It accretes — it is not elicited in one sitting

**`ari-facts.md` was not written up front.** It accumulated from review: drafts generated,
then corrected — *"I would not say that"*, *"I would put it this way instead"*, *"that claim is
not mine to make"*. Approvals, rejections and rewrites, collected over many rounds, became the
file.

That changes the design. The interview is a **seed**, not the mechanism. The mechanism is the
loop:

```
propose  →  person reacts  →  correction is recorded  →  next proposal is closer
```

Identity is the fixed point that emerges, not an input collected at the start. Which is also
the honest account of how anyone finds a voice: not by introspection, but by saying things and
noticing which ones were not quite right.

Two consequences worth building for:

- **Corrections are the primary data, and they are richer than approvals.** *"I would rather
  say X"* carries the substitution, not just the rejection. Store the replacement alongside what
  it replaced, or the most useful signal is thrown away.
- **A persona is never finished, so there is no state where the interview is "done".** The UI
  should not present identity as a form to complete. It should present it as something that
  sharpens each time you correct it, and show that it is sharpening.

This partly answers open question 3 below: evidence is verified by accumulated review rather
than by an upfront gate. The gate is what stops an *unreviewed* claim shipping; the review is
what puts claims in the inventory in the first place.

## Flow

```
1. Interview     background · what you have built · what people ask you ·
                 what you explain repeatedly · what frustrated you enough to learn

2. Sample scrape a few hundred reels, not five thousand. Cheap, and enough
                 to see the shape of a niche

3. Propose       topics at the three-way intersection, each carrying:
                   - the evidence that gives you standing
                   - how much the niche discusses it
                   - who covers it and how thinly

4. React         confirm · reject · edit. Rejection is signal: it says something
                 about identity that the interview did not surface

5. Commit        topics get stable IDs. Full scrape now points somewhere
```

Sampling before committing costs a few dollars and **dissolves the intent-first versus
content-first question** rather than answering it — you get intent, then content, in that order.
The original framing treated a sequence as a fork.

## Reversibility

A taxonomy decision must not be a one-way door, because the whole premise is that people change
their minds once they see what is out there.

Two guarantees make changing direction cheap:

**Transcripts are retained and extraction runs from stored text.** Re-classifying does not
re-scrape or re-transcribe. At the project's documented GPT-4o-mini rate of ~$0.0003/reel,
re-extracting 5,000 reels costs roughly **$1.50** and one queue run. The expensive stages —
Apify and Deepgram — are untouched.

**Taxonomies are versioned, never overwritten.** Old assignments stay; new ones are added
alongside. Without this, switching destroys the historical comparison that makes a large corpus
worth having: you could no longer tell whether a gap closed or a label moved.

With both, intent-first is a draft rather than a commitment.

## Boundary with the existing content skills

`ari-content-strategist` reads `ari-context.md` (the spine, the reader, the pillars) and
`ari-facts.md` (verified facts, DO NOT SAY) and picks a concept. It **assumes the persona already
exists** — it cannot run for someone who has not written those files.

That is the clean split:

```
Senpai Reel  ──produces──>  persona + evidence + topics  ──consumed by──>  ari-content-strategist
              (who am I, what do I have,                     (what do I post
               what does this niche discuss)                  about it this week)
```

Discovery upstream, strategy downstream. **Do not build a second interviewer** without first
checking whether the strategist's questions can be reused — duplicated elicitation logic that
drifts apart is worse than either copy alone.

## What changes in the code

| Change | Note |
|---|---|
| Topics move from `analysis/taxonomy.py` constant to per-persona rows | seed Jobs-AU for the demo client |
| `taxonomy_versions` with stable topic IDs | the reversibility guarantee |
| Persona gains `platform`, `goal`, evidence inventory | extends `clients` |
| Content Studio topic dropdown reads persona topics | the hardcoded list disappears from the UI |
| Extraction prompt takes the persona's topics | currently imports the module constant |
| Orphan reporting | "312 units fit no topic; these clusters recur — promote?" |

`content_type` (`tip`, `warning`, `stat`, `myth`, `story`, `hook`, `cta`) stays **shared**. It
describes the shape of an idea, not its subject, and shape is niche-independent.

Orphan reporting is where the system tells you something you did not know — a subject the niche
discusses that you had not thought to name. It only works if existing topics hold still enough
to define what "does not fit" means, which is another argument for pinning.

## Open questions

1. **How much interview is too much?** Every question is friction before any value is shown.
   Is there a two-question version that gets 80% there?
2. **Does rejection get recorded?** Rejecting a proposed topic is information about identity.
   Storing rejections would improve later proposals — but it is also a surveillance-shaped
   feature, and personas are personal. Probably opt-in.
3. **How many rounds until a persona stabilises?** If identity is the fixed point of a
   correction loop, there is some number of review cycles after which proposals stop being
   corrected much. Nobody knows what it is. It is measurable — track correction rate per round —
   and worth measuring, because it tells a new user how much work is ahead of them before the
   tool earns its keep.
4. **Sample size for step 2.** A few hundred reels is a guess. Worth measuring at what point the
   proposed topics stop changing as more data arrives.
5. **Multiple personas, one corpus.** Two personas in overlapping niches may watch the same
   accounts. `client_posts` already models shared posts; do they share extractions too, or does
   each persona re-extract under its own vocabulary? Sharing is cheaper; separate is more honest.

## What this does not solve

It does not make the product sellable. The acquisition-rights blocker (#16, #20) is untouched —
this is about what the tool is *for*, not who may pay for it.

It does not replace judgement. The system proposes; the person decides. A tool that decided
identity on someone's behalf would be both wrong and unpleasant.

## Later, not now: learning the labels instead of assigning them

Topic assignment is currently one LLM call per transcript against a fixed vocabulary. That is
classification, and it is the simplest thing that works.

Once a persona has accumulated enough corrections, a different approach becomes available:
treat the accumulated approvals, rejections and substitutions as **weak supervision** — noisy,
cheap labels that a model learns from in aggregate rather than trusting individually. The
literature calls this programmatic or weak supervision; labelling functions vote, their
disagreements are modelled, and a label model resolves them.

Why it is interesting here: the corrections already exist as a by-product of normal use, so the
training signal is free. And it inverts the current relationship — instead of the person
conforming to a taxonomy, the taxonomy is learned from what the person keeps saying.

**Explicitly out of scope for this proposal**, for two reasons. It needs a corpus of corrections
that does not exist yet, and it is only worth doing once the simple version has proven the loop
is useful at all. Noted here so the data model does not foreclose it: storing corrections with
their substitutions, rather than a bare accept/reject flag, is what keeps that door open, and it
costs nothing to do now.

---

*Written to be disagreed with. The interview design (§Flow step 1) and the evidence-verification
question are the weakest parts and deserve the most argument.*
