# Source radar

**Status:** banked design sketch. Written from the 2026-09-15 conversation with Ari.
This is not implementation work.

## The idea

`senpai-reel` currently starts from market observation: collect competitor/social content,
transcribe it, extract message units, and use those units as evidence for grounded generation.

That answers one question well:

> What is the market already saying?

It does not answer a different question:

> What changed recently that this client should have a point of view on?

A source radar would collect source material such as papers, webpages, reports, news articles,
blog posts and official updates. It would summarise what is emerging, extract source-backed
claims, and turn those into content suggestions the operator can review.

The intended output is not "summarise this page and post it." The useful output is:

- what changed
- why it matters now
- who it matters for
- what the client can safely say
- what caveats must travel with the claim
- which content formats could carry the idea

This matters especially for articles, where source density and recency matter more than they do
in a short caption. It also helps short-form social by giving the client a timely angle before
the rest of the niche has copied it.

## How it fits the existing product

The clean architecture is four inputs feeding original content:

| Input | Question it answers |
|---|---|
| Competitor observations | What is the market already talking about, and where is coverage thin? |
| Knowledge layer | What durable claims can we cite with source and caveat attached? |
| Source radar | What timely source-backed topics are emerging now? |
| Client material and rulebook | What can this client credibly say, and how would they say it? |

The source radar should not replace the competitor corpus. It complements it. Competitor
content is evidence of market conversation; papers and webpages are evidence of source-backed
change.

## First useful version

Start with manual ingestion, not a crawler.

1. Add a PDF or webpage URL.
2. Extract text and basic metadata: title, author, publisher, URL, source type, publication
   date if available, collection date, and file hash for uploads.
3. Summarise the source in a structured way:
   - central claim
   - supporting claims
   - what is new or timely
   - who it matters for
   - limits, uncertainty and caveats
4. Generate content opportunities:
   - article angle
   - LinkedIn/post angle
   - carousel angle
   - short-form video angle
   - client question to ask before using it
5. Require human review before anything enters generation as usable context.

The first version can be boring on purpose: one PDF or one webpage in, a reviewed opportunity
brief out. That is enough to prove whether the workflow helps Ari think, before building any
ongoing monitoring.

## Data shape to consider

These are design concepts, not a schema decision.

### Source documents

One row per ingested source.

| Field | Meaning |
|---|---|
| `source_id` | Stable id for the source. |
| `source_type` | PDF, webpage, report, paper, official update, news article or other. |
| `url` | Canonical URL when available. |
| `file_hash` | Upload hash for PDFs or local files. |
| `title` | Source title, nullable when unknown. |
| `author` | Author or organisation, nullable when unknown. |
| `publisher` | Publisher, journal, company or site, nullable when unknown. |
| `published_at` | Publication date, nullable when unknown. Unknown must stay NULL, not "today." |
| `collected_at` | When Ari added or fetched it. |
| `rights_notes` | Retention or use notes, especially for uploaded PDFs and paywalled pages. |
| `extraction_status` | Whether text extraction succeeded, partially succeeded or failed. |

### Source claims

One row per claim or finding extracted from the source.

| Field | Meaning |
|---|---|
| `claim_id` | Stable id for the claim. |
| `source_id` | Source document it came from. |
| `claim` | The claim in our own words. |
| `evidence_type` | Finding, opinion, method, forecast, anecdote, benchmark or unknown. |
| `source_location` | Page, section, heading or paragraph anchor when available. |
| `caveat` | What must travel with the claim. |
| `review_status` | Draft, approved, rejected or needs research. |

### Content opportunities

One row per possible content direction created from reviewed source material.

| Field | Meaning |
|---|---|
| `opportunity_id` | Stable id for the suggestion. |
| `client_id` | Nullable. NULL means general opportunity, not yet client-specific. |
| `claim_ids` | Source-backed claims used by the opportunity. |
| `timeliness_reason` | Why this is worth saying now. |
| `audience_relevance` | Who should care and why. |
| `suggested_formats` | Article, post, carousel, short-form video or other. |
| `operator_note` | What Ari should check before using it. |
| `status` | Candidate, approved, used, parked or rejected. |

## Generation boundary

The source radar should feed generation only after review. Otherwise it becomes a fast way to
produce confident, source-shaped nonsense.

Each suggestion used for generation should carry:

- source document ids
- claim ids
- publication date or explicit unknown date
- caveats
- whether the claim is a finding, opinion, forecast or method
- any operator note about what the client can credibly say

The generation prompt should see the approved brief, not an unbounded scraped webpage. This
keeps the prompt smaller, protects citation discipline, and gives the reviewer one clear place
to reject or edit the interpretation.

## Rights and retention questions

This needs a rights pass before it becomes commercial infrastructure.

- Uploaded PDFs may be lawful to read but not lawful to retain, redistribute or use in a paid
  third-party service.
- Webpages may have terms that restrict automated collection or storage.
- Paywalled material needs special handling, and maybe exclusion.
- Long verbatim excerpts should not be stored or reused unless a rights position is clear.
- The client-facing output should cite sources without copying protected expression.

The product-safe posture is to store metadata, source locations, claims in our own words, and
short necessary snippets only when review says they are allowed. If full extracted text is kept,
that decision should be explicit rather than accidental.

## Evaluation

The first evaluation should be qualitative and cheap.

For a small batch of manually added sources, record:

- how many source summaries Ari accepts without major correction
- how many content opportunities are accepted, parked or rejected
- whether the opportunity says something timely that the competitor corpus did not already show
- whether the generated draft can preserve the source caveat
- whether the client voice/rulebook changes the angle materially

Do not mix this into #41's current generation A/B result. If source radar context becomes part
of generation, it deserves its own comparison: current generation versus generation with
reviewed source-radar opportunities.

## Later version

Only after manual ingestion proves useful, add monitoring:

- RSS feeds
- official blogs and documentation pages
- industry publications
- saved search queries
- paper/report uploads
- client-specific source lists

The monitoring layer should produce candidates, not publishable content. Most candidates being
rejected is healthy; it means the radar is broad and the operator remains the filter.

## Non-goals

- Not another competitor scraper.
- Not an autonomous publisher.
- Not a replacement for the knowledge layer.
- Not a claim that every webpage deserves to become content.
- Not a reason to loosen the source gate.
- Not implementation work yet.

## Open questions for research

- Which source types are commercially safe to retain and reuse?
- Should full extracted text be stored, or should the system store only metadata, locations and
  reviewed claims?
- What is the smallest metadata set needed to cite a webpage or PDF responsibly?
- Should general sources live globally and attach to clients later, or should every source be
  client-scoped from ingestion?
- What tools are reliable enough for PDF extraction, webpage readability and deduplication?
- How should the system handle conflicting sources on the same timely topic?
- What does "emerging" mean operationally: newly published, newly discussed, newly relevant to
  a client, or newly connected to a content gap?
