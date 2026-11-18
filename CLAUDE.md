# LLM Quality Lab

Personal learning repository for a 24-week transition from traditional QA into
LLM and AI quality engineering. Solo project, no other contributors.
Plan runs 18 November 2026 to 4 May 2027 in three stages: eight weeks of Python
only, six braided weeks, then ten weeks of LLM quality engineering.

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

**Stage A (18 Nov 2026 - 12 Jan 2027): Python from the ground up. No LLM work.**

Week 1: mechanics. Week 2: control flow and collections. Week 3: functions.
Week 4: comprehensions and generators. Week 5: modules and stdlib.
Week 6: classes. Week 7: exceptions, context managers, decorators.
Week 8: files, testing, capstone project.

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
practice/       Stage A exercises, one package per week, all pytest-based
practice/GAPS.md  every wrong prediction, drives the weekly checkpoints
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
- Tag at the end of each stage: `v0.1.0-python`, then `v0.2` through `v1.0`.

## Cost discipline

API calls cost real money and I am on a capped monthly budget.

- Default to Ollama for development loops.
- Before any script that loops over a dataset, state the estimated call count
  and cost, and wait for me to confirm.
- Never write a script that calls an API inside an unbounded loop.
- Promptfoo caching is on by default. Do not pass `--no-cache` unless I ask.

## What I already know

18 years in QA: test design, risk-based testing, defect lifecycle, Playwright,
CI pipelines, ISO 9001, Agile QA. Built a Rust and Playwright test automation
framework at a previous employer.

So: assume competence on testing concepts, programming concepts, debugging
method, version control and architecture. Do not explain what a regression
suite, a race condition or a stack trace is.

But do NOT assume Python fluency. My Python is being rebuilt from the ground
up. Explain Python syntax, idiom and standard library as if new. Where a
concept exists in another language I know, name the parallel rather than
explaining the concept from scratch.

## Stage A rules - delete this section on 13 January 2027

Do not write Python for me. Explain the concept, show a two-line illustrative
snippet, and let me write the implementation. Review what I write and name
what is weak.

I am learning Python properly from the ground up, not refreshing it. A cold
diagnostic came back short, so do not assume I know something because I have
18 years in IT. Check rather than assume.

Do not suggest DeepEval, Promptfoo, RAGAS or Langfuse during Stage A. They are
in the stack above for context only and are not touched until Stage B, which
starts 13 January 2027.

Every wrong prediction I make goes in practice/GAPS.md. If you spot a
misconception I have not recorded, say so and tell me to add it.
