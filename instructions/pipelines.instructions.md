---
description: Forward-looking architecture guidance for new work under pipelines/
applyTo: "pipelines/**"
---

# Pipelines Direction

`pipelines/` is the forward-looking home for new ingestion, orchestration, and crawler architecture in this repo.

## Default stance

- Put new production-oriented pipeline code here.
- Treat `py/`, `mediawiki/`, `scrapy/`, and root upload scripts as legacy reference unless the task explicitly targets them.
- Reuse ideas and field knowledge from legacy code, but do not copy legacy structure by default.

## Architecture direction

- Prefer Dagster-oriented structure for orchestration code here.
- Keep core transformation logic in plain Python modules that Dagster assets or jobs call.
- Keep source-specific scraping and ingestion logic behind explicit adapters or source modules.
- Scrapy can be one ingestion engine here, but do not let Scrapy concerns leak into unrelated transformation modules.

## Legacy code usage

- Use `py/` to learn current record shapes, extraction behavior, and incremental-state concerns.
- Use `mediawiki/` for source-specific extraction examples and field interpretation.
- Use `scrapy/` for legacy crawling patterns, but do not extend that project by default.
- When porting behavior from legacy code, preserve the behavior that matters and re-express it in the new architecture.

## Design expectations

- Keep serialized keys and document shapes snake_case unless a compatibility boundary requires otherwise.
- Make orchestration boundaries explicit.
- Keep external-system access, retries, and credentials behind clear resource or client layers.
- Keep validation and run commands local to this new architecture and document them near the new code as they appear.
