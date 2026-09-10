# Creative Director AI: Marketing Intelligence and Creative OS for Agencies Serving SMBs

Revision v2, created on branch `codex/research-revision-v2`.

This document revises `research/2026-09-marketing-intelligence-creative-os.md` in response to Claude's review in `research/reviews/2026-09-10-claude-review.md`. The original report is intentionally left untouched.

All public sources in this report were accessed on 2026-09-11, unless otherwise noted. Source IDs such as `[S1]` resolve to real URLs in the source register at the end of the document. This revision deliberately removes all opaque session-local citation handles from the original report.

This is engineering and product research, not legal advice. Copyright, platform-terms, advertising-compliance, scraping, likeness, voice, music, and data-retention positions require review by qualified counsel before becoming customer-facing claims or binding product policy.

## Executive Summary

The strongest product is not "AI ad cloning." The stronger product is a marketing intelligence and creative operating system that helps an agency understand the creative patterns repeatedly appearing in a market, convert those patterns into safe strategy abstractions, and produce original brand-specific creative from owned, licensed, or newly generated assets.

The recommended wedge is **agency-first**, specifically performance creative agencies and small growth teams serving multiple SMB clients. Direct SMB self-service can come later, but the first buyer should be someone who already has recurring creative workflow pain, multiple client contexts, and enough budget to value evidence, provenance, and safety.

The product hierarchy should be:

```text
Market evidence
-> Creative DNA
-> Brand DNA
-> Safe Strategy
-> Editable Blueprint
-> Rights-checked render
-> Authorised performance feedback
```

The immediate validation environment should be `Instagram_Reel_Analysis_App`, not the paid-ad product. Organic Reels can provide public or authorised engagement signals much earlier than competitor paid ads can provide conversion outcomes. The core research question becomes:

```text
Do structured Creative DNA features predict engagement better than baseline metadata?
```

If the answer is no, the ad-product moat is probably weaker than the original report implied. If the answer is yes, the Reel app becomes the cheapest proof that Creative DNA is worth productising.

The biggest changes from the original report are:

- Replace opaque citations with a source register containing real URLs and access dates.
- Choose an agency-first buyer instead of saying "SMBs" generically.
- Treat the Reel app as the validation spike and shared extraction stack.
- Make the abstraction firewall an enforced boundary, not a prompt convention.
- Remove the hand-weighted Creative Confidence Score from v1.
- Remove numeric copyright/similarity thresholds from v1.
- Add an explicit per-video cost model.
- Re-plan around one vertical slice and a human extraction-quality gate.
- Defer OpenTimelineIO until there is an actual NLE interchange requirement.

## 1. Market Positioning

The competitor landscape already contains products that generate ads from product URLs, AI actors, scripts, creative workflows, and ad inspiration. Creatify positions itself around automated marketing-video generation and exposes developer-facing API documentation for video ad generation workflows [S1, S2]. Arcads positions itself as an AI UGC/video-ad platform with AI actors and workflow tooling [S3]. Foreplay positions itself around saving, organising, and analysing ads for creative strategy workflows used by performance teams and agencies [S4]. TwelveLabs offers managed video-understanding primitives that can return timestamped structured data from video [S5].

That means the durable differentiation is not "we can make a video." It is:

- Evidence quality: the system says why a strategy is worth testing.
- Provenance: every recommendation traces to source type, rights basis, and extraction lineage.
- Abstraction: competitor creative is evidence, not production source material.
- Production safety: the generator cannot access competitor expression.
- Brand fit: outputs use client-owned claims, proof, assets, and voice.
- Learning loop: authorised client performance data eventually fits the model.

The user-facing promise should be:

```text
Show me the creative strategies my market keeps returning to,
explain the evidence,
identify underused whitespace,
and build original brand-specific concepts I can safely brief, edit, approve, and test.
```

Do not use "winning ad" language for public competitor research. Public ad libraries and commercial ad-intelligence tools can expose presence, copy, creative, date ranges, and in some jurisdictions transparency signals, but they generally do not expose ordinary competitor ROAS, CPA, or margin. Google Ads API reporting and TikTok Business API reporting are useful for authorised first-party accounts, not for proving a competitor's unit economics [S6, S7].

## 2. Buyer And Wedge

The buyer for v1 should be a **performance creative agency serving SMB clients**.

This is a sharper buyer than "SMB" for several reasons:

| Dimension | Agency-first implication |
|---|---|
| Pain | Agencies repeatedly research competitors, build briefs, produce variants, defend recommendations, and report results. |
| Budget | Agencies can justify software that reduces research and briefing hours across several clients. |
| Data | Agencies may connect multiple authorised ad accounts over time, creating the outcome loop the product eventually needs. |
| Safety | Agencies benefit from visible claims, rights, and originality controls because they need client approval and professional accountability. |
| Workflow | Agencies already have review, approval, client feedback, asset collection, and reporting loops. |

Direct SMB self-service is still a possible later segment, but it should not set the first product surface. A self-service SMB onboarding flow tends to demand a polished editor, broad automation, and low-touch support before the evidence layer is proven. An agency pilot can tolerate a narrower product if the strategic output is strong.

At five customers, there is no meaningful data moat yet. The early moat must be:

