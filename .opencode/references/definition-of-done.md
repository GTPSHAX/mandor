# Definition of Done

This file is the shared project-wide completion gate. Apply it proportionally to the workflow level selected by Mandor. Task-specific acceptance criteria remain authoritative.

## Workflow-aware gates

- **Quick:** scope, actual target files, correctness, smallest relevant verification, working-tree preservation, and authority are required. Self-review is sufficient. Documentation and memory apply only when affected.
- **Normal:** all applicable gates below are required, with targeted verification and targeted review. A separate reviewer is required when behavior, public API, or multiple production files change.
- **Full:** all gates below are required. Six-axis review is separate from implementation, and security-sensitive work also requires security audit.

`Not applicable` is valid only with a concrete reason. Do not perform a check merely to fill a template.

## Required gates

1. **Scope** - The result stays within the user-approved requirement and decisions. No unrelated cleanup, dependency, contract, destructive action, or external operation was added.
2. **Actual files** - Every edited or reviewed target file was read directly. Any conflict between memory and actual files was reported, and actual files were used as evidence of current state.
3. **Correctness** - Relevant acceptance criteria and error paths were checked. Behavior changes have tests when testing is applicable.
4. **Structure** - Namespace/class-first was followed where supported, without empty wrapper classes. Any material deviation has explicit user approval.
5. **Documentation** - Public APIs created or changed satisfy the language-appropriate documentation contract. C-like Doxygen targets pass the inventory, tag, signature, and accuracy checks.
6. **Verification** - Relevant tests, build, lint, type checks, runtime checks, and Doxygen were run when available and authorized. Each result is labeled `verified directly`, `verified from provided evidence`, or `not verified`; unavailable checks are not invented.
7. **Review** - Review depth matches the workflow level. Normal/Full work has no unresolved blocking finding; Full work uses the six-axis contract. Security-sensitive work has no unresolved Critical or High security finding.
8. **Working tree** - User changes were preserved. No broad staging, destructive reset, or unrelated file absorption occurred.
9. **Authority** - No commit, push, merge, tag, release, deploy, migration, or external-service change occurred without explicit approval for that exact operation.
10. **Memory** - Material project knowledge was synchronized through `memorize`. Shared todo status was synchronized only when a persisted todo was created. Cosmetic/local Quick changes do not require a memory transaction.

## Incomplete verification

A task may be reported as partially complete when a check cannot run, but the report must name the missing check, why it was not run, and the resulting risk. Do not convert `not verified` into a pass.
