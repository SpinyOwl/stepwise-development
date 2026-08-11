---
name: stepwise-planner
description: Create markdown plans for software projects, using dependency graphs for high-level epics, milestones, and phases and stable-ID tasks for low-level implementation work. Use when turning a roadmap, milestone, feature, refactor, architecture change, bug-fix initiative, or vague goal into structured outcomes or executable tasks with clear acceptance checks, risks, and verification guidance.
---

# Stepwise Planner

## Outcome

Produce a markdown plan that matches the requested planning level and lets the next engineer or agent act without guessing the intended scope.

## Planning Root Compatibility

Resolve plan paths using the single planning-root rule in the shared
`folder-structure` convention. The hierarchy and filename examples below
apply relative to that resolved planning root.

- High-level plans define epics, milestones, or phases as dependency graph nodes with architectural propositions, decisions, questions, risks, and validation points.
- Phase documents consist of stable-ID `T<N>` implementation tasks, not steps.
- Low-level plans divide a phase, feature, fix, or refactor into small implementation tasks that can be reviewed and verified independently; they are not dependency graphs by default.
- High-level `Depends on` fields are authoritative. High-level Mermaid diagrams are derived views, and parallelism metadata is optional.

## Choose the Planning Level

Use **high-level planning** when the user asks for a roadmap, milestone list, phase plan, architecture direction, or the parts of a single milestone. High-level plans should clarify system direction and tradeoffs before implementation details.

Use **low-level planning** when the user asks to divide a phase, feature, bug fix, refactor, or already-chosen direction into implementation tasks. Low-level plans may refer to existing or planned code entities, files, APIs, tests, modules, data shapes, and verification commands.

If the requested level is unclear, infer from scale:

- Broad goal, multi-module initiative, or architecture uncertainty: high-level.
- Concrete feature/fix/refactor with known target behavior: low-level.
- Ask only when the ambiguity would materially change the output.

## Workflow

1. Inspect the current project context before planning.
   - Read the smallest applicable instruction and documentation chain, then inspect only the architecture, plans, module boundaries, code, and tests that materially affect the requested plan.
   - If local instructions conflict with the requested direction, raise the conflict before writing the plan.
   - Prefer the repository's current tools, testing style, naming, and planning conventions.

2. Define the planning target.
   - State the goal in one paragraph.
   - State explicit non-goals when they prevent speculative scope.
   - List assumptions and unknowns. Ask only for unknowns that would materially change the plan.

3. Build the plan at the chosen level.
   - Assign every epic, milestone, phase, or task a stable ID.
   - Use `E<N>` for epics, `M<N>` for milestones, `P<N>` for phases, and `T<N>` for low-level implementation tasks.
   - When writing one epic, milestone, phase, or task per file, name the file `{identifier} - {name}.md`, for example `M1 - Authentication foundation.md`.
    - Store documents under the resolved planning root using full `{identifier} - {name}.md` filenames and ID-only parent directories.
   - In that hierarchy, `{epic}`, `{milestone}`, `{phase}`, and `{task}` are full document filenames using `{identifier} - {name}.md`; `{epic-id}`, `{milestone-id}`, and `{phase-id}` are ID-only directory names such as `E1`, `M1`, and `P1`.
   - For high-level planning, organize milestones or phases around capabilities, architecture decisions, ownership boundaries, and validation points.
   - When planning inside a phase, represent the phase's contents as `T<N>` tasks.
   - For low-level planning, slice the work into small reviewable implementation tasks with stable `T<N>` IDs.
   - Keep a single straightforward outcome as one concise task; do not invent extra tasks or a dependency graph merely to make the plan look substantial.
   - For every high-level `E<N>`, `M<N>`, or `P<N>` node, include `Depends on`; use `None` when no dependency exists. Add `Parallelizable with` only when it communicates useful, safely established parallelism. Do not require `Enables`; when legacy or repository-local plans include it, treat it as a derived reverse view that cannot override `Depends on`.
   - For every low-level `T<N>` task, include `Purpose`, `Changes`, `Acceptance Checks`, and `Risks`. Add `Prerequisites` only for a hard dependency or readiness condition.
   - Tolerate legacy `Depends on`, `Enables`, and `Parallelizable with` fields in existing low-level plans. Interpret legacy task-level `Depends on` as prerequisite information and the other fields as advisory; do not rewrite them solely for format conformity.
   - Avoid speculative foundations unless a later listed milestone, phase, or task directly consumes them.

4. Model dependencies and risk.
   - Put contract, data model, boundary, and architecture decisions before dependent implementation.
   - Put high-uncertainty spikes early, but time-box them and define their output.
   - Put broad cleanup after behavior is covered by tests or reviewable acceptance criteria.
   - Do not encode high-level ordering by document position alone. Express it through authoritative `Depends on` fields.
   - In low-level plans, arrange tasks in the recommended execution order. When `Prerequisites` is absent, document order supplies that recommendation without creating a hard dependency.
   - Reserve `Prerequisites` for hard dependencies or readiness conditions that must be satisfied before a task can start; do not use it for every preferred sequencing choice.
   - Mark optional high-level parallelism only when nodes do not share files, public contracts, ordered migrations, or overlapping test surfaces likely to conflict.