- faster research-to-brief turnaround;
- better structured creative evidence than a swipe file;
- safer reuse boundaries than "clone this reference";
- reusable Brand DNA, claims, and asset-rights records;
- a reviewable process agencies can show clients.

The later moat is a cross-client dataset that connects Brand DNA, Creative DNA, audience/context, media spend, and outcomes. That moat only exists after enough authorised performance data accumulates.

## 3. Relationship To `Instagram_Reel_Analysis_App`

This section is the most important addition to v2. I could not locate or inspect the actual `Instagram_Reel_Analysis_App` repository from this workspace or the connected GitHub search. This section therefore relies on the language Claude quoted from `Instagram_Reel_Analysis_App/Research_Intent.md`, especially that its acquisition path is framed as local, controlled research and not as a ready-to-use commercial scraping tool.

The Reel app is not merely adjacent to Creative Director AI. It is the practical validation spike.

Paid competitor ads have a structural measurement problem: without authorised access to the advertiser's ad account, the system cannot observe ROAS, CPA, conversion rate, gross margin, or true business quality. It can observe proxies such as longevity, relaunches, variant count, cross-platform recurrence, and number of distinct advertisers using a pattern. Those are useful market signals, but they are still proxies.

Organic Reels are different. Public and authorised Instagram surfaces can expose engagement signals such as likes, comments, views/plays, saves, shares, reach, or related media insights depending on account type, API permission, and endpoint. Meta's Instagram Business Discovery and Media Insights documentation shows that official APIs can expose basic media metrics and insights in authorised/professional-account contexts [S8, S9]. The public UI may show additional visible engagement signals, but unauthorised automated collection must be treated as a platform-terms and research-ethics issue, not as a commercial default [S10].

The core validation experiment should be:

```text
Question:
  Do Creative DNA features explain or predict Reel engagement better than
  baseline surface metadata?

Baseline features:
  account size, post age, duration, posting time, caption length,
  hashtag count, audio type, format, topic label

Creative DNA features:
  hook family, first-claim timing, time-to-payoff, scene count,
  shot-length distribution, OCR density, direct-to-camera ratio,
  proof mechanism, loop structure, narrative function sequence,
  emotional arc, CTA presence

Outcome labels:
  engagement rate, view-normalised engagement, comment rate,
  save/share proxy where authorised, and relative performance within account

Success criterion:
  A held-out model using Creative DNA features beats the baseline by a
  pre-registered margin, and the winning features are interpretable enough
  for a human creative strategist to trust.
```

This is the cheapest falsification path for the whole company. If Creative DNA does not improve prediction in organic short-form video, then the ad product should be scaled back to workflow automation and research organisation. If it does improve prediction, then Creative DNA is worth productising into the paid-ad environment where true outcome labels arrive later through client-authorised ad accounts.

### Shared Package Boundary

The two projects should share a package, not copied code:

```text
creative-dna-core
  schemas/
    creative_dna.schema.json
    creative_scene.schema.json
    extraction_lineage.schema.json
  extractors/
    ffprobe_adapter
    scene_detect_adapter
    asr_alignment_adapter
    ocr_timeline_adapter
  evaluators/
    human_review_packet
    extraction_quality_metrics
  docs/
    schema_versioning.md
```

`Instagram_Reel_Analysis_App` should own:

- local research acquisition;
- experiment datasets;
- notebooks or scripts for engagement modelling;
- research-only data governance;
- labels and feature-importance analysis for organic Reels.

`creative-director-ai` should own:

- rights-gated commercial acquisition;
- Brand DNA;
- claims and proof ledger;
- client asset library;
- Safe Strategy;
- Blueprint;
- production rendering and approval flows.

The shared line is **analysis**, not acquisition. Acquisition must remain separate because the rights posture differs.

### Rights Boundary Between Reels Research And Commercial SaaS

The Reel app's research-only framing should not be reused as a commercial ingestion justification. If a local research pipeline uses downloader-style acquisition, that pipeline should stay in the research repo and should not feed a paid SaaS. Instagram's terms prohibit automated collection without permission [S10], and YouTube's terms and API services terms likewise distinguish authorised service/API use from arbitrary downloading or automated access [S11, S12].

Creative Director AI should accept reference creative through:

- client-owned uploads;
- client-authorised account connections;
- official APIs used within their terms;
- licensed stock/media libraries;
- commercial ad-data vendors with contracts that permit analysis, retention, embeddings, and derived data;
- generated assets whose provider terms permit the intended commercial use.

It should not commercialise:

```text
public URL -> downloader succeeds -> therefore production ingestion is permitted
```

The safest architecture is two services with different credentials and different data stores:

```text
Reels research acquisition service
  -> research-only raw media store
  -> creative-dna-core extraction
  -> research dataset

Creative Director AI acquisition service
  -> rights-gated media store
  -> creative-dna-core extraction
  -> commercial provenance store
```

A boolean flag is not enough. Separate services make the boundary reviewable.

## 4. Data Model

The system should distinguish observations from interpretations.

An observation is extracted evidence:

```text
OCR detected "$49" from 02.1s to 04.8s at lower-centre bounding box.
```

An interpretation is a model or human judgement:

```text
This scene uses a low-friction price anchor.
```

