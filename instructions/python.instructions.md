---
description: Generic Python standards
applyTo: "**/*.py"
---

# Python Rules

Prefer idiomatic Python. Use `snake_case` names, small functions with explicit inputs and return values, and clear module boundaries.

Prefer `pathlib.Path` for new filesystem work unless the project has a strong reason not to.

Add type hints where they clarify interfaces, record shapes, or non-obvious return values. Do not add noisy annotations to every local variable.

Avoid hidden module state when explicit inputs and outputs make the code easier to reason about.

Keep boundary code thin. Put orchestration, I/O, external-system access, retries, credentials, and side effects behind clear functions, clients, or adapters. Keep core business logic importable and testable on its own.

Preserve public shapes and serialized keys unless a migration is intentional.

When extraction, parsing, normalization, or data cleanup is uncertain, verify against real inputs before hard-coding fallback behavior.

Use comments and docstrings to explain tricky transforms, assumptions, invariants, external I/O behavior, lifecycle rules, edge cases, and any deliberate tradeoffs that are not obvious from the code itself.