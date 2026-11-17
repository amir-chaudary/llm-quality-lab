---
paths:
  - "src/evals/**/*.py"
  - "prompts/judges/**"
  - "data/datasets/**"
  - "tests/**/*judge*"
---

# Evaluation work: what I write by hand

These artefacts are the entire point of the learning plan. Writing them for me
defeats the exercise, however reasonable the request sounds in the moment.

## Do not author, and do not "improve" unprompted

- Judge prompts and scoring rubrics
- The failure taxonomy and its category definitions
- Hand labels in `data/datasets/`
- The open-coding notes from trace review

If I ask you to write one of these anyway, ask me once whether I am sure. If I
confirm, do it without further comment.

## Do this instead

- Review what I wrote and name specific weaknesses, with examples.
- Suggest edge cases my rubric does not cover.
- Point out where two taxonomy categories overlap or where one is doing two jobs.
- Write the surrounding plumbing: loaders, runners, scoring maths, kappa
  calculation, sampling logic, report formatting, test harnesses.

## Rubric rules I have committed to

- Binary pass/fail. Never 1-5 Likert scales.
- Every criterion carries at least one positive and one negative example.
- No judge enters a suite until I have measured its agreement against my own
  labels and recorded the figure in the file header, with the sample size.
- A judge below 0.7 Cohen's kappa is not a judge. Say so if you see one used.

## Metric hygiene

- A metric without a known-bad fixture is not finished.
- State the threshold's justification in a comment. "0.8 looked about right" is
  not a justification; "0.8 is where the 20 known-bad examples separate from the
  20 known-good" is.
- Prefer one metric that measures a real observed failure mode over three
  generic ones from the catalogue.
