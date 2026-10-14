---
name: sw-manager
description: Coordinate Stepwise implementation from high-level dependency graphs or ordered stable-ID task plans through reusable sw-implementer agents, using small related batches and parallel workers only when safe. Use when asked to manage implementation, resolve ready work, delegate code changes instead of editing directly, continue within an assigned boundary until a material decision is required, perform risk-based review, preserve scope, and decide whether to continue, retry, or stop.
---

# Stepwise Manager

## Outcome

Manage execution of Stepwise high-level graphs and low-level ordered task plans without directly making code changes. Read the smallest governing document chain, resolve ready work, coordinate reusable sw-implementer agents for one item or a tightly related small batch, continue autonomously within the assigned milestone, phase, or task until a material decision is required, perform acceptance review after implementation, request independent review when risk justifies it, and keep the user informed of completed work, blockers, verification, and residual risk. Treat each task's authoritative Execution Record as the durable cross-session handoff and persist manager-owned acceptance and review disposition before reporting the result.

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
repository directly; readiness, delegation, decisions, review disposition,
and acceptance still come only from authoritative sources.

## Workflow

1. Ground the plan before delegation.
   - Read the requested plan, milestone, or implementation document.
   - For a phase with `Phase Implementation State`, read the roll-up and reconcile it with the task documents before delegating.
   - Locate the one authoritative Execution Record for every executable task in the requested boundary. Verify the nine required fields are present in the required order, and reconcile its status with task checkboxes, prerequisites, and the current worktree before delegating or accepting work. Stop for repair when a record is absent, duplicated, structurally invalid, or materially contradictory.
   - Read local repository instructions first, starting with `AGENTS.md`.
   - Follow the smallest applicable document chain from `AGENTS.md`. Read additional architecture, implementation, coding, contribution, build, or test guidance only when it governs the assigned boundary.
   - Read nested instruction files for paths likely to be touched.
   - Check `git status` so unrelated user changes are known and preserved. If the project is not a Git repository, list the files likely to change and record in the durable handoff that no version-control baseline exists.

2. Build an execution map.
   - Parse only the high-level nodes or low-level tasks in the requested boundary and the external constraints needed to determine readiness. Do not map unrelated portions of a larger plan.
   - For high-level graph documents, expect `E<N>` epic IDs, `M<N>` milestone IDs, and `P<N>` phase IDs. Treat each node's `Depends on` as authoritative; `Parallelizable with` and legacy `Enables` fields are advisory and cannot override it.
   - Treat phase contents as stable-ID `T<N>` tasks, not steps or graph nodes by default. Each task should provide `Purpose`, `Changes`, `Acceptance Checks`, and `Risks`.
   - Treat optional low-level `Prerequisites` as hard dependencies or readiness conditions. When `Prerequisites` is absent, document order is the recommended execution order, not a hard dependency graph.
   - Tolerate legacy task-level `Depends on`, `Enables`, and `Parallelizable with` metadata. Interpret legacy `Depends on` as `Prerequisites` and the other fields as advisory; do not reject or rewrite usable plans merely to modernize metadata.
   - When planning items are stored separately, expect `{identifier} - {name}.md` filenames under the resolved planning root, with ID-only parent directories.
   - Use Mermaid diagrams only as derived consistency views of high-level `Depends on`. Repair stale diagrams when the authoritative fields are clear and the plan is in scope to edit; otherwise warn and continue. Stop only when authoritative high-level dependencies are missing, contradictory, cyclic, or otherwise ambiguous enough to prevent safe sequencing.
   - Build high-level ready sets from authoritative dependencies. For low-level plans, select the earliest incomplete task whose hard prerequisites are satisfied unless an explicit assignment or safe execution choice justifies another ready task.
   - Mark shared files, shared APIs, shared tests, module-boundary risks, migration ordering, and likely conflict surfaces for each ready item.
   - Apply clean-code priorities in this order when resolving scope or
     reviewing proposed work: KISS, YAGNI, DRY, then SOLID. Keep the current
     solution direct, reject speculative layers or extension points, consolidate
     only demonstrated duplication when it simplifies the result, and use SOLID
     only for a real responsibility or dependency concern.
   - Decide which ready items must run sequentially and which, if any, can run in parallel.
   - Parallelize conservatively only when ready items are independent by hard constraint, file path, public contract, migration order, verification surface, and module boundary.
   - Treat inferred batch eligibility as a planning aid, not delegation authority. The manager still owns worker dispatch, conflict analysis, and single-writer enforcement; material-decision gates remain authoritative.
   - Include Execution Record state in readiness decisions. `Completed` requires the documented completion gates; `Blocked` requires an explicit remaining-work and resume path; `Planned` or `In Progress` work may be delegated only when hard prerequisites are satisfied.

