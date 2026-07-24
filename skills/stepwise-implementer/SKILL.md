---
name: stepwise-implementer
description: Implement one small, reviewable graph node from a stepwise dependency-graph plan without committing changes. Use when asked to implement a specific task ID or node ID from stepwise-planner output, apply the next planned dependency graph node, execute a markdown implementation plan incrementally, or make a small testable code change while preserving unrelated work and reporting automated and manual verification.
---

# Stepwise Implementer

## Outcome

Implement exactly one selected plan graph node as a small, testable, reviewable change. Keep the working tree uncommitted unless the user explicitly asks for a commit.

## Workflow

1. Ground the work before editing.
   - If the manager provides a task document path, read that task document first and treat it as the assignment source for the selected node.
   - Follow references from the task document instead of relying on duplicated context in the prompt.
   - Read the stepwise plan section relevant to the requested task ID or node ID.
   - For high-level work, recognize `E<N>` as epic IDs, `M<N>` as milestone IDs, and `P<N>` as phase IDs.
   - For phase work, treat child work units as `T<N>` task nodes, not steps.
   - When the assigned epic, milestone, or phase is stored in its own document, expect `{identifier} - {name}.md` filename format.
   - Expect graph documents to follow `work\{epic}.md`, `work\{epic-id}\{milestone}.md`, `work\{epic-id}\{milestone-id}\{phase}.md`, and `work\{epic-id}\{milestone-id}\{phase-id}\{task}.md`.
   - In document hierarchy paths, treat `{epic-id}`, `{milestone-id}`, and `{phase-id}` as ID-only directory names such as `E1`, `M1`, and `P1`; treat `{epic}`, `{milestone}`, `{phase}`, and `{task}` as full `{identifier} - {name}.md` filenames.
   - Read the selected node's `Depends on`, `Enables`, and `Parallelizable with` fields when present.
   - Verify the selected node's dependencies are complete or explicitly cleared by `stepwise-manager` before editing. If not, stop and report the unmet dependency.
   - Read local repository instructions before any code changes, starting with `AGENTS.md`.
   - Follow every relevant document reference from `AGENTS.md` before any code changes, including architecture, implementation-plan, implementation-guideline, coding-guideline, contribution, build, and test documents when referenced.
   - Read nested instruction files that apply to the files or directories likely to be changed.
   - Inspect the current implementation and tests before deciding what to change.
   - Check `git status` before editing so unrelated user changes are known and preserved.

2. Select one graph node.
   - If the user or manager names a specific task ID or node ID, implement only that node.
   - If the user asks for the "next" node, infer it from the plan dependency fields and current code state when possible.
   - If no node can be selected confidently, ask which single node to implement before changing files.
   - Do not batch adjacent or newly unlocked nodes unless the user explicitly requests that.

3. Check architecture and scope.
   - Raise conflicts before implementation if the plan contradicts local instructions, documented architecture, module boundaries, or dependency direction.
   - Keep edits limited to the selected node's stated outcome and acceptance checks.
   - Avoid opportunistic refactors, speculative abstractions, unrelated formatting churn, or cleanup outside the node.

4. Implement the node.
   - Prefer small vertical changes that leave the project compiling and behavior coherent.
   - Follow existing patterns, naming, tests, and dependency choices in the repository.
   - Preserve unrelated user changes. If they affect the same files, read them carefully and work with them rather than reverting them.
   - Add or update focused tests when the node changes behavior, contracts, serialization, persistence, or user-visible output.
   - If the plan contains markdown checkboxes for the selected node, update those checkbox items as part of the implemented node when the corresponding implementation tasks and acceptance checks are completed.
   - Mark only work actually completed in this turn as checked (`- [x]`). Leave skipped, blocked, deferred, or unverified items unchecked.
   - Do not update checkboxes for other nodes, manager-only review tasks, or work completed by a different agent.

5. Verify the node.
   - Run the narrowest meaningful automated checks for the changed area.
   - Run broader checks when the node affects shared contracts, build configuration, module boundaries, or integration behavior.
   - If a check is impractical, unavailable, or blocked, state that clearly and explain why.
   - Identify manual tests when they are useful, when UI/runtime behavior changed, or when automated coverage is incomplete.

6. Review the implemented code against the plan.
   - Re-read the selected plan node, its acceptance checks, dependency fields, and any repository instructions or architecture constraints that governed the change.
   - Inspect the final diff before responding. Check that each changed file is required for the selected node and that unrelated user changes were not reverted or absorbed.
   - Compare the implementation to the stated requirements. Verify behavior, tests, public APIs, module boundaries, serialization/persistence contracts, dependency direction, and error handling as relevant to the node.
   - Treat this as a code-review pass, not a rewrite pass. Fix clear defects, requirement gaps, or plan-alignment issues discovered during review; leave unrelated improvements as follow-up notes.
   - If the review finds the selected node cannot meet the plan as written, stop and report the conflict or blocker instead of marking the node complete.

7. Stop after one node.
   - Do not start the next plan node in the same turn unless the user asks.
   - Do not commit, stage, push, or create a pull request unless explicitly requested.
   - If you updated plan checkboxes, include the plan document in the same uncommitted change set and summarize which boxes were checked.

## Final Response

Report the result concisely using this handoff format:

- The selected plan node implemented, including node ID when present.
- The behavior or code changed, with key file references.
- Plan checkboxes marked complete for the selected node, if any.
- Code-review alignment result, including any issues found and fixed, or "Review found no plan-alignment issues."
- Automated verification commands run and their results.
- Manual test tasks that should be performed, or "Manual tests: not needed" when automated checks cover the change.
- Any tests not run and the reason.
- Any remaining risks, blockers, or follow-up work from the plan.

## Implementer Handoff

When reporting to `stepwise-manager`, include this explicit packet:

- Node implemented:
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
- Do not expose internal storage, mutable globals, or implementation details through public APIs unless the selected node requires it.
- If the selected node is too large to review safely, split it into a smaller first implementation slice and explain the split before continuing.
