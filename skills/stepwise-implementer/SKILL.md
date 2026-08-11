---
name: stepwise-implementer
description: Implement one small graph node or a manager-assigned batch of tightly related ready nodes from a stepwise dependency-graph plan without committing changes. Use when asked to implement specific task IDs, apply the next planned dependency-graph work, execute a markdown plan incrementally, or make a small testable code change while preserving unrelated work and reporting verification.
---

# Stepwise Implementer

## Outcome

Implement one selected plan graph node by default, or a manager-assigned small batch of tightly related ready nodes, as a testable and reviewable change. Keep the working tree uncommitted unless the user explicitly asks for a commit.

## Planning Root Compatibility

Resolve plan paths using the shared `folder-structure` convention.
Use `docs/plans/` for new projects, fall back to an existing `docs/work/` or
root-level `work/` directory for compatibility, never create a new `work/`
directory, and stop if multiple planning roots exist.

## Workflow

1. Ground the work before editing.
   - If the manager provides task document paths, read those documents first and treat them as the assignment source for the selected work.
   - Follow references from the task document instead of relying on duplicated context in the prompt.
   - Read the stepwise plan section relevant to the requested task ID or node ID.
   - For high-level work, recognize `E<N>` as epic IDs, `M<N>` as milestone IDs, and `P<N>` as phase IDs.
   - For phase work, treat child work units as `T<N>` task nodes, not steps.
   - When the assigned epic, milestone, or phase is stored in its own document, expect `{identifier} - {name}.md` filename format.
    - Expect graph documents under the resolved planning root, with ID-only parent directories and full `{identifier} - {name}.md` filenames.
   - Read each assigned node's `Depends on`, `Enables`, and `Parallelizable with` fields when present.
   - Verify every assigned node's dependencies are complete or explicitly cleared by `stepwise-manager` before editing. If not, stop and report the unmet dependency.
   - Read local repository instructions before any code changes, starting with `AGENTS.md`.
   - Follow the smallest applicable document chain from `AGENTS.md`. Read additional architecture, implementation, coding, contribution, build, or test guidance only when it governs the assigned work.
   - Read nested instruction files that apply to the files or directories likely to be changed.
   - Inspect the current implementation and tests before deciding what to change.
   - Check `git status` before editing so unrelated user changes are known and preserved.

2. Select the assigned graph work.
   - If the user names a specific task ID or node ID, implement only that node unless they explicitly request a batch.
   - If the manager assigns a small batch, accept it only when all nodes are ready, tightly related, share a coherent implementation and verification cycle, and remain individually traceable.
   - If the user asks for the "next" node, infer it from the plan dependency fields and current code state when possible.
   - If no node can be selected confidently, ask which single node to implement before changing files.
   - Do not add adjacent or newly unlocked nodes beyond the explicit user or manager assignment.

3. Check architecture and scope.
   - Raise conflicts before implementation if the plan contradicts local instructions, documented architecture, module boundaries, or dependency direction.
   - Keep edits limited to the assigned nodes' stated outcomes and acceptance checks.
   - Avoid opportunistic refactors, speculative abstractions, unrelated formatting churn, or cleanup outside the assigned work.

4. Implement the node.
   - Prefer small vertical changes that leave the project compiling and behavior coherent.
   - Follow existing patterns, naming, tests, and dependency choices in the repository.
   - Preserve unrelated user changes. If they affect the same files, read them carefully and work with them rather than reverting them.
   - Add or update focused tests when the node changes behavior, contracts, serialization, persistence, or user-visible output.
   - If the plan contains markdown checkboxes for the assigned nodes, update those checkbox items when the corresponding implementation tasks and acceptance checks are completed.
   - Mark only work actually completed in this turn as checked (`- [x]`). Leave skipped, blocked, deferred, or unverified items unchecked.
   - Do not update checkboxes for other nodes, manager-only review tasks, or work completed by a different agent.

5. Verify the node.
   - Run the narrowest meaningful automated checks for the changed area.
   - Run broader checks when the node affects shared contracts, build configuration, module boundaries, or integration behavior.
   - If a check is impractical, unavailable, or blocked, state that clearly and explain why.
   - Identify manual tests when they are useful, when UI/runtime behavior changed, or when automated coverage is incomplete.

6. Review the implemented code against the plan.
   - Re-read the assigned plan nodes, their acceptance checks, dependency fields, and the repository instructions or architecture constraints that governed the change.
   - Inspect the final diff before responding. Check that each changed file is required for the assigned work and that unrelated user changes were not reverted or absorbed.
   - Compare the implementation to the stated requirements. Verify behavior, tests, public APIs, module boundaries, serialization/persistence contracts, dependency direction, and error handling as relevant to the assigned work.
   - Treat this as a code-review pass, not a rewrite pass. Fix clear defects, requirement gaps, or plan-alignment issues discovered during review; leave unrelated improvements as follow-up notes.
   - If the review finds an assigned node cannot meet the plan as written, stop and report the conflict or blocker instead of marking it complete.

7. Stop after the assigned node or batch.
   - Do not start work beyond the assigned node or batch in the same turn.
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

Keep task IDs, markdown checkboxes, commands, filenames, code, and Mermaid diagrams plain text. In final responses, use `✅` for completed nodes, `🧪` for verification, and `❌` for blockers or unmet acceptance checks.

## Final Response

Report the result concisely using this handoff format:

- The plan node or nodes implemented, including IDs when present.
- The behavior or code changed, with key file references.
- Plan checkboxes marked complete for the assigned nodes, if any.
- Code-review alignment result, including any issues found and fixed, or "Review found no plan-alignment issues."
- Automated verification commands run and their results.
- Manual test tasks that should be performed, or "Manual tests: not needed" when automated checks cover the change.
- Any tests not run and the reason.
- Any remaining risks, blockers, or follow-up work from the plan.

## Implementer Handoff

When reporting to `stepwise-manager`, include this explicit packet:

- Nodes implemented:
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
- If an assigned node or batch is too large to review safely, split it into a smaller first implementation slice and explain the split before continuing.
