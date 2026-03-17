# Repository-wide Copilot Instructions

## What this file is for

This file is the repo-wide base instruction layer.

Use it for:

- repository context
- architecture understanding
- global reasoning and writing expectations
- safety and correctness defaults

More specific instructions live under `.github/instructions/` and apply by file pattern or task type.
When a more specific instruction file applies, follow that file for the scoped task and use this file as the fallback base.

Do not duplicate specialized commit, changelog, or file-pattern-specific rules here unless they are truly repo-wide.

## What this project is

This repo is a mixed scraping and data-loading workspace, not a single library.

- `pipelines/` is the intended home for new pipeline and orchestration work going forward. It is currently empty, which means new architecture work should start there instead of extending older paths by default.
- `py/` contains an older Hamilton-based MediaWiki pipeline. Use it as reference material unless a task explicitly asks you to maintain, migrate, or extract behavior from it.
- `py/profiles/` shows wiki-specific interpretation patterns from the older pipeline and is useful as reference when designing new classification or profile layers.
- `mediawiki/` contains standalone exporter scripts with their own state and resume logic. Treat them as legacy source-specific reference implementations.
- `scrapy/` contains an older Scrapy project with one custom spider. Treat it as legacy reference for crawling patterns, not the default destination for new crawler code.
- `upload_*.ts` and `_arango_script_utils.ts` are standalone Deno scripts for loading JSONL into ArangoDB.

Do not describe the current codebase as already Dagster-based. `dagster` packages exist in `py/requirements.txt`, but the checked-in implementation does not yet expose Dagster `Definitions`, assets, jobs, schedules, or sensors. The direction of travel is toward Dagster and Scrapy in `pipelines/`, not more Hamilton code in `py/`.

There is no active root `pipeline/` directory in the workspace today. Use `pipelines/` for new work unless the task explicitly targets a legacy path.

## Working defaults

- Preserve snake_case JSON and record shapes unless a task explicitly requires a schema change.
- Put new pipeline, orchestration, and crawler architecture in `pipelines/`.
- Treat `py/`, `mediawiki/`, `scrapy/`, and the upload scripts as legacy reference material unless a task explicitly asks for maintenance there.
- When reusing logic from legacy code, port the behavior intentionally instead of copying the old structure wholesale.
- Keep orchestration boundaries explicit. New orchestration should be Dagster-oriented in `pipelines/`; older Hamilton boundaries in `py/` are useful reference, not the default design target.
- When changing legacy incremental or resume behavior, reason carefully about SQLite state, recentchanges overlap, append-only JSONL outputs, and Scrapy JOBDIR semantics.
- For new crawler work, move toward Scrapy-backed ingestion under `pipelines/` rather than extending the legacy `scrapy/` project by default.

## Writing and explanation style

- Use familiar language that a JavaScript or TypeScript developer with about 2 to 3 years of experience would likely understand on first read.
- Do not assume parser, compiler, or formal language theory background unless the task clearly requires it.
- Do not replace one abstract phrase with another abstract phrase and call it clarity.
- Do not swap one hard word for another and call that plain English.
- When explaining a hard idea, start with concrete technical behavior from this repo or task.
- Ground explanations in at least one concrete anchor such as:
  - a real code path
  - a concrete input or output
  - a token or marker sequence
  - a bug or failure mode
  - a performance or allocation cost
  - a downstream effect for callers, maintainers, or consumers

- Explain what happens first, then why it matters here, then introduce the technical name only if it still helps.
- If a technical term such as `lexical`, `invariant`, or `delimiter` is necessary, explain what it means in this codebase and why it matters here.
- Use a real-world metaphor only when direct technical grounding still is not enough.
- Keep metaphors brief and accurate.
- After using a metaphor, return to the real technical behavior before moving on.
- Diagrams must match real behavior in the implementation or spec. Do not simplify them into something that teaches the wrong thing.
- Avoid em dashes in prose.

## Default operating mode

- Be explicit and high-signal.
- Prefer the smallest correct change first.
- Do not invent files, APIs, config, behavior, or guarantees that are not visible in the repo.
- If something is unclear, state the assumption and give a concrete verification step.
- Prefer established standards and conventions when they fit the problem.
- Call out trade-offs when multiple valid approaches exist.
- Optimize for maintainability, clarity, reproducibility, and educational value.
- Verify diagrams, examples, and explanatory claims against the implementation before presenting them as fact.

## Safety and correctness

- Default to least privilege.
- Avoid unsafe patterns such as string-built SQL, unsafe eval, weak crypto, or hidden trust assumptions.
- Do not leak secrets or credentials in logs, examples, or test fixtures.
- Call out trust boundaries around auth, permissions, parsing, and untrusted input.
- Respect remote sites when changing crawlers or API clients. Keep delays, retries, and robots-related behavior explicit and conservative unless the task requires otherwise.

## Python defaults

- Prefer idiomatic Python: snake_case names, `pathlib.Path` for new path handling, and small functions with explicit inputs and outputs.
- Add type hints where they clarify module boundaries, record shapes, or non-obvious return values. Do not add noisy annotations to every local variable.
- In legacy Hamilton code, keep nodes deterministic and side-effect-light. Helpers that should stay out of the DAG should keep a leading underscore.
- In new Dagster-oriented code under `pipelines/`, keep assets, resources, schedules, and sensors thin over plain Python logic.
- Put wiki-specific or source-specific interpretation behind explicit profile or adapter boundaries rather than scattering it across shared modules.
- For Scrapy spiders, always `yield` or `return` Requests, validate selectors with `scrapy shell` when extraction is uncertain, and use item pipelines only for real processing.

## Validation

- No automated test suite is currently visible in the repo, and `deno.jsonc` does not define any tasks.
- For legacy Hamilton pipeline work, install `py/requirements.txt`, work from `py/`, and use `python run.py --test` as the documented end-to-end sanity check.
- For legacy Scrapy work, install `scrapy/requirements.txt`, work from `scrapy/`, and prefer a small crawl slice such as `scrapy crawl tvtropes_tropes -a page_start=1 -a page_end=1 -O output/sample.jsonl` before broader runs.
- For new code under `pipelines/`, document and prefer the validation path that ships with that new code instead of inheriting legacy commands by assumption.
- For Deno upload scripts, run the script directly with `deno run ...`; do not assume repo-wide `deno task` helpers exist.

## Instruction routing

More specific instructions live under `.github/instructions/` and apply by file pattern or task type. Follow those files when they apply.

Examples:

- docs-writing instructions for docs, comments, and TSDoc work
- pipelines instructions for new work under `pipelines/`
- python instructions for `**/*.py`
- commit-writing instructions for commit messages
- changelog-writing instructions for changelog entries and release notes
