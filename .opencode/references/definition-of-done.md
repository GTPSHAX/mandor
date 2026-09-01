# Definition of Done

This file is the shared project-wide completion gate. Task-specific acceptance criteria remain authoritative for the requested behavior; this gate adds the minimum evidence required before any agent reports a file-changing task complete.

## Required gates

1. **Scope** - The result stays within the user-approved requirement and decisions. No unrelated cleanup, dependency, contract, destructive action, or external operation was added.
2. **Actual files** - Every edited or reviewed target file was read directly. Any conflict between memory and actual files was reported, and actual files were used as evidence of current state.
3. **Correctness** - Relevant acceptance criteria and error paths were checked. Behavior changes have tests when testing is applicable.
4. **Structure** - Namespace/class-first was followed where supported, without empty wrapper classes. Any material deviation has explicit user approval.
5. **Documentation** - Public APIs created or changed satisfy the language-appropriate documentation contract. C-like Doxygen targets pass the inventory, tag, signature, and accuracy checks.
6. **Verification** - Relevant tests, build, lint, type checks, runtime checks, and Doxygen were run when available and authorized. Each result is labeled `verified directly`, `verified from provided evidence`, or `not verified`; unavailable checks are not invented.
7. **Review** - The six-axis review has no unresolved Critical or Important finding. Security-sensitive work has no unresolved Critical or High security finding.
8. **Working tree** - User changes were preserved. No broad staging, destructive reset, or unrelated file absorption occurred.
9. **Authority** - No commit, push, merge, tag, release, deploy, migration, or external-service change occurred without explicit approval for that exact operation.
10. **Memory** - Relevant project memory and shared todo status were synchronized through `memorize` after the final file-changing batch.

## Incomplete verification

A task may be reported as partially complete when a check cannot run, but the report must name the missing check, why it was not run, and the resulting risk. Do not convert `not verified` into a pass.