Both are useful, but they must be stored separately. Better models can reinterpret old observations later without reprocessing every frame.

### Creative DNA

| Group | Fields | Notes |
|---|---|---|
| Identity/provenance | `creative_id`, `platform`, `source_uri`, `capture_method`, `rights_basis`, `retention_policy`, `accessed_at` | Required before any source material enters the system. |
| Technical media | `duration_ms`, `fps`, `width`, `height`, `aspect_ratio`, `codec`, `audio_present` | FFmpeg/ffprobe are the default primitives; build flags need licence tracking [S13]. |
| Transcript evidence | word spans, sentence spans, confidence, speaker labels where appropriate | WhisperX is a strong candidate for word-level alignment; diarisation/model terms still need diligence. |
| Scene evidence | scene boundaries, shot-duration stats, transition type, keyframe refs | PySceneDetect is the local default; v0.7 was released in May 2026 and v0.7.1 in July 2026 [S14]. |
| OCR timeline | text, bounding boxes, role, confidence, first/last visible time | PaddleOCR is mature and Apache-2.0 according to GitHub metadata. |
| Visual motifs | object labels, logo/text presence, screen-time, salience | GroundingDINO/SAM 2 are optional after the first validation slice. |
| Messaging interpretation | hook family, proof mechanism, objection, CTA, claim type | Domain-specific extensions differ for paid ads and organic Reels. |
| Pacing | cuts/minute, median shot length, time-to-hook, time-to-proof, time-to-CTA | Shared directly across Reels and paid ads. |
| Performance evidence | public proxy signals, authorised campaign metrics, organic engagement metrics | Keep public proxies separate from client-owned outcomes. |
| Embeddings/fingerprints | transcript embeddings, visual embeddings, perceptual hashes, audio fingerprints | Restricted derived data with explicit rights basis and retention policy. |
| Extraction lineage | model/tool name, version/commit, prompt/schema version, timestamp, reviewer status | Required for reproducibility. |

The paid-ad interpretation vocabulary should include:

```text
offer, CTA, urgency, objection, proof, claim, guarantee,
mechanism, price anchor, social proof, founder proof,
product demonstration, authority, comparison
```

The Reel interpretation vocabulary should include:

```text
retention hook, loop, payoff timing, curiosity gap,
comment trigger, save trigger, trend participation,
topic promise, watch-to-end device, identity cue
```

Use a shared scene container with domain-specific extensions rather than a single giant schema full of null fields.

### Brand DNA

Brand DNA should be the production source of truth:

| Group | Fields | Product behaviour |
|---|---|---|
| Identity | legal name, trading name, URLs, locations, markets | Grounds all generated claims and assets. |
| Products/offers | services, price models, availability, margin bands | Prevents obsolete or low-priority offers from being advertised. |
| Audience | ICP, pains, jobs-to-be-done, buying stage, objections | Makes concepts segment-specific. |
| Positioning | category, differentiators, reasons to believe | Adapts market patterns to the actual business. |
| Claims ledger | claim, allowed wording, evidence, expiry, jurisdiction, qualifier | Blocks unsupported claims. |
| Proof library | testimonials, certifications, case studies, metrics, consent | Supplies real proof instead of invented proof. |
| Visual system | logos, colours, fonts, motion rules, safe areas | Feeds deterministic rendering. |
| Asset library | media, subjects, rights, talent release, term/geography limits | Ensures production uses allowed assets. |
| Channel rules | platform, aspect ratio, duration range, CTA rules | Turns publishing requirements into constraints. |
| Performance memory | spend, audience, placement, campaign lineage, outcomes | Comes only from authorised client accounts [S6, S7]. |

Uncertain web-extracted facts should be presented for owner or account-manager confirmation before they can enter the claims ledger.

### Blueprint

The internal Blueprint should be the single MVP representation. Do not make OpenTimelineIO a required dependency until a customer or workflow actually needs Premiere, Resolve, Avid, or similar interchange. OpenTimelineIO remains useful as a future export/import adapter because it is built for editorial timeline interchange, but the Remotion-oriented product can start with a constrained JSON Blueprint [S15].

## 5. Enforced Strategy-Abstraction Firewall

The original report had the right idea but specified too much of the firewall as convention. In v1, make the firewall a code boundary.

### Required Boundary

```text
Restricted Analysis Environment
  raw competitor media
  transcripts
  OCR text
  source frames
  audio fingerprints
  Creative DNA observations
  analyst model context

        |
        | only SafeStrategy JSON may cross
        v

Production Environment
  Brand DNA
  claims ledger
  client-owned/licensed/generated assets
  Blueprint generator
  renderer
```

The analyst and producer must be separate model invocations with no shared conversation state. The producer receives only validated `SafeStrategy` JSON, Brand DNA, and allowed assets. Tool permissions should enforce this: the producer has no `get_reference_frames`, no `get_reference_audio`, no raw transcript access, and no OCR source text access.

### Closed SafeStrategy Schema

`SafeStrategy` should be mostly enumerated and bounded:

