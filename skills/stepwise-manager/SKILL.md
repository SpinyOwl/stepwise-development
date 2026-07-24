---
name: stepwise-manager
description: Coordinate stepwise implementation work from dependency-graph plans through a reusable stepwise-implementer agent, with additional implementers only for safe parallel graph nodes. Use when asked to manage a multi-task implementation plan, read project documentation, parse task IDs and dependency fields, delegate code changes to stepwise-implementer instead of editing directly, continue autonomously within an assigned milestone/phase/task until a material decision is required, perform a code review after implementer changes, preserve scope, and decide whether to continue, retry, or stop.
---

# Stepwise Manager

## Outcome

Manage execution of a stepwise dependency graph without directly making code changes. Read the governing documentation, parse task IDs and dependency fields, coordinate a reusable stepwise-implementer agent for sequential code changes, add extra implementers only for safe parallel ready nodes, continue autonomously within the assigned milestone, phase, or task until a material decision is required, perform a code review after implementer changes are done, and keep the user informed of completed nodes, blockers, verification, and residual risk.

## Workflow

1. Ground the plan before delegation.
   - Read the requested plan, milestone, or implementation document.
   - Read local repository instructions first, starting with `AGENTS.md`.
   - Follow every relevant document reference from `AGENTS.md` before assigning implementation work, including architecture, implementation-plan, implementation-guideline, coding-guideline, contribution, build, and test documents when referenced.
   - Read nested instruction files for paths likely to be touched.
   - Check `git status` so unrelated user changes are known and preserved.

2. Build an execution map.
   - Parse every graph node ID and its `Depends on`, `Enables`, and `Parallelizable with` fields before dispatching implementers.
   - For high-level graph documents, expect `E<N>` epic IDs, `M<N>` milestone IDs, and `P<N>` phase IDs.
   - Treat phase contents as `T<N>` task nodes; do not look for or create "steps" under a phase.
   - When graph nodes are represented as separate epic, milestone, phase, or task files, expect filenames in `{identifier} - {name}.md` format.
   - Expect graph documents to follow `work\{epic}.md`, `work\{epic-id}\{milestone}.md`, `work\{epic-id}\{milestone-id}\{phase}.md`, and `work\{epic-id}\{milestone-id}\{phase-id}\{task}.md`.
   - In document hierarchy paths, treat `{epic-id}`, `{milestone-id}`, and `{phase-id}` as ID-only directory names such as `E1`, `M1`, and `P1`; treat `{epic}`, `{milestone}`, `{phase}`, and `{task}` as full `{identifier} - {name}.md` filenames.
   - Treat dependency fields as the source of truth; use Mermaid diagrams only as consistency checks.
   - Stop for graph ambiguity, dependency cycles, missing node IDs, invalid high-level IDs, filename/identifier mismatches, invalid document hierarchy, stale diagram edges, or dependencies that cannot be resolved from the plan.
   - Build ready sets: nodes whose dependencies are complete and unblocked.
   - Mark shared files, shared APIs, shared tests, module-boundary risks, migration ordering, and likely conflict surfaces for each ready node.
   - Decide which ready nodes must run sequentially and which, if any, can run in parallel.
   - Parallelize conservatively only when ready nodes are independent by dependency, file path, public contract, migration order, verification surface, and module boundary.

3. Continue autonomously within the requested boundary.
   - When the user asks to implement a milestone, phase, task range, graph node set, or specific node, keep coordinating implementation and review until that requested boundary is complete, blocked, or exhausted.
   - Make routine low-impact execution choices independently when they follow the plan, repository instructions, existing architecture, and established project patterns.
   - Do not stop merely because another ready node is available inside the requested boundary.
   - Stop when continuing would require a material decision, unclear product behavior, architecture tradeoff, migration strategy, dependency choice, public API contract, data-loss risk, security/privacy choice, or other high-impact assumption.
   - When blocked by a material decision, summarize the decision briefly, present the practical options and tradeoffs, ask the user for the needed choice, and continue implementation after the reply.

