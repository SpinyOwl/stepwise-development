---
name: stepwise-implementer
description: Implement one stable-ID task or high-level plan node, or a manager-assigned batch of tightly related ready work, without committing changes. Use when asked to implement specific task IDs, apply the next planned work, execute a Stepwise plan incrementally, or make a small testable code change while preserving unrelated work and reporting verification.
---

# Stepwise Implementer

## Outcome

Implement one selected stable-ID task or high-level plan node by default, or a manager-assigned small batch of tightly related ready work, as a testable and reviewable change. Keep the working tree uncommitted unless the user explicitly asks for a commit. Persist the implementation result in the assigned task's authoritative Execution Record before responding. When that task has a parent phase with a `Phase Implementation State` roll-up, keep its active work and phase-level blockers current without duplicating task detail; the final handoff summarizes those already-recorded states.

## Planning Root Compatibility

Resolve plan paths using the single planning-root rule in the shared
`folder-structure` convention, including a planning root declared in the
repository instruction file; do not apply a separate precedence rule. Read the
declared documentation index first and honor the declared `Profile`. Write artifacts and reply in chat in the languages set by the shared convention's language rule.

## Optional Graph Discovery

During repository discovery, read and apply the package-local
[Optional Graph Discovery Policy](references/optional-graph-discovery.md).
Graphify remains an optional candidate locator. If the policy or an already
available permitted index cannot be used, skip Graphify and inspect the
repository directly; implementation identity, status, evidence, and completion
still come only from authoritative sources.

## Workflow

1. Ground the work before editing.
   - If the manager provides task document paths, read those documents first and treat them as the assignment source for the selected work.
   - Follow references from the task document instead of relying on duplicated context in the prompt.
   - When an assigned standalone task belongs to a phase with a `Phase Implementation State` roll-up, read that parent phase document to establish its current progress, active work, and phase-level blockers before editing.
   - Read the durable handoff and the Stepwise plan section relevant to the requested task or high-level node ID. Treat detailed context in the durable handoff as authoritative for the assignment; use the delegated prompt only for IDs, paths, and immediate deltas or constraints.
   - Locate and read the one authoritative Execution Record for every assigned executable task before editing. Verify that it contains the nine required fields in the required order (under `Frontmatter: on`, the six body fields plus the frontmatter `status`, `updated`, and `review`) and that its current status, prerequisites, and recorded state are compatible with the assignment. Stop and report the conflict if the record is absent, duplicated, structurally invalid, or contradicts the authoritative task document in a way that prevents safe work.
   - Treat the task document and its Execution Record as the cross-session source of truth. Do not accept a delegated prompt that attempts to replace durable requirements or previously recorded state; an immediate delta may narrow or clarify the assignment only when it does not contradict the durable handoff.
   - Keep handoff context reference-first and bounded. Do not copy full conversations, raw evidence, private reasoning, or implementation traces into durable outputs.
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
   - Check `git status` before editing so unrelated user changes are known and preserved. If the project is not a Git repository, list the files likely to change and record in the Execution Record's `Decisions and Deviations` that no version-control baseline exists.

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
   - Apply clean-code priorities in this order: KISS, YAGNI, DRY, then SOLID.
     Prefer the smallest clear change for the current requirement; remove
     demonstrated duplication only when a shared solution stays simpler; use
     SOLID to correct a real responsibility or dependency issue, not to add
     layers, interfaces, or indirection by default.

4. Implement the assigned work.
   - Take writer ownership of the assigned task's implementation-owned Execution Record fields for this turn. Set `Status` to `In Progress` and refresh `Last Updated` (under `Frontmatter: on`, the frontmatter `status` and `updated`) before or with the first durable implementation update. Sequential work transfers this ownership between agents; parallel implementers must have separate task records and must never write the same record concurrently.
   - Prefer small vertical changes that leave the project compiling and behavior coherent.
   - Follow existing patterns, naming, tests, and dependency choices in the repository.
   - Keep control flow, data flow, and ownership easy to follow. Reuse an
     existing local abstraction when it fits; otherwise keep the solution
     direct. Do not create a reusable framework or extension point without a
     current, evidenced need.
   - Preserve unrelated user changes. If they affect the same files, read them carefully and work with them rather than reverting them.
   - Add or update focused tests when the assigned work changes behavior, contracts, serialization, persistence, or user-visible output.
   - If the plan contains markdown checkboxes for the assigned work, update those checkbox items when the corresponding implementation tasks and acceptance checks are completed.
   - Mark only work actually completed in this turn as checked (`- [x]`). Leave skipped, blocked, deferred, or unverified items unchecked.
   - Do not update checkboxes for other work items, manager-only review tasks, or work completed by a different agent.
   - Follow the repository's existing comment standard for the language in use, and document the public contracts and implementation boundaries of the assigned work. Documentation must match observed behavior.
   - Write the record in the field shapes defined by the shared convention: list fields as bullets or `None`, `Acceptance Evidence` as one table row per check with the checkbox text verbatim, `Resume or Closure` as one sentence starting `Resume by`. Under `Frontmatter: on`, change only `status` (never to `Completed`) and `updated` in the frontmatter, never `review`, `review_note`, `resolved`, `priority`, `assignee`, `labels`, or `areas`.
   - Keep the record current as material facts change: summarize implemented scope; name relevant files and symbols with portable repository-relative references; record acceptance evidence by check with its evidence status and class; capture bounded decisions or deviations; and maintain exact remaining work plus the resume instruction. Do not copy conversations, raw logs, private reasoning, full diffs, secrets, or machine-specific absolute paths into the record.
   - When an assigned standalone task has a parent phase with `Phase Implementation State` and you are the sole active writer for that phase, refresh only that roll-up after a durable task-record update: name the active task or next action and phase-level blockers or decisions. With parallel tasks in the phase, leave the roll-up to the manager and report the delta in your packet. Keep `Progress` to manager-accepted completed tasks; describe unaccepted implementation work as active or pending review. Do not duplicate task scope, evidence, deviations, or `Execution Record` content in the phase document.
   - Do not set the parent phase `Status`, mark its task-index checkbox, or increase its completed-task count before manager acceptance.
   - Do not write the manager-owned acceptance or review disposition. Leave `Review Outcome` as `Not Reviewed` while awaiting manager review, and do not set the task `Status` to `Completed` solely because implementation and focused verification finished.