```json
{
  "schema_version": "safe_strategy.v1",
  "source_support": {
    "distinct_advertisers": 8,
    "creative_families": 3,
    "platforms": ["instagram_reels", "meta_ads"],
    "source_entropy_band": "high",
    "single_advertiser_pattern": false
  },
  "hook_family": "cost_of_inaction",
  "audience_problem_class": "lost_revenue_from_delay",
  "narrative_functions": [
    "hook",
    "problem",
    "mechanism",
    "proof",
    "cta"
  ],
  "proof_mechanism": "client_owned_demonstration",
  "offer_mechanism": "low_friction_audit",
  "pacing_band": "medium_fast",
  "text_density_band": "medium",
  "visual_structure_family": "talking_head_plus_demonstration",
  "producer_constraints": {
    "must_use_brand_claims_ledger": true,
    "must_use_allowed_assets_only": true,
    "must_not_reference_competitors": true
  }
}
```

Allowed examples:

```text
hook_family = cost_of_inaction | myth_bust | before_after | founder_warning
narrative_function = hook | problem | mechanism | proof | offer | cta | payoff
proof_mechanism = demonstration | testimonial | third_party_review | metric | authority
pacing_band = slow | medium | medium_fast | fast
source_entropy_band = low | medium | high
```

Every free-text field must be justified. If a field is needed, it should be produced after the firewall from Brand DNA, not copied from competitor analysis.

### Leak Tests At The Firewall

Run cheap leak checks before generation:

```python
def validate_safe_strategy(strategy_json, source_texts, source_ocr):
    safe_text = normalise_text(json.dumps(strategy_json, sort_keys=True))
    source_text = normalise_text("\\n".join(source_texts + source_ocr))

    if exact_ngram_overlap(safe_text, source_text, n=5):
        return reject("5-token source n-gram crossed firewall")

    if rare_phrase_overlap(safe_text, source_text):
        return reject("rare source phrase crossed firewall")

    if contains_distinctive_slogan(safe_text):
        return reject("slogan-like expression crossed firewall")

    if strategy_json["source_support"]["single_advertiser_pattern"]:
        return reject("single-advertiser pattern cannot cross")

    if strategy_json["source_support"]["source_entropy_band"] == "low":
        return reject("low-entropy source pattern cannot cross")

    return accept()
```

The source-entropy idea should be mandatory. A pattern observed in only one advertiser does not cross the firewall. A pattern distributed across independent advertisers may cross only as a bounded taxonomy value, not as a prose description lifted from source material.

Post-render similarity scanning still matters, but it should be the backstop, not the first place the system discovers a leak.

## 6. Evidence Panel, Not Creative Confidence Score

Cut the hand-weighted composite Creative Confidence Score from v1.

The score in the original report looked rigorous while admitting its weights were heuristics. That is the wrong tradeoff. It is safer and more useful to ship the evidence itself:

```text
CREATIVE STRATEGY
Specific pain -> quick demonstration -> proof -> low-friction CTA

Market evidence
  Observed across 11 distinct advertisers
  37 related variants
  Median observed persistence: 74 days
  8 relaunch events
  Seen on 3 platforms

Not known from public competitor research
  Competitor ROAS
  Competitor CPA
  Conversion rate
  Gross margin
```

Sort v1 strategy recommendations by the most defensible observable signal:

1. distinct advertisers;
2. source entropy;
3. number of related variants;
4. persistence/longevity;
5. recency;
6. cross-platform recurrence;
7. evidence completeness.

Do not collapse those into a 0-100 number until the product has enough authorised outcome data to fit and validate a model. The later model can be hierarchical or Bayesian:

```text
outcome ~ creative_family + offer + audience + placement + spend + brand + seasonality
```

The Reel validation spike can supply an earlier supervised learning test using engagement outcomes. Paid-client accounts later supply commercial outcomes through official account APIs [S6, S7].

## 7. Similarity And Safety Policy

Do not ship a composite legal/similarity risk score in v1. Do not ship thresholds such as "0.68 means reauthor" or "0.55 means human review." Australian and U.S. copyright analysis does not map cleanly to a percentage or cosine threshold. Australian guidance distinguishes copyright-protected expression from ideas or information and identifies fair-dealing exceptions as purpose-specific; U.S. Copyright Office fair-use guidance also stresses case-specific analysis rather than fixed quantities [S16, S17].

Ship hard gates and human review:

| Gate | v1 action |
|---|---|
| Exact or near-exact third-party footage match | Block |
| Unlicensed audio fingerprint match | Block |
| Competitor logo, watermark, or trade dress retained | Block |
| Distinctive source slogan or rare phrase retained | Block |
| Missing face, likeness, or voice consent | Block |
| Unsupported claim not in the claims ledger | Block |
| Platform policy violation | Block |
| Any non-hard-gate similarity concern | Human review |

The similarity engine still needs separate channels:

```text
lexical, semantic, visual, sequence, pacing, layout/OCR, audio, identity
```

But in v1 these channels should produce an explanation packet for review, not an automated "legal safe" number.

A reviewer packet should show:

```text
Potential concern:
  Similar sequence: hook -> screen demo -> proof -> CTA

Why not automatically blocked:
  Sequence is a common category convention.
  No source n-gram >= 5 tokens.
  No competitor logo/watermark.
  No source footage/audio match.

Reviewer question:
  Does the combination of structure, wording, timing, layout, and visuals
  look source-specific enough to require re-authoring?
```

### Retained Fingerprints Are Derived Data

