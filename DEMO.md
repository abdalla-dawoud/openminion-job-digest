# Demo script (1-5 min, TTS narration)

1. Show the repo and README: one agent, three free sources, deduped digest.
2. Run `python job_digest.py` with the env vars from `.env.example`.
3. Point out the forced `load_seen_job_ids` tool call (a real SDK tool invocation).
4. Open `out/digest-<date>.md`: picks with exact source links.
5. Run again: seen-store dedupe means already-surfaced jobs are skipped.
6. Close with the "What We Learned" points from LEARNINGS.md.
