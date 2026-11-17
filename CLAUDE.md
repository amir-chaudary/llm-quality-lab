# LLM Quality Lab

Personal learning repository for a 14-week transition from traditional QA into
LLM and AI quality engineering. Solo project, no other contributors.
Plan runs 16 November 2026 to 21 February 2027.

## Learning contract - read this first

This repository exists so that I learn, not so that code gets written fast.
Optimise for my understanding, not for throughput.

- Explain WHY before writing code. Name the trade-off you are making.
- Prefer the smallest change that teaches the concept over the complete solution.
- When I ask for an eval metric, tell me the failure mode it catches AND the one
  it misses.
- Push back when my test design is weak. Say so plainly, with the reason.
- Do not add dependencies without asking first.
- Some artefacts I must write by hand. Those rules are in
  `.claude/rules/evals.md` and load when we touch evaluation code.

## Current phase

**Phase 0 (16-29 Nov 2026): environment setup, Python and LLM foundations.**

Update this line at the start of each phase. It tells you what I should and
should not already know, so you can pitch explanations correctly.

## Stack

- Python 3.12 managed by `uv`. Never `pip`, never the system Python.
- `pytest` for all tests. Pydantic v2 for all structured data.
- FastAPI for serving the system under test.
- Evaluation: DeepEval (Python, pytest-native) and Promptfoo (YAML, CI gate).
- RAG metrics: RAGAS. Observability: Langfuse, self-hosted.
- Local models via Ollama at `http://localhost:11434/v1`. Prefer these for
  iteration; frontier models only to validate a stable suite.
- Containers: Colima, not Docker Desktop. Run `colima start` before any docker
  command. Machine specifics are in `CLAUDE.local.md`.

## Layout

```
src/app/        system under test: RAG pipeline, agent, FastAPI service
src/evals/      custom metrics, judge prompts, calibration scripts
tests/          pytest suites including DeepEval test cases
prompts/        prompt templates referenced by promptfooconfig.yaml
data/traces/    raw JSONL traces (gitignored)
data/datasets/  curated, hand-labelled eval sets (committed, the valuable part)
reports/        generated HTML reports and red team output
scripts/        one-off tooling and CI helper scripts
```

## Commands

```
uv sync                            install dependencies
uv run pytest tests/ -v            run the test suite
uv run deepeval test run <file>    run DeepEval cases
uv run ruff check --fix            lint
uv run ruff format                 format
promptfoo eval                     run the Promptfoo suite
promptfoo view                     open results at localhost:15500
colima start                       start the container runtime
```

## Conventions

- Conventional Commits: `feat:`, `fix:`, `test:`, `docs:`, `chore:`, `refactor:`.
- Work on `feat/<topic>` branches and merge via PR, even though I work alone.
  The PR is what makes the CI eval gate meaningful.
- `ruff check` and `ruff format` must pass before every commit.
- Every new eval metric needs a test proving it fires on a known-bad example and
  stays quiet on a known-good one.
- Type hints on every function. Pydantic models over dicts for anything crossing
  a boundary.
- Never commit `.env`, API keys, raw traces, or client material.
- Tag at the end of each phase: `v0.1` through `v1.0`.

## Cost discipline

API calls cost real money and I am on a capped monthly budget.

- Default to Ollama for development loops.
- Before any script that loops over a dataset, state the estimated call count
  and cost, and wait for me to confirm.
- Never write a script that calls an API inside an unbounded loop.
- Promptfoo caching is on by default. Do not pass `--no-cache` unless I ask.

## What I already know

18 years in QA: test design, risk-based testing, defect lifecycle, Playwright,
CI pipelines, ISO 9001, Agile QA. Comfortable with Python and Node at a
scripting level, moving to engineering level.

Assume competence on testing fundamentals. Explain LLM-specific concepts, not
QA ones. Do not explain what a fixture or a regression suite is.