The system cannot scan a render against references unless it retains some representation of those references. Therefore, fingerprints and embeddings must be treated as retained derived data with their own rights basis, security controls, and retention policy.

Minimum policy fields:

```text
fingerprint_id
source_creative_id
source_type
rights_basis
allowed_uses
retention_until
deletion_policy
embedding_model
hashing/fingerprinting_method
access_roles
audit_log
```

The report should not claim "raw media not retained" as if no reference-derived data remains. The correct claim is narrower: raw media is not exposed to the producer, and retained derived fingerprints are restricted to safety, deduplication, and family clustering under a documented policy.

## 8. Cost Model

The original report needed a cost model because video understanding can dominate onboarding COGS. The model should be implemented before choosing managed video understanding at scale.

For each reference video:

```text
reference_video_cost =
  acquisition_or_vendor_cost
  + storage_cost
  + deterministic_media_cost
  + asr_cost
  + ocr_cost
  + object_tracking_cost
  + vlm_or_managed_video_cost
  + embedding_cost
  + human_review_cost
  + deletion_or_retention_overhead
```

For each generated output:

```text
output_cost =
  concept_generation_cost
  + supplemental_asset_generation_cost
  + render_cost
  + safety_scan_cost
  + claims_review_cost
  + human_approval_cost
```

The useful unit economics question is not "what does one model call cost?" It is:

```text
Can an agency onboard one client, analyse enough market references,
produce three reviewable blueprints, and pass safety review
below the gross margin available in the first billing cycle?
```

### Example Cost Worksheet

Assume a narrow onboarding slice:

```text
20 reference creatives
30 seconds each
10 total reference minutes
3 generated blueprints
0-3 supplemental generated shots
1 final render
20 human-reviewed Creative DNA records
```

And the original mockup scale:

```text
347 reference creatives
30 seconds each
173.5 total reference minutes
```

The table below is intentionally formulaic. Vendor pricing changes, and several vendors require calculators or sales conversations. The product should store current assumptions in a versioned cost model rather than hiding them in prose.

| Cost driver | Formula | Buy/build implication |
|---|---|---|
| Commercial corpus | `monthly_vendor_fee / clients_or_searches_used` | Use only if contract permits analysis, embeddings, retention, and derived data. |
| Deterministic extraction | `worker_minutes * infra_cost_per_worker_minute` | Build/reuse locally first; benchmark on real videos. |
| ASR | `video_minutes * asr_price_per_minute` or local GPU/CPU equivalent | Local WhisperX is attractive when accuracy and alignment are adequate. |
| OCR | `frames_processed * ocr_cost_per_frame` or local worker cost | Keep frame selection sparse; OCR every frame is usually wasteful. |
| Object/VLM layer | `video_minutes * managed_video_price` or `gpu_minutes * gpu_price` | Defer until transcript + scenes + OCR pass human validation. TwelveLabs is a benchmark/accelerator, not an assumed default [S5]. |
| Generated video | `generated_seconds * provider_price_per_second` | Put fal or similar behind a provider adapter; pricing is model-dependent [S18]. |
| Remotion automation | `max(render_count * per_render_price, monthly_minimum)` for commercial automation | Remotion's current pricing page lists automator pricing and a monthly minimum; cloud compute is separate [S19, S20]. |
| Human QA | `records_reviewed * minutes_per_record * reviewer_rate` | This may exceed compute costs during validation and should be budgeted explicitly. |

### Buy-Versus-Build Recut

| Layer | v2 recommendation | Rationale |
|---|---|---|
| Reference acquisition | Buy/partner or client-authorised only | Commercial rights matter more than scraping coverage. |
| Website/brand ingest | Use hosted crawler or simple custom crawler | Firecrawl is useful, but AGPL server embedding needs review [S21]. |
| FFmpeg normalisation | Reuse/build wrapper | Mature, but pin build flags because FFmpeg licence obligations depend on configuration [S13]. |
| Scene detection | Reuse PySceneDetect | Low cost, deterministic, and enough for v0 [S14]. |
| ASR | Reuse WhisperX or managed ASR benchmark | Word-level timestamps are critical; measure accuracy on short-form ads. |
| OCR | Reuse PaddleOCR or managed OCR | Keep bounding boxes and time intervals. |
| Object tracking | Defer from first slice | Valuable later, not required to prove Creative DNA. |
| VLM/video semantic layer | Benchmark after v0 | Managed TwelveLabs or self-hosted VLM only after cheaper signals are validated [S5]. |
| Agent graph | LangGraph-style typed workflow | Useful for explicit state and permissions; do not use a monolithic agent. |
| Durable jobs | Add when retries/renders become painful | Temporal or equivalent is useful after jobs become operationally non-trivial. |
| Blueprint/render | Internal Blueprint + Remotion/FFmpeg | Remotion is a strong fit; budget commercial terms [S19, S20]. |
| OTIO | Defer to adapter | Keep MVP representation in Blueprint JSON; add OTIO only for NLE interchange [S15]. |

The highest-risk cost assumption is the old "analyse 347 creatives" onboarding surface. That may be strategically useful later, but v1 should cap analysis to a small reference set until the extraction quality and unit economics are proven.

## 9. Revised Timeline

