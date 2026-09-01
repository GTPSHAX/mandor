---
description: Produce a user-reviewed implementation plan with acceptance criteria, dependencies, and decision gates.
agent: mandor
subtask: false
---

Use `planning-and-task-breakdown`.

Planning request: `$ARGUMENTS`

Confirm the session mode, load relevant memory/rules, and read the actual spec plus only relevant codebase sections. Planning is read-only until the user approves saving the plan.

1. Identify requirements, dependency graph, existing boundaries, and memory gaps.
2. Apply namespace/class-first without creating empty wrappers.
3. List every important unresolved decision as `DECISION REQUIRED`; do not select architecture, framework, database, dependency, public contract, or scope for the user.
4. Slice work into small tasks with acceptance criteria, verification steps, dependencies, likely files, Doxygen requirements for public API, and explicit checkpoints.
5. Present the plan for user review.
6. Only after approval, save it to `tasks/plan.md` and the checklist to `tasks/todo.md`.

Do not implement code, commit, push, merge, tag, release, or deploy.
