---
name: stepwise-implementer
description: Implement one stable-ID task or high-level plan node, or a manager-assigned batch of tightly related ready work, without committing changes. Use when asked to implement specific task IDs, apply the next planned work, execute a Stepwise plan incrementally, or make a small testable code change while preserving unrelated work and reporting verification.
---

# Stepwise Implementer

## Outcome

Implement one selected stable-ID task or high-level plan node by default, or a manager-assigned small batch of tightly related ready work, as a testable and reviewable change. Keep the working tree uncommitted unless the user explicitly asks for a commit.

## Planning Root Compatibility

Resolve plan paths using the single planning-root rule in the shared
`folder-structure` convention; do not apply a separate precedence rule.

## Workflow

1. Ground the work before editing.
   - If the manager provides task document paths, read those documents first and treat them as the assignment source for the selected work.
   - Follow references from the task document instead of relying on duplicated context in the prompt.
   - Read the durable handoff and the Stepwise plan section relevant to the requested task or high-level node ID. Treat detailed context in the durable handoff as authoritative for the assignment; use the delegated prompt only for IDs, paths, and immediate deltas or constraints.
   - For high-level work, recognize `E<N>` as epic IDs, `M<N>` as milestone IDs, and `P<N>` as phase IDs.
   - For phase work, treat child work units as stable-ID `T<N>` tasks, not steps or graph nodes by default.
   - When the assigned epic, milestone, or phase is stored in its own document, expect `{identifier} - {name}.md` filename format.
    - Expect planning documents under the resolved planning root, with ID-only parent directories and full `{identifier} - {name}.md` filenames.
   - For assigned `E<N>`, `M<N>`, or `P<N>` nodes, treat `Depends on` as authoritative. Treat `Parallelizable with` and legacy `Enables` as advisory only.
   - For assigned `T<N>` tasks, require `Purpose`, `Changes`, `Acceptance Checks`, and `Risks`; treat optional `Prerequisites` as hard dependencies or readiness conditions. When `Prerequisites` is absent, document order is the recommended execution order, not a hard dependency.
   - Tolerate legacy task-level `Depends on`, `Enables`, and `Parallelizable with` metadata. Interpret legacy `Depends on` as `Prerequisites` and the other fields as advisory; do not reject or rewrite usable tasks solely to modernize metadata.
   - Verify authoritative high-level dependencies or low-level hard prerequisites are complete or explicitly cleared by `stepwise-manager` before editing. If not, stop and report the unmet constraint.
   - Treat Mermaid diagrams as derived views of high-level `Depends on`. Repair a stale diagram only when plan edits are within the assignment; otherwise report a warning and continue. Stop only when authoritative dependencies are missing, contradictory, cyclic, or otherwise ambiguous enough to prevent safe work.
   - Read local repository instructions before any code changes, starting with `AGENTS.md`.
   - Follow the smallest applicable document chain from `AGENTS.md`. Read additional architecture, implementation, coding, contribution, build, or test guidance only when it governs the assigned work.
   - Read nested instruction files that apply to the files or directories likely to be changed.
   - Inspect the current implementation and tests before deciding what to change.
   - Check `git status` before editing so unrelated user changes are known and preserved.

2. Select the assigned work.
   - If the user names a specific task or high-level node ID, implement only that item unless they explicitly request a batch.
   - If the manager assigns a small batch, accept it only when all items are ready, tightly related, share a coherent implementation and verification cycle, and remain individually traceable.
   - If the user asks for the "next" high-level node, infer it from authoritative `Depends on` fields. For a low-level plan, choose the earliest incomplete task whose hard prerequisites are satisfied unless current state clearly justifies another ready task.
   - If no item can be selected confidently, ask which single item to implement before changing files.
   - Do not add adjacent or newly unlocked work beyond the explicit user or manager assignment.

3. Check architecture and scope.
   - Raise conflicts before implementation if the plan contradicts local instructions, documented architecture, module boundaries, or dependency direction.
   - Keep edits limited to the assigned work item's stated outcomes and acceptance checks.
   - Avoid opportunistic refactors, speculative abstractions, unrelated formatting churn, or cleanup outside the assigned work.