The original 16-week plan compressed too many uncertain systems into too little time. A realistic plan separates a validation spike from a commercial pilot.

### Phase A: Reels Validation Spike, 3-5 Weeks

**Corrected 2026-09-10.** The original 6-8 week estimate was written without knowledge of
`ArianKalantari1/senpai-reel`, a working pipeline that already implements most of the
deterministic stack. See `docs/existing-systems/senpai-reel-inventory.md` for the full inventory.

Already built, and not to be rebuilt:

| Capability | Where |
|---|---|
| Apify scrape, download, ffmpeg audio extraction | `collection/`, `processing/` |
| ASR with word-level timestamps | Deepgram Nova-2, `transcript_words` table |
| Engagement labels (likes, views, comments, duration) | `posts` table |
| Semantic extraction, embeddings, search | `analysis/` |
| **Baseline engagement model** | `analysis/engagement_predictor.py` — RandomForest over metadata |

The baseline arm of the validation experiment therefore already exists. What remains:

| Time | Outcome | Gate |
|---|---|---|
| Week 1 | Define Creative DNA v0 schema, observation/interpretation split, enumerated vocabularies | Schema reviewed by one creative strategist. |
| Week 1-2 | Add the two missing extractors: scene/shot detection and OCR timeline | Both run over the existing downloaded corpus. |
| Week 2-3 | Time-alignment fusion and the structural interpretation pass | Fused view is independently inspectable. Budget real effort here — reconciling three tools' notions of time is the step most likely to overrun. |
| Week 3 | Human-label 20 records, attribute every disagreement to a layer | **Hard gate.** Stop if key fields are not trusted by the reviewer. Bad extraction produces a clean-looking null result that cannot be distinguished from a failed premise. |
| Week 4-5 | Baseline vs Creative DNA comparison, split by account | Held-out Creative DNA model must beat the metadata baseline by a pre-registered margin. |
| Week 5 | Error analysis, schema revision, written verdict | Decide whether the paid-ad product proceeds, narrows, or pauses. A null result closes Phase A successfully. |

No VLM, object detection, render pipeline, or paid-ad production system is required before this gate.

Tracked as issues #1-#7 in this repository.

### Phase B: Agency Research MVP, 8-12 Additional Weeks

| Time | Outcome | Gate |
|---|---|---|
| Weeks 1-2 | Agency buyer interviews and workflow mapping | Pick one recurring workflow, not a generic dashboard. |
| Weeks 3-4 | Rights-gated reference acquisition path | Named API/vendor/upload path with terms reviewed. |
| Weeks 5-6 | Creative-family grouping and evidence panel | No composite confidence score. |
| Weeks 7-8 | Brand DNA + claims ledger v0 | Claims require human/account-manager confirmation. |
| Weeks 9-10 | Strategy abstraction firewall | Closed schema plus leak tests pass on sample references. |
| Weeks 11-12 | Agency pilot workflow | Produce strategy briefs or blueprints, not automated final ads. |

### Phase C: Commercial Production Pilot, 12-16 Additional Weeks

| Time | Outcome | Gate |
|---|---|---|
| Month 1 | Blueprint editor and client asset assignment | No competitor raw media in production context. |
| Month 2 | Render path with Remotion/FFmpeg | Cost per render and licensing tracked. |
| Month 3 | Hard safety gates and human approval | Every output has provenance, claims, rights, and review packet. |
| Month 4 | Authorised ad-account outcome loop | Start fitting outcome model only after sufficient data. |

This makes a commercial v1 closer to 6-9 months than 16 weeks. The shorter milestone is the Reels spike, which should decide whether the larger product deserves that investment.

## 10. Product Flow

The MVP should feel simple even though the backend is strict.

### Agency Onboarding

```text
Create client
-> enter website and approved public profiles
-> upload or connect authorised assets
-> confirm Brand DNA
-> confirm claims/proof ledger
-> define competitors/reference sources
```

The account manager must confirm high-risk fields:

```text
service area
pricing
availability
regulated claims
testimonials
performance claims
talent releases
brand exclusions
```

### Market Intelligence

```text
Market snapshot
  19 relevant competitors
  40 rights-permitted reference creatives analysed
  11 creative families

Strategies worth reviewing
  Problem -> demo -> proof -> CTA
  Founder advice -> objection -> proof
  Myth-busting educational hook

Not shown
  "Winning ad"
  estimated ROAS without authorised data
  clone button
```

### Strategy Inspection

```text
Strategy:
  Cost-of-inaction -> demonstration -> proof -> low-friction CTA

Evidence:
  observed across multiple advertisers
  recurred across more than one platform
  active/persistent in the observed corpus

Do not copy:
  competitor wording
  actors
  footage
  music
  logo/watermark
  distinctive visual arrangement
```

### Production

The system should generate three materially different blueprints:

```text
A. Founder-led authority
B. Customer-problem narrative
C. Demonstration-first
```

Each blueprint is built from Brand DNA, claims ledger, and allowed assets. If required assets are missing, the product should say so instead of hallucinating proof:

```text
Missing:
  close-up service demonstration
  approved customer quote for speed claim
  founder talking-head clip
```

### Safety View

