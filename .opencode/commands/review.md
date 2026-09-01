---
description: Conduct a six-axis evidence-based review with a blocking Doxygen compliance gate.
agent: mandor
subtask: false
---

Use `code-review-and-quality`.

Review scope: `$ARGUMENTS`

Confirm the session mode, load relevant memory/rules, inspect the actual diff/files, and preserve a read-only review posture. In Mode Delegasi use `code-reviewer`; in Mode Langsung Mandor performs the same contract directly.

Review all six axes:

1. Correctness
2. Readability
3. Architecture
4. Security
5. Performance
6. Style & Conventions

Inventarisasi every public API created or changed. For each, report Doxygen `PASS`, `FAIL`, or reasoned `N/A`. Every `FAIL` is Important and forces `REQUEST CHANGES`. Categorize all findings as Critical, Important, or Suggestion and include file:line plus an actionable fix for blocking findings.

For tests, build, security checks, and Doxygen execution, report `verified directly`, `verified from provided evidence`, or `not verified`. Never fabricate execution. Do not edit or commit during review.