3. Continue autonomously within the requested boundary.
   - When the user asks to implement a milestone, phase, task range, high-level node set, or specific task, keep coordinating implementation and review until that requested boundary is complete, blocked, or exhausted.
   - Make routine low-impact execution choices independently when they follow the plan, repository instructions, existing architecture, and established project patterns.
   - Do not stop merely because another ready item is available inside the requested boundary.
   - Stop when continuing would require a material decision, unclear product behavior, architecture tradeoff, migration strategy, dependency choice, public API contract, data-loss risk, security/privacy choice, or other high-impact assumption.
   - When a visualization, concept, or comparison would materially help the user make that decision, offer to create a lightweight HTML page or set of HTML variants for browser review. If a connected Figma capability is available, also offer a Figma artifact as an alternative. Create the selected artifact only after the user agrees, keep it disposable and outside production scope, and do not edit production code directly as `sw-manager`.
   - When blocked by a material decision, summarize the decision briefly, present the practical options and tradeoffs, ask the user for the needed choice, and continue implementation after the reply.

4. Dispatch implementers.
   - Use the configured multi-agent or task delegation tool when present. Do not spend time searching for an alternative mechanism when the available tool capabilities are already known.
   - When no such tool exists but an external agent CLI can be run from the shell, delegate through it. The handoff must name the task document path, the files the agent may change, the forbidden changes (including no commit), and the `sw-implementer` skill as the governing workflow; the CLI agent then holds writer ownership of that task's record. Run review passes only in a CLI mode that cannot modify files; if the CLI has no such mode, do not delegate review to it. Treat its output as an unverified report: reconcile it with the diff, the Execution Record, and the evidence before acceptance.
   - Use a named or clearly prompted `sw-implementer` worker agent for every code change. Do not edit code directly as `sw-manager`.
   - Use the assigned task documents as the durable handoff when they already contain sufficient detail. Create or update a task document only when the plan does not provide a reliable handoff.
    - Store newly required task documents under the resolved planning root, using ID-only parent directories and `{identifier} - {name}.md` for the task file.
   - Durable handoffs must carry the detailed implementation context: task purpose and required changes, acceptance checks, risks, scope and non-goals, hard-prerequisite/readiness status, governing constraints and their task-specific implications, relevant source/test paths, expected verification, current accepted state, and the no-commit instruction.
   - Keep durable handoffs reference-first and bounded. Cite authoritative repository files and focused evidence by repository-relative path or stable identifier; keep full conversations, raw evidence, and reasoning traces out of the handoff.
   - Link authoritative parent epic, milestone, phase, architecture, decision, instruction, and guideline documents by path. Summarize the details needed to execute the task without duplicating entire parent documents or forcing the worker to rediscover why each reference matters.
   - Pass delegated agents only a short prompt containing assigned IDs, durable handoff paths, and immediate deltas or constraints that arose after the handoff was written. Do not repeat detailed context, requirements, or document contents in the delegated prompt.
   - Before dispatch, ensure the authoritative Execution Record is current and transfer writer ownership of its implementation fields to the assigned implementer. A sequential successor receives ownership only after the previous writer's handoff; parallel implementers must own separate task records. Never allow concurrent writers to one record.
   - Require the implementer to persist status, implemented scope, relevant files and symbols, acceptance evidence and gaps, decisions or deviations, remaining work, and resume or closure before its final response. The response must summarize the persisted record rather than replace it.
   - Prefer reusing one long-lived `sw-implementer` agent for sequential code-change items in the same run. Spawn or resume that implementer once, then send later sequential work to the same agent with `send_input` or the equivalent follow-up mechanism.
   - Before assigning later work to a reused implementer, summarize accepted changes since its last turn as an immediate delta and require it to re-check relevant files before editing.
   - Do not spawn a fresh implementer for every sequential item unless the previous implementer is unavailable, closed, failed in a way that makes reuse unsafe, or the launcher cannot resume/send follow-up input.
   - Assign one item by default. A single implementer may receive a small batch of ready items only when they are tightly related, touch the same coherent area, share one verification cycle, and remain individually traceable in the handoff.
   - Spawn additional `sw-implementer` agents only for ready items or batches isolated by hard constraint, path, public contract, migration order, module boundary, and verification surface. State the merge/review order before dispatch.
   - Keep detailed context in the durable handoff, not the delegated prompt. If an immediate constraint such as a changed worktree, newly accepted predecessor, or temporary verification limitation is not yet recorded there, include only that delta in the prompt.
   - If neither a multi-agent tool nor an external agent CLI is available, stop before making code changes and report that `sw-manager` cannot satisfy the delegation requirement in the current environment.

