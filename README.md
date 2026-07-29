# AI Engineering Notes

Working notes from building production AI/RAG systems — the **Apply** and **Teach** layer of a Learn → Apply → Teach loop. Each entry is written to be re-usable: the concept, the actual change, the gotchas, and an interview-ready summary.

## How this repo is organised

Everything starts in [`SKILLS-LEARNING.md`](./SKILLS-LEARNING.md) as topic sections with a table of contents. When a section gets long enough to stand on its own, it gets split into its own file (e.g. `redis.md`, `rag.md`, `fastapi.md`) and linked from the index below.

## Index

| Topic | Where | Status |
|-------|-------|--------|
| Redis — `SCAN` / `scan_iter()` vs `KEYS` | [`SKILLS-LEARNING.md`](./SKILLS-LEARNING.md#redis--scan-scan_iter-vs-keys) | ✅ |

_(Grows as notes are added. Split a topic into its own file once it outgrows a section.)_

## Conventions

- One `##` section per topic, each with: **Concept → The change → Gotchas → Interview line → References.**
- Anchor notes to real code from actual projects, not toy examples.
- Every claim you'd repeat in an interview gets a source link.