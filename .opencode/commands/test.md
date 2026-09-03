---
description: Run a TDD or bug reproduction workflow with honest verification and no automatic commit.
agent: mandor
subtask: false
---

Use `test-driven-development`; for browser behavior also use `browser-testing-with-devtools` when its MCP tools are configured and authorized.

Test request: `$ARGUMENTS`

Confirm the session mode, load relevant memory/rules, inspect the working tree, and read the actual implementation and test targets.

For new behavior: write a failing test, verify RED, implement the minimum change, verify GREEN, then refactor. For bugs: first reproduce the bug with a test that fails for the expected reason, implement the root-cause fix, then run relevant regression tests.

Hard-stop on any important decision not approved by the user. Follow namespace/class-first and the Doxygen completion gate for public API changes. Report each test/build/browser check as `verified directly`, `verified from provided evidence`, or `not verified`.

Review according to the selected workflow level and update memory only when project knowledge changed materially. Do not commit, push, merge, tag, release, or deploy without separate explicit approval.
