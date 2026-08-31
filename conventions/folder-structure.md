# Stepwise Documentation Structure

This document defines the shared documentation layout used by Stepwise skills.
Skills should refer to these conventions instead of redefining folder and
navigation rules.

## Documentation Root

Project documentation belongs under `docs/`.

The documentation entry point is `docs/README.md`. Agents should read that
index before planning, designing, implementing, or reviewing work. They should
follow its links to the smallest relevant document chain instead of scanning
all files under `docs/`.

## Baseline and Optional Sections

A Stepwise repository baseline requires only this documentation structure:

```text
AGENTS.md
docs/
  README.md
  folder-structure.md
<planning-root>/
  README.md
```

The planning root may be inside `docs/` or may be the legacy root-level
`work/` directory selected by the resolution rule below.

Create these optional sections only when they are needed:

```text
docs/
  README.md
  folder-structure.md
  architecture/
  decisions/
  guides/
  references/
  design/
  drafts/
```

- `architecture/` contains system boundaries, module structure, and technical
  architecture documentation.
- `decisions/` contains accepted architectural or product decisions.
- `guides/` contains user, contributor, and operational how-to documentation.
- `references/` contains stable API, domain, and technical reference material.
- `design/` contains active design proposals that are more developed than
  drafts but not yet accepted decisions.
- `drafts/` contains incomplete or exploratory documents.
- The resolved planning root contains epics, milestones, phases, and
  implementation tasks.

Optional sections should not be created until the user requests them or they
contain a document. Each created section should have a `README.md` index.

## Planning Root Resolution

Inspect `docs/plans/`, `docs/work/`, and root-level `work/` before creating a
planning directory, then apply this single rule:

1. If more than one candidate exists, stop and ask the user which existing
   directory is authoritative.
2. If exactly one candidate exists, use it.
3. If no candidate exists, create and use `docs/plans/`.

Never create `docs/plans/` when a legacy `docs/work/` or root-level `work/`
directory is the resolved planning root. Never create a new `docs/work/` or
root-level `work/` directory. Create or preserve `README.md` only in the
resolved planning root; do not add an index to another candidate.

## Planning Hierarchy

The selected planning root contains this hierarchy:

```text
<planning-root>/E1 - Epic name.md
<planning-root>/E1/M1 - Milestone name.md
<planning-root>/E1/M1/P1 - Phase name.md
<planning-root>/E1/M1/P1/T1 - Task name.md
```

Parent directories use ID-only names. Documents use the full identifier and
name.

- Epics use `E<N>`.
- Milestones use `M<N>`.
- Phases use `P<N>`.
- Implementation tasks use `T<N>`.

Phase contents are stable-ID implementation tasks, not unnamed steps. Tasks do
not need to form a dependency graph merely because a phase contains more than
one task.

## Document Navigation

Every index should identify:

- What belongs in its section.
- What does not belong there.
- The documents currently available.
- The recommended reading path.
- Which documents are authoritative.

Every planning document should include a `Document Context` section at the top
with links to its parent, children when applicable, related architecture or
decision documents, and the next document to read, plus mandatory `Status` and
`Dependencies` entries.

Agents should normally traverse this chain:

```text
AGENTS.md
  -> docs/README.md
    -> section README.md
      -> parent epic
        -> milestone
          -> phase
            -> assigned task
              -> explicitly linked supporting documents
```

Read only the parent documents and explicitly linked supporting documents
needed for the current task. Do not recursively read every document in the
project.

## Dependency and Task Metadata

For high-level graph nodes (`E<N>`, `M<N>`, and `P<N>`), `Depends on` is
required and authoritative. Use `None` when the node has no dependency.
`Parallelizable with` is optional and should appear only when recording safe
parallel execution adds useful information. `Enables` is optional derived
metadata and must not override `Depends on`.

Mermaid dependency diagrams are derived views of high-level `Depends on`
fields. A stale diagram should be repaired when the authoritative fields are
clear, or reported as a warning when it cannot be repaired in the current
scope. A stale diagram alone does not make the plan invalid; stop only when
the authoritative dependencies are missing, contradictory, cyclic, or
otherwise ambiguous enough to prevent safe sequencing.

Low-level `T<N>` tasks are stable-ID implementation work units, not graph nodes
by default. Each task must contain `Purpose`, `Changes`, `Acceptance Checks`,
and `Risks`. Add `Prerequisites` only when the task has a hard dependency or
readiness condition. Do not create low-level dependency fields or a Mermaid
graph merely because multiple tasks exist.

Within a low-level plan, document order is the recommended execution order
when a task has no `Prerequisites`. Use `Prerequisites` only for hard
dependencies or readiness conditions that must be satisfied before the task
can start, not to encode every preferred sequencing choice. Document order
recommends a sequence but does not create dependency-graph edges.

Existing plans may retain legacy `Depends on`, `Enables`, or
`Parallelizable with` fields on `T<N>` tasks. Treat a task's legacy
`Depends on` as prerequisite information and the other fields as advisory; do
not reject or rewrite an otherwise usable plan solely to remove legacy
metadata.

## Execution Record Contract

### Applicability and location

Every stable-ID task has one authoritative location. A merely planned inline
task may remain under its parent plan without an `Execution Record`. Before
work begins, add the record beneath that inline task or extract the task to one
standalone document when it needs independent writer ownership or an isolated
cross-session handoff.