5. Emit the requested markdown document or document set.
   - Use concise headings and checklists.
   - Include enough detail for the chosen level, but do not mix high-level roadmap planning with task-level implementation unless the user asks for both.
   - End high-level documents that plan relationships among `E<N>`, `M<N>`, or `P<N>` nodes with a Mermaid `flowchart TD` dependency visualization derived from authoritative `Depends on` fields.
   - Do not add a Mermaid dependency graph to a low-level plan merely because it contains multiple tasks. Include one only when the user or a stronger repository convention explicitly requires it.
   - Include a `Document Context` section in every generated planning document.
   - Do not include motivational prose, project-management filler, or implementation details that belong in code comments.

## High-Level Planning Template

Use this structure for roadmaps, milestones, phase planning, and architecture-direction plans unless the repository already has a stronger planning format:

````markdown
# <Plan Title>

File name: `<identifier> - <name>.md` for each epic, milestone, phase, or task document, such as `E1 - <Epic Name>.md`, `M1 - <Milestone Name>.md`, `P1 - <Phase Name>.md`, or `T1 - <Task Name>.md`. Use only the parent identifier, such as `E1`, `M1`, or `P1`, for nested directories.

Document hierarchy (under the resolved planning root):
- `<planning-root>\E1 - Epic name.md`
- `<planning-root>\E1\M1 - Milestone name.md`
- `<planning-root>\E1\M1\P1 - Phase name.md`
- `<planning-root>\E1\M1\P1\T1 - Task name.md`

## Document Context
- Parent: <Link to the parent document, or None>
- Children: <Links to child documents when applicable, or None>
- Related: <Links to required architecture or decision documents, or None>
- Next: <Next document to read, or None>

## Goal
<One short paragraph describing the desired end state.>

## Context
- <Relevant architecture/module constraint>
- <Relevant product, runtime, persistence, compatibility, or ownership constraint>

## Epics / Milestones / Phases

### M1: <Capability or Decision Outcome>
**Purpose:** <Why this milestone or phase exists.>

**Depends on:** <None, or IDs this node requires.>
<!-- Optional when useful: **Parallelizable with:** <IDs with safely established parallelism.> -->

**Architectural Proposition:** <Preferred direction, boundary, or system shape.>

**Key Work:**
- <Capability, contract, spike, or decision to complete at this level>
- <Next phase-level outcome>

**Open Questions:**
- <Question that materially affects architecture, sequencing, or scope>

**Validation:**
- <Evidence that this milestone or phase is complete enough to plan lower-level work>

### M2: <Capability or Decision Outcome>
...

## Cross-Cutting Risks
- <Risk, tradeoff, or dependency with mitigation>

## Verification / Review Strategy
- <How to review decisions, architecture fit, and readiness for low-level planning>

## Dependency Graph
```mermaid
flowchart TD
  M1["M1: <Capability or Decision Outcome>"]
  M2["M2: <Capability or Decision Outcome>"]
  M1 --> M2
```
````

## Low-Level Planning Template

Use this structure for implementation-task plans unless the repository already has a stronger planning format:

````markdown
# <Plan Title>

## Document Context
- Parent: <Link to the parent phase or plan, or None>
- Children: <Links to separately stored task documents when applicable, or None>
- Related: <Links to required architecture or decision documents, or None>
- Next: <Next document to read, or None>

## Goal
<One short paragraph describing the desired end state.>

## Non-Goals
- <Out-of-scope item>

## Context
- <Relevant architecture/module constraint>
- <Relevant existing or planned file, API, behavior, data shape, or test>

## Assumptions and Open Questions
- Assumption: <Assumption that affects sequencing>
- Question: <Only include if it materially changes the plan>

## Phase Tasks

Tasks appear in recommended execution order. A task's optional `Prerequisites`
lists only hard dependencies or readiness conditions that must be satisfied
before it can start.

### T1: <Small Reviewable Outcome>
**Purpose:** <Why this task exists.>

<!-- Optional when needed: **Prerequisites:** <Hard dependencies or readiness conditions.> -->

**Changes:**
- [ ] <Specific implementation task touching files, APIs, tests, docs, or assets>
- [ ] <Next concrete implementation task if needed>

**Acceptance Checks:**
- [ ] <Build/test/manual check with expected result>
- [ ] <Review criterion that proves the task is complete>

**Risks:** <Risk and mitigation, or "None identified.">

### T2: <Small Reviewable Outcome>
...

## Verification Strategy
- <Command or manual verification that should run after narrow tasks>
- <Full build/test command for final validation>