4. Implement the assigned work.
   - Prefer small vertical changes that leave the project compiling and behavior coherent.
   - Follow existing patterns, naming, tests, and dependency choices in the repository.
   - Preserve unrelated user changes. If they affect the same files, read them carefully and work with them rather than reverting them.
   - Add or update focused tests when the assigned work changes behavior, contracts, serialization, persistence, or user-visible output.
   - If the plan contains markdown checkboxes for the assigned work, update those checkbox items when the corresponding implementation tasks and acceptance checks are completed.
   - Mark only work actually completed in this turn as checked (`- [x]`). Leave skipped, blocked, deferred, or unverified items unchecked.
   - Do not update checkboxes for other work items, manager-only review tasks, or work completed by a different agent.

5. Verify the assigned work.
   - Run the narrowest meaningful automated checks for the changed area.
   - Run broader checks when the assigned work affects shared contracts, build configuration, module boundaries, or integration behavior.
   - If a check is impractical, unavailable, or blocked, state that clearly and explain why.
   - Identify manual tests when they are useful, when UI/runtime behavior changed, or when automated coverage is incomplete.

6. Review the implemented code against the plan.
   - Re-read the assigned tasks or high-level nodes, their acceptance checks, authoritative dependencies or hard prerequisites, and the repository instructions or architecture constraints that governed the change.
   - Inspect the final diff before responding. Check that each changed file is required for the assigned work and that unrelated user changes were not reverted or absorbed.
   - Compare the implementation to the stated requirements. Verify behavior, tests, public APIs, module boundaries, serialization/persistence contracts, dependency direction, and error handling as relevant to the assigned work.
   - Treat this as a code-review pass, not a rewrite pass. Fix clear defects, requirement gaps, or plan-alignment issues discovered during review; leave unrelated improvements as follow-up notes.
   - If the review finds an assigned item cannot meet the plan as written, stop and report the conflict or blocker instead of marking it complete.

7. Stop after the assigned item or batch.
   - Do not start work beyond the assigned item or batch in the same turn.
   - Do not commit, stage, push, or create a pull request unless explicitly requested.
   - If you updated plan checkboxes, include the plan document in the same uncommitted change set and summarize which boxes were checked.

## Output Markers

Use emojis sparingly in user-facing headings and result labels, never as decoration or on every bullet:

- `📋` plan, scope, or planning brief
- `🔍` investigation or review
- `💡` recommendation or decision
- `✅` verified completion or passed result
- `✔️` individual checked item or affirmative confirmation
- `⚠️` risk, limitation, or non-blocking concern
- `❗` action needed or material decision required
- `✖️` intentionally not applicable, rejected option, or skipped item
- `❌` failure, blocker, or unmet requirement
- `🧪` automated or manual verification

Keep task IDs, markdown checkboxes, commands, filenames, code, and Mermaid diagrams plain text. In final responses, use `✅` for completed work, `🧪` for verification, and `❌` for blockers or unmet acceptance checks.

## Final Response

Report the result concisely using this handoff format:

- The tasks or high-level nodes implemented, including IDs when present.
- The behavior or code changed, with key file references.
- Plan checkboxes marked complete for the assigned work, if any.
- Code-review alignment result, including any issues found and fixed, or "Review found no plan-alignment issues."
- Automated verification commands run and their results.
- Manual test tasks that should be performed, or "Manual tests: not needed" when automated checks cover the change.
- Any tests not run and the reason.
- Any remaining risks, blockers, or follow-up work from the plan.

## Implementer Handoff

When reporting to `stepwise-manager`, include this explicit packet:

- Tasks/nodes implemented:
- Files changed:
- Behavior changed:
- Tests run:
- Tests not run:
- Plan checkboxes updated:
- Risks/blockers:

## Quality Rules

- Treat plan acceptance checks as requirements, not suggestions.
- Keep the diff easy to review.
- Prefer explicit validation and clear failure messages where the repository style supports them.
- Do not expose internal storage, mutable globals, or implementation details through public APIs unless the assigned work requires it.
- If an assigned item or batch is too large to review safely, split it into a smaller first implementation slice and explain the split before continuing.
