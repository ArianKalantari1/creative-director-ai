# STATUS — read this first

**Purpose:** so a new chat, a new Codex session, or Ari after a week away can pick up
without re-deriving anything.

**Last substantive update:** 2026-09-13

---

## The one rule that keeps this useful

**Do not mirror what GitHub already tracks.** PR numbers, CI status and issue states
live in GitHub and change hourly; copying them here guarantees this file starts lying
within a day, and a lying status file is worse than none.

This file holds what GitHub *cannot* hold: direction, decisions, the difference between
settled and open, and what we already got wrong.

**Update it when a decision changes, not on a schedule.** If nothing was decided, there
is nothing to update.

---

## What this is

One operator acting as the **marketing function** for small businesses that have no
marketing team. A monthly direction meeting per client; the system carries execution
between meetings. That is what lets one person hold ten clients instead of two.

**Marketing is not video.** Video is one output. Written posts, articles, carousels,
images and client-supplied footage all count. A system that only makes reels is a video
tool, not a marketing function.

Full version: [`docs/vision.md`](docs/vision.md).

### The three layers

| Layer | What it is | State |
|---|---|---|
| **Observation** | scraped competitor content | built — 1,428 posts, 5,222 message units, 17 accounts |
| **Technique** | hooks, CTAs, structure, directness | **now separable** — `unit_role` shipped; the library itself is not built |
| **Knowledge** | marketing theory, verified against sources | started — 12 claims in `docs/knowledge/` |

Generation should draw on all three. It currently draws on one, which is why output
reads like a remix of the corpus.

---

## Requirements agreed

These are the constraints any proposal has to satisfy. They are not aspirations.

1. **Competitor material is evidence, never source material.** The strategy-abstraction
   firewall. Patterns are reusable; wording is not. `tools/audit_reference_leakage.py`
   exists to catch violations, and generation now excludes `technique` units at source.

2. **Absent is not zero.** A missing value stays NULL. It has bitten this project four
   times; see `senpai-reel/AGENTS.md`.

3. **Every claim that reaches a post carries its source.** `ari-source-gate` blocks
   uncited claims, so anything without `claim → source → date` is dead weight
   downstream. This is why the knowledge layer is a table and not notes.

4. **Reversibility.** Taxonomies are versioned, transcripts retained, re-extraction
   costs ~$1.50 per 5,000 reels. Decisions about classification must stay cheap to undo.

5. **Client isolation is the foundation.** A regression there is the one that costs a
   client relationship.

6. **Verify, don't assert.** Numbers get checked against the thing they describe. Both
   repos have had "N passed" reported by someone with no way to check it; CI exists
   because of that.

---

## Settled / Open

Keeping these apart is not pedantry — treating an open question as settled has already
cost a round here.

### Settled

- **Apify cannot be used in a paid service.** Confirmed against their terms. Not a
  blocker on building; a blocker on charging.
- **Downloaded competitor video is retained for research**, personal use, decided
  knowingly.
- **Dead code that is subtly wrong gets archived, not fixed** (#30 resolved this way).

### Open — do not write these down as decided

- **Whether video may be retained once this is commercial.** Needs platform terms
  nobody has read. See #16.
- **What replaces Apify as the acquisition source.** See #20.
- **Whether Symphony output can be published off TikTok.** The controlling terms are
  behind a sign-in. See #28.
- **Whether Symphony custom avatars are portable across clients.** Documentation is
  silent in both directions. See #28.
- **Whether the extracted ideas are good enough that a client would pay.** Partially
  answered — see Evidence below — but not closed.

---

## Evidence we actually have

Measured, not assumed. Everything here came from running something against the real
corpus.

| Question | Answer | How |
|---|---|---|
| Does competitor wording leak into claims? | **No — 1.4%** (74/5,222), 100% transcript coverage | `score_extraction.py score` |
| How much of the corpus is technique, not subject? | **11.2%** (583 units: 343 cta + 240 hook) | `classify_unit_roles.py --dry-run` |
| What does the model pass cost? | **$0.19** for 4,638 units | same |
| Are the extracted claims faithful? | **20% wrong** on a 40-unit hand-marked sample | `blind` + hand marking |
| Does the mechanical scorer track human judgement? | **No — 57%.** Do not trust its corpus numbers | `compare` |

**The single most useful finding:** 7 of the 8 "wrong" verdicts were one failure — the
extractor treating performed sentences (satire, scripted demos, quoted bad practice,
debate montages) as sincere assertions. Fixed in the prompt, not filtered downstream.

---

## Who does what

**Ari** — direction, client relationships, anything needing the real database or a
logged-in account. Marks samples. Supplies marketing material. The only one who can
judge whether output is worth selling.

**Claude** — architecture, review, the things that need judgement about trade-offs.
Writes tickets for Codex. Reviews Codex's PRs by running them, not reading them.

**Codex** — well-specified slices with a written ticket. Works best when the issue
states constraints and what "done" means. Has produced work better than what it
replaced; say so when it does.

Standing conventions: `senpai-reel/AGENTS.md` (building) and `senpai-reel/CLAUDE.md`
(reviewing). This file does not repeat them.

---

## What we got wrong

Kept so it is not repeated. Each cost a round.

- **Claimed tests passed when the harness was broken** — twice. Both caught because the
  result looked too convenient. If a result is surprisingly clean, suspect the harness.
- **Treated an open question as settled** (video retention). Corrected on #16.
- **Designed the correction loop backwards** — proposed topic-approval when the real
  mechanism is draft-diffing into distilled rules. See `docs/design/persona-discovery.md`.
- **Stated a secondary source as a finding** (Symphony avatars locking to an ad account).
  Not in TikTok's documentation. Corrected on #28.
- **Shipped a tool that could not run on a real database** — `--dry-run` opened it
  read-only and then needed columns only a migration could add. CI was green because
  every test built a fresh database. **A green suite is not evidence the thing works.**
- **A SQL precedence bug crossed client boundaries** — `A OR B AND client_id = ?` binds
  as `A OR (B AND ...)`. Caught by a test, not by reading.

---

## Live state

Deliberately short, because GitHub is the source of truth.

- **`senpai-reel`** — the application. `main` green, 323 tests.
- **`creative-director-ai`** — planning, issues, design docs, knowledge layer.
- Open work: see [issues](https://github.com/ArianKalantari1/creative-director-ai/issues)
  and open PRs in both repos.
- Paused: Phase A (#1–#7), Creative DNA. Banked: #20, #28.

**Biggest single gap right now:** the full *How B2B Brands Grow* report (C5 in the
knowledge layer is front matter only), now reachable via Macquarie alumni library access.