## Review Boundaries
- <Suggested commit/PR grouping>

## Deferred Work
- <Useful but intentionally postponed work>
````

## High-Level Quality Rules

- Prefer architectural propositions, decision points, tradeoffs, and questions over premature implementation tasks.
- Do not name concrete classes, files, or methods unless they already exist or are clearly planned architecture entities.
- Surface module boundaries, ownership, data flow, persistence/runtime implications, compatibility constraints, and unresolved tradeoffs.
- Keep phase or milestone completion observable through decisions, artifacts, spikes, acceptance criteria, or readiness for low-level planning.
- Give every epic, milestone, and phase a stable ID and an authoritative `Depends on` field.
- Use only `E<N>` for epics, `M<N>` for milestones, and `P<N>` for phases.
- Name every epic, milestone, phase, and task document `{identifier} - {name}.md`.
- Store graph documents under the resolved planning root, normally `docs/plans/`; use an existing `docs/work/` or root-level `work/` only for compatibility.
- Treat `Parallelizable with` as optional high-level metadata and include it only when useful and safe; do not require `Enables`.
- End high-level documents that plan relationships among epic, milestone, or phase nodes with a Mermaid dependency graph derived from `Depends on`.
- Avoid over-specifying implementation details that belong in a later low-level plan.

## Low-Level Task Quality Rules

- Use verbs in task titles: "Add", "Extract", "Validate", "Wire", "Document".
- Use stable task IDs in task titles, such as `T1: Add ...`.
- Keep each task independently understandable without reading every later task.
- Make "done" observable: a passing test, a new API contract, a generated artifact, or a documented decision.
- Format every concrete implementation task as an unchecked markdown task item (`- [ ] ...`) so implementers can mark completed work in the plan.
- Use checkboxes for task-level implementation tasks and acceptance checks; avoid checkboxes for explanatory bullets that are not actionable.
- Include `Purpose`, `Changes`, `Acceptance Checks`, and `Risks` for every task.
- Add `Prerequisites` only for a hard dependency or readiness condition; do not add an empty field by default.
- Treat document order as the recommended execution order when `Prerequisites` is absent. Recommended order does not itself create a hard dependency.
- Reserve `Prerequisites` for hard dependencies or readiness conditions, not ordinary sequencing preferences.
- Do not manufacture task-level dependency metadata or Mermaid graphs. Multiple `T<N>` tasks alone do not imply a graph.
- Accept legacy task-level `Depends on`, `Enables`, and `Parallelizable with` metadata when reviewing or extending existing plans. Treat legacy `Depends on` as prerequisites and the other fields as advisory.
- Include expected code or artifact changes, tests, build commands, manual checks, or inspection criteria.
- Include rollback or stop criteria when a task is risky.
- Avoid tasks that combine unrelated concerns such as API design, storage migration, UI polish, and cleanup.
- Do not create tasks like "clean up code" unless the cleanup target and verification are explicit.
- Do not use vague acceptance checks such as "works correctly"; name the observable behavior.
- Keep estimates out unless the user explicitly asks for them.

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

Keep task IDs, markdown checkboxes, commands, filenames, code, and Mermaid diagrams plain text. In final responses, use `📋` for plans, `⚠️` for risks, and `🧪` for verification guidance.

## Critical Review Pass

Before finalizing the document, check for:

- Planning level mismatch: high-level plans with premature implementation detail, or low-level plans too vague to execute.
- Hidden big-bang phases or tasks that should be split.
- Milestones, phases, or tasks that cannot be reviewed because they lack concrete outputs.
- Architecture violations or dependency direction mistakes.
- Missing questions around unresolved architecture, ownership, persistence, compatibility, or runtime behavior.
- Missing tests around changed contracts, serialization, persistence, or user-visible behavior in low-level plans.
- Speculative abstractions not required by the stated goal.
- Ordering errors where implementation precedes required contract, data, or architecture decisions.
- Missing, contradictory, cyclic, or ambiguous high-level `Depends on` fields that prevent safe sequencing.
- Stale Mermaid edges or nodes: repair them when authoritative `Depends on` fields are clear, or report a warning when repair is outside the current scope. Do not reject a plan for diagram staleness alone.
- Epic, milestone, or phase IDs that do not follow `E<N>`, `M<N>`, or `P<N>`.
- Epic, milestone, phase, or task document filenames that do not follow `{identifier} - {name}.md`.
- Epic, milestone, phase, or task documents outside the resolved planning-root hierarchy.
- Hidden high-level ordering assumptions that appear only through document order.
- Low-level task order that is not a sensible recommended execution sequence, or a hard dependency/readiness condition that should be explicit under `Prerequisites`. Do not require `Prerequisites` for ordinary sequencing preferences.
- Unsafe parallelism labels for nodes that share files, public contracts, migrations, or overlapping test surfaces.