When extracting an inline task:

- create `<parent-id>/T<N> - <name>.md` beside the parent document's ID-only
  directory, preserving the normal hierarchy (for example,
  `E1/M1/P1/T1 - Task name.md` for a phase task);
- move, rather than copy, `Purpose`, optional `Prerequisites`, `Changes`,
  `Acceptance Checks`, and `Risks` into the task document;
- replace the former inline body with one link to the authoritative task
  document and add that link to the parent's `Children` entry; and
- keep the `Execution Record` only at the authoritative task location.

Do not extract every planned inline task preemptively. Parallel tasks that need
different writers must use different standalone task documents; sequential
roles may update one record only through explicit handoff of writer ownership.

### Authority, ownership, and lifecycle

Task intent and execution result are distinct:

- `Purpose`, `Prerequisites`, `Changes`, `Acceptance Checks`, and `Risks` own
  the approved intent. Recording execution does not silently rewrite them.
- `Execution Record` owns the bounded current implementation and handoff
  summary. It does not override accepted decisions, current code, Git, or
  referenced verification evidence.
- The planner creates the record in `Planned` state when producing a standalone
  executable task or an immediate inline handoff. The assigned implementer
  owns implementation scope, evidence, deviations, remaining work, and the
  resume statement while working. The manager owns acceptance and review
  disposition. A reviewer remains read-only.

Update the current fields in place. The record is not an append-only run log,
session journal, conversation summary, or substitute for Git history. When
the record conflicts with code, Git, an accepted decision, or current
evidence, repair the record before using it to claim completion or delegate
more work.

### Required fields

Use the heading level appropriate to the task location, with the exact title
`Execution Record`, and include this field set in this order:

```markdown
## Execution Record

- Status: <Planned | In Progress | Blocked | Completed>
- Last Updated: <YYYY-MM-DD>
- Implemented Scope: <None, or bounded completed outcomes>
- Relevant Files and Symbols: <None, or repository-relative links and symbol names>
- Acceptance Evidence:
  - <Acceptance check>: <Verified | Failed | Blocked | Not Run | Not Applicable> — <Automated | Simulated Host | Native Host | Browser | Manual | Documentation> — <bounded evidence reference or reason>
- Decisions and Deviations: <None, or bounded decision/deviation references>
- Review Outcome: <Not Reviewed | Changes Requested | Accepted | Not Required> — <bounded disposition or reason>
- Remaining Work: <Specific remaining scope, or None>
- Resume or Closure: <Exact next action, or closure statement>
```

`Last Updated` is a portable calendar date, not a machine-local timestamp. In a
standalone task document, `Execution Record` status must match `Document
Context` status. Use `None` explicitly rather than omitting an empty required
field.

### Evidence and completion rules

- Evidence status is limited to `Verified`, `Failed`, `Blocked`, `Not Run`, or
  `Not Applicable`. `Not Applicable` requires a bounded reason.
- Evidence class is limited to `Automated`, `Simulated Host`, `Native Host`,
  `Browser`, `Manual`, or `Documentation`. One class must not be promoted to
  another.
- Each acceptance check must have one evidence entry. Reference the command,
  artifact, revision, or concise observed result; do not paste its raw body.
- Reconcile every actionable `Changes` and `Acceptance Checks` checkbox with
  the current record before using `Completed`:
  - mark a `Changes` item checked only when that work is completed; otherwise
    leave it unchecked and quote or identify the exact item under `Decisions
    and Deviations` with an accepted `Not Applicable` or deviation reason and
    its approval or decision reference;
  - mark an `Acceptance Checks` item checked only when its evidence is
    `Verified`; otherwise leave it unchecked and identify the exact item under
    `Acceptance Evidence` as `Not Applicable` with an accepted reason; and
  - never delete or rewrite an actionable item merely to hide incomplete,
    inapplicable, or deviated intent.
- `Completed` requires every acceptance check to be `Verified`, or explicitly
  `Not Applicable` with an accepted reason; no `Failed`, `Blocked`, or `Not
  Run` entry; no unresolved blocking review finding; `Review Outcome` set to
  `Accepted` or `Not Required` with an accepted reason; `Remaining Work: None`;
  an exact closure statement; and no unchecked actionable checkbox without the
  explicit accepted disposition above.
- When evidence is unmet, unavailable, interrupted, or unverified, keep the
  status `In Progress` or `Blocked`, preserve the gap, and name the next safe
  action. Never weaken or silently remove the acceptance check.

### Content boundaries

Keep the record reference-first and bounded. It must not contain full
conversations, private reasoning, raw logs, full diffs, credentials, secret
values, environment dumps, vendor session data, or machine-specific absolute
paths. Repository-relative file links, stable symbol names, bounded command
names, evidence identifiers, and commit or revision identifiers are allowed.

## Document Lifecycle

- Move exploratory material from `drafts/` to `design/` when it becomes a
  coherent proposal.
- Move accepted proposals from `design/` to `decisions/` when they become
  authoritative decisions.
- Keep stable, reusable technical knowledge in `references/` or
  `architecture/`, not in temporary drafts.
- Update the nearest index whenever a document is created, moved, promoted, or
  archived.
