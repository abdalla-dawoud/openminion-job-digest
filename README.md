# openminion-job-digest

A small, working agent built on the OpenMinion 0.1 Python SDK that completes
one useful, repeatable workflow: it scans free public remote-job feeds,
dedupes against what it already surfaced, and writes a Markdown shortlist of
junior-friendly AI/software roles.

Built for Cortensor Hackathon #5 (OpenMinion track).

## What it does

1. Fetches fresh remote job listings from two free, keyless JSON APIs
   (Remotive and Arbeitnow) through OpenMinion `@tool` functions with
   explicit least-privilege scopes.
2. Loads the ids of jobs already surfaced in earlier runs, so repeat runs
   only show new matches (a persistent JSON store plus an OpenMinion session
   for run-to-run continuity).
3. Shortlists up to 10 roles that fit a junior AI/software engineer profile,
   excluding senior (3+ yrs), unpaid, and gambling/forex/trading listings.
4. Returns a typed result validated against a Pydantic model
   (`output_type=Digest`) and renders `out/digest-YYYY-MM-DD.md`.

## OpenMinion building blocks exercised

- `Agent` Python SDK surface with a persistent `session_id`
- `@tool` functions with `min_scope` / `idempotent` declarations
- Structured output via `output_type` with automatic validation
- `ProviderError` handling, including rate-limit backoff
- Config written by `openminion setup` (`agents.json`), provider-agnostic

## Setup

```bash
pip install -r requirements.txt
openminion setup --no-chat --provider groq --model qwen/qwen3.8-27b
export GROQ_API_KEY=...   # free tier at console.groq.com
```

Any OpenMinion-supported provider works; the demo used Groq's free tier.

## Run

```bash
python job_digest.py
```

Output lands in `out/digest-<today>.md`. Re-running the same day surfaces
only jobs not seen before. Use `--session` to isolate runs.

## Demo

See `DEMO.md` for the recorded walkthrough script and `out/` for a real
generated digest.

## What we learned

See `LEARNINGS.md` - concrete notes on the 0.1 SDK: what worked, what broke,
and what we would change (including a write-scope confirmation gate surprise
and session turn-lease behavior after a killed process).