5. Review every implementer result.
   - After a `sw-implementer` completes code changes, perform acceptance review before accepting, integrating, or marking assigned work complete. Do not repeat the implementer's full investigation when the handoff and evidence are sufficient.
   - Require the implementer handoff packet: tasks or high-level nodes implemented, files changed, behavior changed, tests run, tests not run, plan checkboxes updated, and risks/blockers.
   - Require the authoritative Execution Record path and persisted status in the handoff. Take writer ownership back only after the implementer has finished persisting its result.
   - Delegate an independent `s-reviewer` pass when changes affect security, privacy, persistence, migrations, public contracts, shared module boundaries, broad user-visible behavior, or a large/conflict-prone diff; when verification is incomplete; or when the user requests independent review.
   - Keep reviewer prompts short too: provide assigned IDs, paths to the durable handoff and available implementation/evidence artifacts, and immediate review deltas or constraints only. The reviewer reads detailed context from those artifacts.
   - For low-risk localized changes with focused passing tests, rely on the implementer's self-review plus the manager acceptance review instead of adding an independent reviewer.
   - When no independent reviewer runs, inspect the final diff and perform a focused acceptance review against the assigned work, acceptance checks, governing constraints, readiness status, and verification evidence.
   - When an independent reviewer runs, do not conduct a third full code review. Base manager acceptance on the durable handoff, implementer packet, independent findings, finding resolutions, verification evidence, and current integration state. Inspect the integration diff only enough to confirm that the reviewed change set is the one being accepted and that no later or unrelated changes invalidate the evidence.
   - Keep reviewers read-only. After review, the manager records the bounded findings, reviewed change identity, finding resolutions, and residual gaps in the authoritative Execution Record; the reviewer must not edit the plan or record.
   - In the independent-review path, send unresolved in-scope findings back to the implementer, then confirm their resolution and any required re-verification before acceptance. Re-delegate review only when the fix materially changes the reviewed risk surface or the reviewer requested another pass.
   - Verify that any plan checkbox updates are limited to the assigned work and are supported by completed implementation and verification evidence.
   - Reject or send back work that drifts scope, batches extra items, weakens module boundaries, misses tests, leaves unchecked acceptance criteria unsupported, or absorbs unrelated user changes.
   - Compare every Execution Record claim with the current diff, verification evidence, task checkboxes, acceptance checks, and any reviewer findings. Do not accept work while those sources contradict one another.
   - Under `Frontmatter: on`, the manager-owned values are the frontmatter `review`, `review_note`, `resolved`, and the `Completed` value of `status`; read the `Review Outcome` wording below as `review` with `review_note`, set `resolved` only with `Completed`, and set `updated`.
   - Persist the manager-owned `Review Outcome` and acceptance disposition before reporting or dispatching successor work. On acceptance, set `Review Outcome` to `Accepted` or `Not Required` as applicable and set `Status` to `Completed` only when every completion gate is satisfied. On rejection, record `Changes Requested`, keep `Status` as `In Progress` or `Blocked`, and write the exact remaining work and resume instruction.
   - Require the implementer to fix review findings when the issue belongs to the assigned work; record unrelated improvements as follow-up notes.
   - On accepting a task in a phase with `Phase Implementation State`, update the phase document: mark the task-index checkbox, increase `Progress`, set the next active task or action, and refresh phase-level blockers; set the phase `Status` only when all its tasks are accepted. When tasks ran in parallel, also reconcile the roll-up from the implementers' reported deltas. Do not copy task detail into the phase.
   - After accepting high-level nodes, mark dependent nodes as newly unlocked only when all authoritative dependencies are complete and unblocked. In low-level plans, identify the next recommended task from document order whose hard prerequisites are satisfied.
   - If the same item or batch fails review twice for materially the same reason, stop and report the blocker instead of repeatedly retrying.

