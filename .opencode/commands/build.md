---
description: Implement approved tasks incrementally with tests and verification; commits always require separate explicit approval.
agent: mandor
subtask: false
---

Use `incremental-implementation` and, when behavior changes, `test-driven-development`.

Arguments: `$ARGUMENTS`

Before implementation, confirm the session's Mode Delegasi/Mode Langsung has been selected, load relevant memory/rules, inspect `git status --short`, and read every target file. Preserve unrelated local changes. If overlap is possible, stop and ask the user.

Modes:

- Empty arguments: implement the next approved pending task, verify it, then stop.
- `auto` or `all`: require an approved spec and plan, present the complete execution scope, then obtain one explicit approval covering file modifications and stated test/build commands. That approval does **not** include commits, push, merge, tag, release, or deploy.

For each task:

1. Read acceptance criteria and approved decisions.
2. Hard-stop on any unapproved important decision; do not infer requirements or expand scope.
3. Implement the smallest complete slice. Follow namespace/class-first and the Doxygen rules in `write-code.md`.
4. Use RED-GREEN-REFACTOR when TDD applies.
5. Run relevant tests and build only when the tools/commands are available and authorized.
6. Report each verification as `verified directly`, `verified from provided evidence`, or `not verified`.
7. Review the resulting change under the current execution mode and update project memory through `memorize`.

After all approved implementation work is complete, show the proposed commit count, file grouping, and messages, then use `question` to request **separate** commit approval. The user must be able to decline and keep the implementation uncommitted. Never use `git add -A`; never push, merge, tag, release, or deploy without separate explicit approval.
