# Existing system inventory — `senpai-reel`

**Repository:** https://github.com/ArianKalantari1/senpai-reel (public, readable by any agent)
**Owner account:** `ArianKalantari1` (transferred from `Arian-MU` on 2026-09-10)
**Surveyed:** 2026-09-10, at commit `3a79832` (2026-04-06)
**Why this file exists:** agents have no memory between sessions. This is the shared record of
what already exists, so no one re-derives it or plans work that is already done.

> **Do not vendor this code into `creative-director-ai`.** It is a live codebase (drift), and it
> contains research acquisition logic that must stay outside the commercial repo. Read it at the
> URL above.

---

## What it is

A working 5-stage Streamlit pipeline that scrapes 23 curated competitor Instagram accounts in the
**Jobs-in-Australia** niche, transcribes their Reels, extracts structured knowledge, and generates
grounded content. 60 Python files, 12 test files, single-file DuckDB.

```
Instagram accounts
  → [1] SCRAPE       Apify actor `apify~instagram-reel-scraper` → raw JSON + DB rows
  → [2] DOWNLOAD     direct HTTP, yt-dlp fallback → .mp4
  → [2b] AUDIO       ffmpeg → 16kHz mono .wav
  → [3] TRANSCRIBE   Deepgram Nova-2 (en-AU) → text + WORD-LEVEL timestamps
  → [4] EXTRACT      GPT-4o-mini → "message units" (topic/type/claim taxonomy)
  → [4b] EMBED       text-embedding-3-small → FLOAT[1536] in DuckDB
  → [5] USE          semantic search · analytics · content studio
```

## What this means for Creative Director AI

**The extraction stack is not greenfield.** Three of the four deterministic layers already run in
production, and the outcome labels the validation experiment needs are already in the database.

### Already built

| Capability | Where | Note |
|---|---|---|
| Engagement labels | `posts` table | `likes`, `views`, `comments_count`, `duration_sec`, `engagement_rate`, `posted_at` |
| Media normalisation | `processing/audio.py` | ffmpeg → 16kHz mono wav |
| ASR with word timings | `processing/transcribe.py`, `transcript_words` table | Deepgram Nova-2, per-word start/end/confidence |
| Semantic extraction | `analysis/extraction.py`, `analysis/taxonomy.py` | 11 topics × 8 content types |
| Embeddings + search | `analysis/embeddings.py`, `analysis/search.py` | DuckDB native cosine similarity |
| **Baseline engagement model** | `analysis/engagement_predictor.py` | **RandomForest over metadata — this is the baseline arm of the validation experiment, already written** |
| Cost figures | `docs/COST_ANALYSIS.md` | Deepgram $0.0058/audio-min; GPT-4o-mini ~$0.0003/reel; embeddings ~free |

### Legacy / disabled

`analysis/ai_video_analyzer.py` — cv2 frame extraction + `gpt-4-vision-preview`. Its Streamlit
pages are underscore-prefixed (disabled) and the model is long superseded. Treat as prior art,
not a foundation.

### Not built — this is the actual gap

1. **Scene/shot detection.** No PySceneDetect anywhere in the repo.
2. **OCR timeline.** No OCR of any kind. On-screen text is a major Reels signal and is entirely missing.
3. **Structural extraction.** This is the conceptual gap, not just a missing library.
   `message_units` capture **what is said** (a tip, a stat, a warning, by topic).
   Creative DNA needs **how it is built** — hook family, time-to-first-claim, time-to-payoff,
   shot-length distribution, narrative function sequence, loop structure. Same transcripts,
   different abstraction. Neither replaces the other.
4. **A second feature arm** in the predictor, to compare Creative DNA features against the
   existing metadata baseline.

## Rights posture

Acquisition is via **Apify**, a commercial vendor — *not* raw scraping of public URLs. That is a
materially better starting position than the research report assumed, but the Apify contract still
needs review before any of this feeds a paid product. The research/commercial acquisition split in
`research/2026-09-marketing-intelligence-creative-os-v2.md` §3 still stands.

## Known operational issues

| Issue | Impact on us |
|---|---|
| Apify CDN URLs expire in ~24–48h | Scrape and download must be close together; affects batch planning |
| DuckDB single-writer lock | Streamlit and CLI jobs cannot write concurrently |
| Audio extraction has no UI trigger | Manual step between download and transcription |

## Ownership

Transferred from `Arian-MU` to `ArianKalantari1` on 2026-09-10, so both repositories now sit
under one owner. Issues, PRs and write access work across them; agent tooling scoped to the
account can see both. Old `Arian-MU/senpai-reel` links still redirect, but prefer the new URL.

## Correction to the v2 research report

§9 Phase A of `research/2026-09-marketing-intelligence-creative-os-v2.md` describes a 6–8 week
Reels validation spike starting from nothing. That estimate was written without knowledge of this
repository and is wrong. Scrape, download, audio, ASR-with-word-timings, engagement labels and a
metadata baseline model all exist. The remaining work is items 1–4 under *Not built* above.