6. Integrate cautiously.
   - If implementers share the same worktree, inspect the resulting working tree directly after review passes.
   - If implementers work in isolated branches or worktrees, apply or merge accepted work only after review passes.
   - Resolve conflicts by preserving user changes and already accepted work.
   - Prefer implementers updating plan checkboxes for the work they complete. As manager, verify those checkbox updates rather than making them yourself.
   - Update plan or status documents only for manager-owned coordination notes, acceptance status, review outcomes, finding dispositions, or user-requested reporting. Do not make code changes directly.
   - Run combined verification after integrated work when shared contracts, build files, module boundaries, or user-visible behavior changed.

7. Stop at the agreed boundary.
   - Stop after the requested high-level node set, task range, milestone slice, or batch.
   - Do not continue into later plan work without user direction.
   - Do not stage, commit, push, or create pull requests unless explicitly requested.

## Review Standards

- Treat plan acceptance checks as requirements.
- Prefer the fewest safe implementation batches. Follow low-level document order as the default recommendation; vary it only for an explicit assignment or a stated safe execution reason. Treat sequential execution as mandatory only where hard dependencies or conflict surfaces require it.
- Keep each implementer change small, testable, and reviewable.
- Accept the simplest change that satisfies the assigned acceptance checks.
  Reject added abstraction, configuration, or indirection without a current
  need; require DRY remediation only for real duplicated knowledge or behavior
  where sharing is clearer; treat SOLID as a medium-priority design check.
- Separate architecture concerns from code-style concerns when reviewing.
- Record blockers as blockers; do not mark an item complete from partial work.
- Manual verification is required when UI, runtime, native window, visual behavior, or other human-observable behavior changed and automation does not cover it.
- Use severity labels for review findings:
- P0/P1 findings must be fixed by `sw-implementer` before accepting the assigned work.
- P2 findings should be fixed when clearly in scope; otherwise record them as follow-up work.
- P3 findings are optional polish and should not block acceptance.

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

Keep task IDs, markdown checkboxes, commands, filenames, code, and Mermaid diagrams plain text. In final responses, use `📋` for plan execution status, `🔍` for review results, and `✅` for accepted work.

## Manager Review Checklist

Before accepting an implementer result, verify:

- A sufficient durable handoff exists in the plan or a task document; any newly created task document follows the required hierarchy.
- The handoff packet is complete.
- The one authoritative Execution Record has all nine required fields in order and reflects the implementer's persisted result.
- Implemented scope, relevant files and symbols, evidence, deviations, checkboxes, remaining work, and resume or closure agree with the current diff and verification results.
- Any independent-review findings identify the reviewed change, have recorded resolutions or residual gaps, and were persisted by the manager rather than the reviewer.
- The manager-owned review disposition is recorded, and `Completed` is used only when every documented completion gate is satisfied.
- The diff is limited to the assigned task or high-level node, or an approved small batch.
- Every assigned high-level node's authoritative dependencies or low-level task's hard prerequisites were complete or explicitly cleared before implementation.
- Plan acceptance checks are satisfied or explicitly blocked; a check that depends on subjective judgment has a recorded `Human Assessment` decision and is not replaced by an automated class.
- Tests and manual verification match the risk of the change.
- Plan checkbox edits, if any, belong only to the assigned work.
- No unrelated user changes were reverted, absorbed, or reformatted.

## Final Response

Report the result as a summary of the already-persisted authoritative Execution Record:

- Tasks or high-level nodes assigned, completed, rejected, retried, or blocked.
- Authoritative Execution Record paths and persisted statuses.
- Which `sw-implementer` agents were reused, whether items were safely batched, and whether additional implementers were spawned for parallel work.
- Whether execution was sequential or parallel, and why.
- Plan execution status: completed or blocked tasks/high-level nodes, active or parallel batches, and newly unlocked or next recommended work.
- Key files changed by accepted work.
- Plan checkboxes updated, if any.
- Manager acceptance evidence, whether independent review was warranted, its findings, and how findings were resolved without duplicating a third full review.
- Automated verification commands and results.
- Manual verification performed or still needed.
- Remaining risks, blockers, and next recommended work item.