5. Verify the assigned work.
   - Run the narrowest meaningful automated checks for the changed area.
   - Run broader checks when the assigned work affects shared contracts, build configuration, module boundaries, or integration behavior.
   - If a check is impractical, unavailable, or blocked, state that clearly and explain why.
   - Identify manual tests when they are useful, when UI/runtime behavior changed, or when automated coverage is incomplete.
   - Map every acceptance check to durable evidence in the Execution Record. Use only the allowed evidence statuses and evidence classes (including `Human Assessment`, which cannot be replaced by an automated class; record a subjective check as `Not Run` or `Blocked`, never `Not Applicable`, until a human decision is recorded), and record failed, blocked, or not-run checks explicitly rather than implying success from adjacent tests.

6. Review the implemented code against the plan.
   - Re-read the assigned tasks or high-level nodes, their acceptance checks, authoritative dependencies or hard prerequisites, and the repository instructions or architecture constraints that governed the change.
   - Inspect the final diff before responding. Check that each changed file is required for the assigned work and that unrelated user changes were not reverted or absorbed.
   - Compare the implementation to the stated requirements. Verify behavior, tests, public APIs, module boundaries, serialization/persistence contracts, dependency direction, and error handling as relevant to the assigned work.
   - Check that the implementation remains simple, contains no speculative
     extensibility, and does not add avoidable duplicated knowledge or logic.
     Consider SOLID only for actual responsibility or dependency problems that
     remain after those higher-priority checks.
   - Treat this as a code-review pass, not a rewrite pass. Fix clear defects, requirement gaps, or plan-alignment issues discovered during review; leave unrelated improvements as follow-up notes.
   - If the review finds an assigned item cannot meet the plan as written, stop and report the conflict or blocker instead of marking it complete.
   - Verify that the public contracts and implementation boundaries of the assigned work are documented in the repository's comment standard and that the documentation matches observed implementation behavior.
   - After inspecting the final diff, persist the final implementation state before responding. Record verified scope and evidence, unresolved evidence gaps, decisions or deviations, relevant files and symbols, remaining work, and the exact resume point. Use `Blocked` only for a genuine implementation blocker; otherwise leave `Status` as `In Progress` pending manager acceptance.

7. Stop after the assigned item or batch.
   - Do not start work beyond the assigned item or batch in the same turn.
   - Do not commit, stage, push, or create a pull request unless explicitly requested.
   - If you updated task checkboxes or a parent phase roll-up, include the relevant plan documents in the same uncommitted change set and summarize those changes.
   - Do not send the final response until the authoritative Execution Record reflects the current result. A conversational handoff cannot substitute for the persisted record.

## Output Markers

Output markers are on by default. Omit all emojis when `Output markers: off` applies under the shared convention (declared `off`, or undeclared with repository instructions asking for plain output). When on, use emojis sparingly in user-facing headings and result labels, never as decoration or on every bullet:

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

Report the result concisely as a summary of the already-persisted Execution Record using this handoff format:

- The tasks or high-level nodes implemented, including IDs when present.
- The authoritative Execution Record path and its persisted status.
- Parent phase roll-up updated or deferred to the manager, when applicable.
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
- Execution record path/status:
- Parent phase roll-up updated/deferred:

## Quality Rules

- Treat plan acceptance checks as requirements, not suggestions.
- Persist the assigned task's current implementation state before every final response; never leave the only authoritative result in chat.
- For a standalone task under a phase with `Phase Implementation State`, keep the parent roll-up current but concise. The task document remains the sole source for task intent, implementation detail, evidence, deviations, and `Execution Record`; manager acceptance alone closes phase progress.
- Respect field ownership: the implementer writes implementation scope, evidence, deviations, relevant files and symbols, remaining work, and resume or closure; the manager owns acceptance and review disposition.
- Keep the diff easy to review.
- Prefer explicit validation and clear failure messages where the repository style supports them.
- Do not expose internal storage, mutable globals, or implementation details through public APIs unless the assigned work requires it.
- If an assigned item or batch is too large to review safely, split it into a smaller first implementation slice and explain the split before continuing.
- Document the public contracts and implementation boundaries of the assigned work in the repository's existing comment standard, and keep that documentation consistent with observed behavior.
