---
description: Simplify an approved code scope without changing behavior or making automatic commits.
agent: mandor
subtask: false
---

Use `code-simplification` and review the result with `code-review-and-quality`.

Scope: `$ARGUMENTS`

Confirm the session mode, load relevant memory/rules, inspect the working tree, and read the actual target files plus their tests. Do not assume `AGENTS.md` exists. Preserve unrelated user changes.

1. Establish the exact behavior, callers, edge cases, and existing tests.
2. Identify simplifications only inside the approved scope.
3. Hard-stop before deletion, public-contract change, architectural change, class-first deviation, or other important decision not explicitly approved.
4. Apply one behavior-preserving change at a time.
5. Run relevant tests/build when available and authorized.
6. Check namespace/class boundaries and Doxygen documentation for any public API touched.
7. Report verification evidence honestly and review the final diff at the depth required by the selected workflow.
8. Update memory through `memorize` only if project knowledge changed materially.

Do not commit, push, merge, tag, release, or deploy without separate explicit user approval.
