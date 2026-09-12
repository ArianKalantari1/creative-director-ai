# What this is for

**Status:** the business, in plain language. Written 2026-09-12 from the founder's own
account of it. Not a design doc and not a plan — the thing the plans are supposed to serve.

Everything else in this repo is downstream of this. If a ticket does not eventually serve
what is written here, it is probably the wrong ticket.

---

## The business

Small businesses mostly do not have a marketing team. They have an owner who knows their
trade and has neither the time nor the training to run content.

The offer is to *become* that function. Not an agency retainer with a deck — one person
holding the marketing for a set of clients, with a monthly conversation per client to set
direction, and a system that carries execution in between.

**The system is what makes this possible at more than two clients.** Each client has their
own voice, their own calendar, their own audience, their own competitor set, their own
targets. A person cannot hold ten of those in their head at once. Software can.

That is the whole reason the tooling exists. It is not a product in search of users; it is
the leverage that lets one operator serve many.

## What it covers

**Marketing is not video.** Video is one output.

- Written posts and articles
- Carousels and static images
- Video — client-supplied footage, and generated footage where the shot cannot be filmed
- Where each of those should be published, and when

The mix is the point. A professional trying to gain traction on LinkedIn needs posts *and*
carousels *and* video, because the platform rewards different formats at different moments.
A tool that only does one of those solves a fraction of the problem.

## Where the material comes from

Three inputs, all of which already exist in some form:

**Market and competitor signal.** What is actually being discussed in a client's niche right
now, and which of it is covered thinly. This is what `senpai-reel` does — scrape, transcribe,
extract ideas, find the gaps.

**A news radar.** Articles and developments collected continuously, drafted into suggestions,
reviewed by a human who decides what is worth saying anything about. Most suggestions are
rejected. That is the system working, not failing.

**The client's own material.** Their website, their existing posts, footage they send over,
and what they say in the monthly conversation.

## How it gets better

The output is corrected, and the corrections are kept.

Not stored raw — **distilled into rules**. A draft comes back rewritten, with a reason
attached ("I would not say that, it sounds like it is blaming someone"), and what persists is
the generalised rule rather than the pair of drafts. The rulebook grows; the drafts are
discarded.

Two things this produces that are easy to miss:

- **The corrections teach it how to work with the person, not only how to sound like them.**
  A complaint that a draft was written without asking the goal is not a voice note — it is a
  missing step in the process, and the process should change.
- **Reacting to a draft is a far easier task than self-description.** Nobody can answer "who
  are you" cold. Almost anyone can look at a paragraph written in their name and say "no, not
  like that." That is why this can work for a client who does not yet know their own niche.

## What already exists

Two halves, built separately, not yet joined.

**`senpai-reel`** is the multi-client data layer — client-scoped posts, per-persona
taxonomies, per-client cost tracking, the acquisition seam. Built; never yet run against a
real corpus by its owner.

**The `ari-*` skills** are a working content OS — radar, strategy, source gate, voice,
carousel. In daily use, versioned, improved by exactly the correction loop described above.
Hardcoded to a single client: the founder.

The gap between them is the product. Generalising the skills from one client to many is a
clearer statement of the work than "build persona discovery".

## Client zero

The founder is the first client, and the pilot runs on him before it runs on anyone paying.

**Who.** Recently completed a Master of Data Science in Sydney. Four years in delivery and
project management at an agency before that. Currently also works as a bartender. Fluent in
Japanese.

**What he talks about.** How small businesses actually use AI — what helps, what does not,
and what people are really doing with Claude, Codex and similar tools to make operations
easier. Marketing as a discipline, because he is building a marketing product. And this
product, built in public.

**The angle that is his.** Most writing about AI for small business is produced by people who
have never worked in one. He does the data and AI work, he has run client delivery, and he
works shifts in a small business where he can see the operations from the inside. That
intersection is not a positioning exercise — it is a vantage point almost nobody else has.

The Japanese-to-Australian angle is the same shape: the useful knowledge sits in the gap
between two systems that only someone standing in both can see. Neither gap shows up in a
market analysis, because the gap is not in the market.

## Constraints that are already settled

**Apify cannot be used in a paid service.** Confirmed against their terms. The product is not
sellable on the current acquisition stack, and replacing that source is the gate between
research and revenue. Not a blocker on building — a blocker on charging.

**Research use is the current frame.** Competitor material is evidence, never source
material. Downloaded video is retained for research, by a deliberate decision.

## Open, and deliberately not decided

- Whether video may be retained once this is commercial, and what the minimum keepable
  artefact is if not. Needs platform terms nobody has read yet. See #16.
- What the acquisition source becomes. See #20.
- Whether the correction loop holds up for someone less articulate about their own voice than
  client zero. Testable, untested.

## The nearest real risk

None of the above matters if the extracted ideas are not good enough that a client would pay
for them. That has never been checked against a real scrape. It is cheap to check. It should
be checked before anything else here is built on.
