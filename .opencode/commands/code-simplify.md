---
description: Simplify an approved code scope without changing behavior or making automatic commits.
agent: mandor
subtask: false
---

Use `code-simplification`. Load `code-review-and-quality` only if the scope is Normal/Full or the user requests formal review.

Scope: `$ARGUMENTS`

Inspect the working tree and read the actual target files plus relevant tests. Load memory/rules only when needed. Preserve unrelated user changes.

1. Establish the exact behavior, callers, edge cases, and existing tests.
2. Identify simplifications only inside the approved scope.
3. Hard-stop before deletion, public-contract change, architectural change, or another material decision not explicitly approved.
4. Apply one behavior-preserving change at a time.
5. Run relevant tests/build when available and authorized.
6. Check affected public API documentation only when public API changed.
7. Report verification evidence honestly and review the final diff at the depth required by the selected workflow.
8. Use `project-memory` only if project knowledge changed materially.

Do not commit, push, merge, tag, release, or deploy without separate explicit user approval.
