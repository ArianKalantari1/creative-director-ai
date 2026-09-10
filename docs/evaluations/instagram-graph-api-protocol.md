# Instagram Graph API evaluation protocol

**Issue:** #15 · **Status:** UNVERIFIED — this is a test plan, not a set of answers
**Why unverified:** `developers.facebook.com` and `help.instagram.com` are blocked by the
egress proxy in the environment where this was written. Nothing below was confirmed against
Meta's documentation. **Do not treat any expectation here as fact** — the point of the document
is to make the checks cheap to run, not to pre-empt them.

## What is being decided

Apify is the only meaningful recurring cost in the stack. Everything else runs at roughly a
third of a cent per reel. The question is whether Instagram's official **Business Discovery**
endpoint can replace it, in whole or in part.

This is not only a cost question. An official API used within its terms is a materially
better rights position for a paid product than a third-party scraper — see #16.

## Run these in order. Stop early if an earlier one fails.

### Check 1 — Can you reach competitor accounts at all?

Business Discovery requires a Facebook Business account plus an Instagram Professional
account you control, and queries other accounts **by username**.

Take the 23 seed handles in `senpai-reel/collection/account_list.py` and, for each, record:

```
handle | resolves? | account type (business / creator / personal / not found)
```

**Decision rule:** if a meaningful share of a typical client's competitors are personal
accounts, Business Discovery cannot be the primary source regardless of how good its fields
are. Record the actual fraction — do not estimate it.

### Check 2 — Which fields come back? (the critical one)

For a handle that resolves, request the media edge and record exactly which fields are
populated, and which are absent or zero.

The fields that matter for this product:

| Field | Used for | If missing |
|---|---|---|
| `caption` | hashtags, message-unit extraction | severe — extraction depends on it |
| `timestamp` | posting-time features | minor |
| `like_count` | engagement | recoverable |
| `comments_count` | engagement | recoverable |
| `media_url` | download → audio → transcription | **fatal** — the whole pipeline needs the media |
| **view/play count** | **view-normalised engagement** | **see below** |

**The view-count question is the one that decides this.** For accounts the caller does not
own, view or play counts may not be exposed. If they are not:

- raw likes mostly measure audience size, not creative quality
- view-normalised engagement is the better label and would be unavailable
- the Phase A experiment (#1) loses its strongest outcome metric

That may still be an acceptable trade for the cost and rights improvement — but it is a real
cost on the other side of the ledger, and it should be weighed explicitly rather than
discovered later.

Record the raw API response for one media item verbatim in the results file. Field
availability is the entire decision and should not be summarised.

### Check 3 — Does `media_url` stay valid long enough to download?

The existing Apify path has a documented failure mode: CDN URLs expire in roughly 24–48
hours, so scraping without downloading promptly loses the media.

Fetch a `media_url`, then retry the same URL at +1h, +6h, +24h and record when it stops
working. **Decision rule:** if it expires faster than Apify's, the pipeline must download
inline with fetching rather than queueing, which changes #12's dashboard design.

### Check 4 — Rate limits at realistic volume

A client has ~20 competitor accounts. A refresh reads recent media for all of them.

Measure how many accounts can be refreshed before throttling, and how long a full refresh
takes. **Decision rule:** if a single client's refresh cannot complete within a normal working
session, this is not viable as the primary source no matter what it costs.

### Check 5 — Commercial use

Confirm the terms permit use in a **paid service delivered to third parties**, not merely
personal or internal use. This is #16's question; note the answer here and cross-reference.

## Recording the outcome

Write results to `docs/evaluations/instagram-graph-api-results.md` with the date, the app
and account type used, and the raw response from Check 2. Then pick one:

| Outcome | Decision |
|---|---|
| Checks 1–4 pass, views available | Primary source. Apify becomes fallback. |
| Passes but no view counts | Hybrid: Graph API for metadata, keep Apify where views matter. Record which field came from where — see the note below. |
| Check 1 or 4 fails | Stay on Apify. Re-run when the account mix changes. |
| Check 5 fails | Stop. No cost saving justifies it. |

## Design constraint regardless of outcome

The adapter in #15 must mark unavailable fields as **unavailable**, never default them to
zero. A missing view count silently becoming `views = 0` would corrupt every engagement
calculation downstream, and would do it invisibly — the numbers would look fine.

This matters most in the hybrid outcome, where the same column is populated from two sources
with different coverage. Every record should carry which source produced it.
