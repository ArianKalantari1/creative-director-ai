# STATUS

Two things live here, and nothing else:

- **Direction** — rarely changes. Written once.
- **Decisions** — append-only. One line, dated. Never edited, only added to.

Everything else (open PRs, CI, issues, test counts) is **computed** — run
`./bin/where` or just read GitHub. Nothing in this file needs to stay fresh,
which is why it will still be true in a month.

---

## Direction

One operator acting as the **marketing function** for small businesses that have no
marketing team. Monthly direction meeting per client; the system carries execution
between meetings. That is what lets one person hold ten clients instead of two.

**Marketing is not video.** Video is one output. Posts, articles, carousels, images and
client footage all count.

Three layers, all feeding generation: **Observation** (scraped competitor content),
**Technique** (hooks, CTAs, structure), **Knowledge** (marketing theory with sources
attached). Long version: [`docs/vision.md`](docs/vision.md).

Conventions for building and reviewing live in `senpai-reel/AGENTS.md` and
`senpai-reel/CLAUDE.md`. Not repeated here.

---

## Open questions

The only list that gets edited — items move down to Decisions when they close.
**Do not write these down as decided.**

- Whether video may be retained once commercial — #16
- What replaces Apify as the acquisition source — #20
- Whether Symphony output can be published off TikTok — #28
- Whether Symphony avatars are portable across clients — #28
- Whether the extracted ideas are good enough that a client would pay — partly answered

---

## Decisions and findings

Append at the top. Never edit a line once written — if it turns out wrong, add a new
line saying so.

- `2026-09-13` Extractor error rate 20% on a 40-unit hand-marked sample. **7 of the 8 were one bug**: performed sentences (satire, scripted demos, quoted bad practice) read as sincere assertions. Fixed in the prompt.
- `2026-09-13` Mechanical scorer agrees with a careful human read only **57%**. Its corpus numbers are not trustworthy. It cannot see semantic laziness or speaker stance.
- `2026-09-13` Competitor wording leaks into claims at **1.4%** (74/5,222, 100% transcript coverage). The abstraction firewall holds.
- `2026-09-13` **11.2%** of the corpus is technique, not subject (343 cta + 240 hook). Generation now excludes it at source.
- `2026-09-13` Model pass to classify the remaining 88.8% costs **$0.19**. Cost is not a reason to delay it.
- `2026-09-13` Dead code that is subtly wrong gets **archived, not fixed** — its presence implies it works.
- `2026-09-13` Corrected: Symphony avatars locking to an ad account is **not** in TikTok's documentation. It came from a secondary source and was stated here as fact.
- `2026-09-11` Apify **cannot** be used in a paid service. Blocker on charging, not on building.
- `2026-09-11` Downloaded competitor video is retained for **research, personal use** — decided knowingly.
- `2026-09-10` Competitor material is **evidence, never source material**. Patterns reusable, wording never.

---

## Lessons

Also append-only. Each cost a round.

- A **green suite is not evidence the thing works** — a tool shipped unable to open a real database because every test built a fresh one.
- If a result looks **surprisingly clean, suspect the harness**. Happened twice.
- `A OR B AND client_id = ?` binds as `A OR (B AND ...)`. A precedence bug crossed client boundaries.
- A **secondary source is not a finding**. Say where a claim came from or do not state it.
- Treating an **open question as settled** costs a round. Hence the section above.