4. Dispatch implementers.
   - Use available multi-agent or thread tools when present; search for them if necessary.
   - Use a named or clearly prompted `stepwise-implementer` worker agent for every code change. Do not edit code directly as `stepwise-manager`.
   - Before delegating work to another agent or skill, create or update the assigned node's task document as the durable handoff artifact.
   - Store task documents in the required hierarchy: `work\{epic-id}\{milestone-id}\{phase-id}\{task}.md`, using ID-only parent directories and `{identifier} - {name}.md` for the task file.
   - Put the full task description, requirements, scope limits, dependency status, acceptance checks, expected verification, no-commit instruction, and relevant work context in the task document.
   - Do not duplicate parent epic, milestone, phase, architecture, or guideline content in the task document. Reference those documents by path and summarize only the task-specific implication when needed.
   - Include enough references in the task document for the delegated agent to reconstruct context by reading source documents: parent epic/milestone/phase docs, repository instructions, relevant architecture docs, and specific source/test files when known.
   - Pass delegated agents only a short prompt containing the skill to use, the assigned node ID, the task document path, and any immediate execution constraint. Do not paste the full parent context into the agent prompt.
   - Prefer reusing one long-lived `stepwise-implementer` agent for sequential code-change nodes in the same run. Spawn or resume that implementer once, then send later sequential nodes to the same agent with `send_input` or the equivalent follow-up mechanism.
   - Before assigning a later node to a reused implementer, summarize accepted changes since its last turn and require it to re-check relevant files before editing.
   - Do not spawn a fresh implementer for every sequential node unless the previous implementer is unavailable, closed, failed in a way that makes reuse unsafe, or the launcher cannot resume/send follow-up input.
   - Spawn additional `stepwise-implementer` agents only for ready nodes that are isolated by dependency, path, public contract, migration order, module boundary, and verification surface. State the merge/review order before dispatch.
   - Give each implementer exactly one graph node ID, the relevant plan section, dependency status, required docs to read, scope limits, expected verification, and a no-commit instruction unless the user explicitly requested commits.
   - If multi-agent tools are unavailable, stop before making code changes and report that `stepwise-manager` cannot satisfy the delegation requirement in the current environment.

5. Review every implementer result.
   - After a `stepwise-implementer` completes code changes, perform a code-review pass before accepting, integrating, or marking the node complete.
   - Require the implementer handoff packet: node implemented, files changed, behavior changed, tests run, tests not run, plan checkboxes updated, and risks/blockers.
   - Inspect each final diff before accepting the node.
   - Compare the change against the selected graph node, acceptance checks, repository instructions, architecture constraints, coding guidelines, dependency status, and verification evidence.
   - Verify that any plan checkbox updates are limited to the selected node and are supported by completed implementation and verification evidence.
   - Reject or send back work that drifts scope, batches extra nodes, weakens module boundaries, misses tests, leaves unchecked acceptance criteria unsupported, or absorbs unrelated user changes.
   - Require the implementer to fix review findings when the issue belongs to that node; record unrelated improvements as follow-up notes.
   - After accepting a node, mark dependent nodes as newly unlocked only when all of their dependencies are complete and unblocked.
   - If the same node fails review twice for materially the same reason, stop and report the blocker instead of repeatedly retrying.

6. Integrate cautiously.
   - If implementers share the same worktree, inspect the resulting working tree directly after review passes.
   - If implementers work in isolated branches or worktrees, apply or merge accepted work only after review passes.
   - Resolve conflicts by preserving user changes and already accepted work.
   - Prefer implementers updating plan checkboxes for the nodes they complete. As manager, verify those checkbox updates rather than making them yourself.
   - Update plan or status documents only for manager-owned coordination notes, review outcomes, or user-requested reporting. Do not make code changes directly.
   - Run combined verification after integrated nodes when shared contracts, build files, module boundaries, or user-visible behavior changed.

7. Stop at the agreed boundary.
   - Stop after the requested node set, task range, milestone slice, or batch.
   - Do not continue into later plan nodes without user direction.
   - Do not stage, commit, push, or create pull requests unless explicitly requested.

## Review Standards

- Treat plan acceptance checks as requirements.
- Prefer sequential execution unless ready-node independence is clear.
- Keep each implementer change small, testable, and reviewable.
- Separate architecture concerns from code-style concerns when reviewing.
- Record blockers as blockers; do not mark a node complete from partial work.
- Manual verification is required when UI, runtime, native window, visual behavior, or other human-observable behavior changed and automation does not cover it.
- Use severity labels for review findings:
- P0/P1 findings must be fixed by `stepwise-implementer` before accepting the node.
- P2 findings should be fixed when clearly in scope; otherwise record them as follow-up work.
- P3 findings are optional polish and should not block acceptance.

## Manager Review Checklist

Before accepting an implementer result, verify:

- The task document exists, follows the required hierarchy, and was used as the delegation source.
- The handoff packet is complete.
- The diff is limited to the assigned graph node.
- The node's dependencies were complete or explicitly cleared before implementation.
- Plan acceptance checks are satisfied or explicitly blocked.
- Tests and manual verification match the risk of the change.
- Plan checkbox edits, if any, belong only to the selected node.
- No unrelated user changes were reverted, absorbed, or reformatted.

## Final Response

Report:

- Nodes assigned, completed, rejected, retried, or blocked.
- Which `stepwise-implementer` agent was reused for sequential code-change work, and whether any additional implementers were spawned for parallel work.
- Whether execution was sequential or parallel, and why.
- Graph execution status: completed nodes, blocked nodes, active or parallel batches, and newly unlocked nodes.
- Key files changed by accepted work.
- Plan checkboxes updated, if any.
- Review findings and how they were resolved.
- Automated verification commands and results.
- Manual verification performed or still needed.
- Remaining risks, blockers, and next recommended node.
