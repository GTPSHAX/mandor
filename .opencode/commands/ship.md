---
description: Produce a pre-launch GO or NO-GO assessment from code and security review without deploying.
agent: mandor
subtask: false
---

Use `shipping-and-launch`.

Ship assessment scope: `$ARGUMENTS`

This command assesses readiness only. It never commits, pushes, merges, tags, releases, deploys, runs migrations, or changes external services without separate explicit approval.

Confirm the session mode, load relevant memory/rules, inspect the actual change, and preserve unrelated working-tree changes.

In **Mode Delegasi**, invoke `code-reviewer` and `security-auditor` concurrently with complete context. Keep fan-out flat; neither specialist invokes another specialist. In **Mode Langsung**, Mandor performs the same two independent passes directly without inventing persona reports.

Synthesize:

1. Six-axis code quality review, including blocking Doxygen compliance.
2. Security findings and threat boundaries.
3. Test/build evidence actually available; there is no `test-engineer`, so do not claim a separate coverage report.
4. Performance, accessibility, infrastructure, and documentation only to the extent directly verified or supported by provided evidence.
5. A rollback plan.

Return `GO` only when no Critical/Important code-review finding, no Critical/High security finding, and no Doxygen `FAIL` remains. Label every verification `verified directly`, `verified from provided evidence`, or `not verified`. A GO/NO-GO assessment is not permission to ship.
