# Pre-invoice commercial rights checklist

**Issue:** #16 · **Status:** OPEN — no item below is resolved
**Blocks:** invoicing a client. Does not block building.

**Not legal advice.** This is a checklist to take *into* a conversation with an Australian
IP/commercial practitioner, not a substitute for one. Vendor terms and platform terms could
not be verified while writing this — `apify.com`, `docs.apify.com`, `developers.facebook.com`
and `help.instagram.com` are all blocked by this environment's egress proxy. Every item is a
question to answer, not a finding.

## Why this exists now

Everything to date has been research. `Research_Intent.md` frames the pipeline as local,
controlled, author-only and non-commercial — and that framing has been doing real work in
limiting exposure.

Charging clients ends it. Australian fair-dealing exceptions are purpose-specific; research
and study do not extend to commercial marketing production. The same pipeline, pointed at the
same data, sits in a different legal position the moment it is sold.

The risk is not that the work stops. It is that the question gets answered *after* the first
invoice rather than before, when the answer is cheap.

---

## 1. Vendor terms — Apify, or whichever source #15 selects

**Question:** do the terms permit using collected competitor data in a **paid service
delivered to third parties**?

Vendor terms commonly distinguish internal use from resale or client delivery. This is the
single most likely place to find a real problem, and the cheapest to check.

- [ ] Does the current plan permit commercial use?
- [ ] Does it permit *client delivery* specifically, not just internal analysis?
- [ ] Any restriction on retention, embeddings, or derived data? (The system retains
      transcripts, message units and 1536-dim embeddings indefinitely.)
- [ ] Does a different tier remove the restriction, and at what cost?

**If this fails, nothing else on this list matters.** Check it first.

## 2. Platform terms — Instagram

**Question:** does routing collection through a vendor discharge the obligation, or shift it?

Automated collection without permission is restricted regardless of who performs it. Using a
vendor changes who bears the obligation; it does not erase it.

- [ ] Confirm the position when collection is performed by a licensed third party
- [ ] Confirm whether an official API path (#15) changes the answer materially

## 3. What actually reaches the client — the live one

**Question:** can a competitor's distinctive wording appear in content a client publishes
under their own name?

**This is not hypothetical.** #17 documents the open path in the current code: extraction is
instructed to capture competitor phrasing verbatim, the generation prompt prefers that
verbatim field over the abstracted one, and no generation prompt contains any instruction
against reusing it.

- [ ] Run `tools/audit_reference_leakage.py` against the real database for a measured rate
- [ ] Implement the #17 fix (prefer `claim`, add non-reuse instruction, add the n-gram gate)
- [ ] Re-run the audit and confirm it comes back clean
- [ ] Keep the audit as a test so it cannot regress silently

Of everything on this list, this is the item with a live exposure and a cheap fix. Do it
regardless of how the legal conversation goes.

## 4. Claims made to clients

**Question:** does the marketing describe what the system actually knows?

The system observes engagement on competitor posts. It does **not** observe ROAS, CPA,
conversion rate, or revenue — that is precisely why the v2 report replaced the Creative
Confidence Score with an evidence panel.

- [ ] No language implying outputs are based on "proven", "winning" or "high-converting" content
- [ ] No implied guarantee of performance
- [ ] Evidence shown to clients is described as observed engagement, with its limits stated

Overstating this is a consumer-law problem independent of copyright, and it is entirely
self-inflicted.

## 5. Client data handling

**Question:** what is stored, where, for how long, and what happens when they leave?

- [ ] Written answer for each of the above
- [ ] Deletion actually deletes — `delete_client()` currently leaves dangling references on
      `transcripts` and `message_units` (raised on senpai-reel PR #4)
- [ ] Position on shared posts: a post retained for another client is not deleted when one
      client leaves. Correct behaviour, but a client asking "delete my data" should be
      answered accurately about what is and is not removed
- [ ] Confirm whether anything collected constitutes personal information under the
      Privacy Act, given the corpus contains identifiable individuals in video

---

## Recording answers

Answer inline above with a date and source. Where an answer is "no", record the product
change it forces rather than leaving the box unticked — an unresolved item that has been
thought about is very different from one that was never opened, and six weeks from now
nobody will remember which was which.