```text
Originality packet          READY FOR REVIEW
Third-party footage         none detected
Unlicensed music            none detected
Competitor identity         none detected
Rare phrase/slogan match    none detected
Claims                      4 checked / 4 substantiated
Talent permissions          valid
Reviewer decision           required
```

For v1, the final action should be "request human approval", not "auto-publish".

## 11. Legal, Platform, And Ethical Risk

Copyright protects expression, not mere ideas, but copying a qualitatively substantial part can still create risk even when the copied portion is small. Australian fair dealing is purpose-specific, and the U.S. fair-use framework is case-specific [S16, S17]. Therefore the product must avoid promising that a numeric similarity result creates legal safety.

The safe product framing is:

```text
Automated provenance, rights, claims, and copying-risk controls
to support human creative and legal review.
```

The unsafe framing is:

```text
Our model guarantees this ad is original because similarity is below X%.
```

Production inputs should be rights-first:

```text
client-owned upload
client-authorised social/ad account
licensed stock
official API within terms
contracted commercial corpus
generated asset with commercial-use rights
```

High-risk inputs should be excluded from commercial production:

```text
unauthorised downloads
scraped social media outside platform terms
competitor raw media passed to generator
source music used as style target
unlicensed faces or voices
unverified testimonials or performance claims
```

Ethical controls must go beyond copyright. The system should not invent testimonials, certifications, performance metrics, medical/financial claims, or endorsements. Synthetic people, voices, and avatars need explicit policy and consent controls.

## 12. Dependency And Licensing Table

Star counts are volatile GitHub metadata, rounded to the nearest sensible unit. They are context only, not build-vs-buy proof. Licence values marked "GitHub metadata" come from the GitHub REST repository endpoint linked in each row; for projects with custom or source-available terms, the project licence or pricing pages are more important than the SPDX field.

| Layer | Project/service | Stars or pricing context | Licence/terms note | v2 recommendation |
|---|---|---:|---|---|
| Media | [FFmpeg](https://github.com/FFmpeg/FFmpeg) ([metadata](https://api.github.com/repos/FFmpeg/FFmpeg)) | ~64.1k | Official legal page says LGPL by default with optional GPL/nonfree build concerns [S13]. | Use behind `MediaService`; pin build flags. |
| ASR | [WhisperX](https://github.com/m-bain/whisperX) ([metadata](https://api.github.com/repos/m-bain/whisperX)) | ~24.0k | BSD-2-Clause in GitHub metadata. | Use or benchmark; track diarisation/model terms separately. |
| Scenes | [PySceneDetect](https://github.com/Breakthrough/PySceneDetect) ([metadata](https://api.github.com/repos/Breakthrough/PySceneDetect)) | ~5.2k | BSD-3-Clause in GitHub metadata; v0.7 release confirmed [S14]. | Use in v0. |
| OCR | [PaddleOCR](https://github.com/PaddlePaddle/PaddleOCR) ([metadata](https://api.github.com/repos/PaddlePaddle/PaddleOCR)) | ~89.3k | Apache-2.0 in GitHub metadata. | Use in v0 if operationally acceptable. |
| Detection | [GroundingDINO](https://github.com/IDEA-Research/GroundingDINO) ([metadata](https://api.github.com/repos/IDEA-Research/GroundingDINO)) | ~10.6k | Apache-2.0 in GitHub metadata. | Defer until after v0 validation. |
| Tracking | [SAM 2](https://github.com/facebookresearch/sam2) ([metadata](https://api.github.com/repos/facebookresearch/sam2)) | ~19.8k | Apache-2.0 in GitHub metadata. | Defer until motif tracking matters. |
| VLM | [Qwen3-VL](https://github.com/QwenLM/Qwen3-VL) ([metadata](https://api.github.com/repos/QwenLM/Qwen3-VL)) | ~19.9k | Apache-2.0 in GitHub metadata; verify exact checkpoint/model-card terms. | Benchmark after v0. |
| Timeline | [OpenTimelineIO](https://github.com/AcademySoftwareFoundation/OpenTimelineIO) ([metadata](https://api.github.com/repos/AcademySoftwareFoundation/OpenTimelineIO)) | ~2.0k | Apache-2.0 in GitHub metadata; timeline interchange format [S15]. | Defer to adapter. |
| Agent graph | [LangGraph](https://github.com/langchain-ai/langgraph) ([metadata](https://api.github.com/repos/langchain-ai/langgraph)) | ~41.4k | MIT in GitHub metadata. | Use typed graph patterns, not a super-agent. |
| Durable jobs | [Temporal Python SDK](https://github.com/temporalio/sdk-python) ([metadata](https://api.github.com/repos/temporalio/sdk-python)) | ~1.2k | MIT in GitHub metadata. | Add when job retries become painful. |
| Website ingest | [Firecrawl](https://github.com/firecrawl/firecrawl) ([metadata](https://api.github.com/repos/firecrawl/firecrawl)) | ~178.8k | AGPL-3.0 in GitHub metadata. | Hosted API or legal-reviewed deployment; do not casually embed server code. |
| Renderer | [Remotion](https://github.com/remotion-dev/remotion) ([metadata](https://api.github.com/repos/remotion-dev/remotion)) | ~58.9k | GitHub metadata is no-assertion; current Remotion pages describe custom/commercial licensing [S19, S20]. | Use if licence/cost accepted. |
| Editor | [OpenCut](https://github.com/OpenCut-app/OpenCut) ([metadata](https://api.github.com/repos/OpenCut-app/OpenCut)) | ~89.2k | MIT in GitHub metadata. | Watch; do not depend on it for MVP editor core. |
| Video R&D | [InternVideo](https://github.com/OpenGVLab/InternVideo) ([metadata](https://api.github.com/repos/OpenGVLab/InternVideo)) | ~2.4k | Apache-2.0 in GitHub metadata. | R&D benchmark only. |
| Workflow prototype | [n8n](https://github.com/n8n-io/n8n) ([metadata](https://api.github.com/repos/n8n-io/n8n)) | ~203.9k | Repository metadata is no-assertion; licence file uses Sustainable Use License plus enterprise-file exclusions [S22]. | Internal prototype only unless commercially licensed. |
| Alternative VLM | [VideoLLaMA3](https://github.com/DAMO-NLP-SG/VideoLLaMA3) ([metadata](https://api.github.com/repos/DAMO-NLP-SG/VideoLLaMA3)) | ~1.2k | Apache-2.0 in GitHub metadata; verify model/data/service terms separately. | Benchmark only. |
| Managed video understanding | [TwelveLabs](https://www.twelvelabs.io/blog/video-segmentation-api-how-to-extract-structured-data-from-video) | Pricing calculator/sales | Managed timestamped structured video segmentation [S5]. | Benchmark as accelerator after cost model. |
| Video generation gateway | [fal](https://fal.ai/pricing) | per-model pricing | Pricing varies by model and output seconds [S18]. | Use behind provider adapter only. |

## 13. First Build Recommendation

Do not start with an autonomous ad generator. Start with the smallest artefact that can prove or falsify the thesis:

```text
Input:
  20-100 short-form organic videos with allowed research use

Pipeline:
  ffprobe -> scene detection -> ASR -> OCR -> Creative DNA v0

Human gate:
  creative strategist reviews 20 records

Experiment:
  baseline metadata model vs Creative DNA feature model

Output:
  one written research brief that explains which Creative DNA features
  predict engagement, which do not, and what schema changes are needed
```

Only after this should the team build:

- paid-ad reference ingestion;
- market evidence panels;
- Brand DNA and claims ledger;
- SafeStrategy firewall;
- Blueprint editor;
- rendering;
- safety gates;
- performance-feedback loop.

The durable asset is not the rendered MP4. The durable asset is structured intelligence:

```text
who the creative is for
what problem it activates
what promise it makes
how it proves the promise
how the scenes deliver the argument
what evidence supports testing it
what must not cross from source references
how the final asset differs from references
how it performs when tested
```

## Source Register

All sources below were accessed on 2026-09-11 unless otherwise noted.

- [S1] Creatify API documentation: https://docs.creatify.ai/
- [S2] Creatify product site: https://creatify.ai/
- [S3] Arcads product site: https://www.arcads.ai/
- [S4] Foreplay product site: https://www.foreplay.co/
- [S5] TwelveLabs, "Video Segmentation API: How to Extract Structured Data From Video": https://www.twelvelabs.io/blog/video-segmentation-api-how-to-extract-structured-data-from-video
- [S6] Google Ads API reporting overview: https://developers.google.com/google-ads/api/docs/reporting/overview
- [S7] TikTok API for Business portal: https://business-api.tiktok.com/portal
- [S8] Meta Instagram Business Discovery documentation: https://developers.facebook.com/docs/instagram-platform/instagram-api-with-facebook-login/business-discovery
- [S9] Meta Instagram Media Insights documentation: https://developers.facebook.com/docs/instagram-platform/reference/instagram-media/insights
- [S10] Instagram Terms of Use: https://help.instagram.com/581066165581870/
- [S11] YouTube Terms of Service: https://www.youtube.com/t/terms
- [S12] YouTube API Services Terms of Service: https://developers.google.com/youtube/terms/api-services-terms-of-service
- [S13] FFmpeg license and legal considerations: https://ffmpeg.org/legal.html
- [S14] PySceneDetect v0.7 release: https://github.com/Breakthrough/PySceneDetect/releases/tag/v0.7-release
- [S15] OpenTimelineIO repository and project description: https://github.com/AcademySoftwareFoundation/OpenTimelineIO
- [S16] Australian Attorney-General's Department, copyright basics: https://www.ag.gov.au/rights-and-protections/copyright/copyright-basics
- [S17] U.S. Copyright Office Fair Use Index: https://www.copyright.gov/fair-use/
- [S18] fal pricing: https://fal.ai/pricing
- [S19] Remotion licence and pricing: https://www.remotion.dev/docs/license/pricing
- [S20] Remotion terms: https://www.remotion.dev/docs/terms
- [S21] Firecrawl GitHub repository metadata: https://api.github.com/repos/firecrawl/firecrawl
- [S22] n8n licence file: https://github.com/n8n-io/n8n/blob/master/LICENSE.md
- [S23] Meta Ad Library: https://www.facebook.com/ads/library/
- [S24] Meta Graph API Ads Archive reference: https://developers.facebook.com/docs/marketing-api/reference/ads_archive
- [S25] Google Ads Transparency Center: https://adstransparency.google.com/

