# Stepwise Documentation Structure

This document defines the shared documentation layout used by Stepwise skills.
Skills should refer to these conventions instead of redefining folder and
navigation rules.

## Documentation Root

Project documentation belongs under the documentation root: `docs/` by
default, or the path declared under Repository Declarations. Paths in this
document are written with the default `docs/` root; substitute a declared root
throughout.

The documentation entry point is the documentation index: `docs/README.md` by
default, or the declared index. Agents should read that index before planning,
designing, implementing, or reviewing work. They should follow its links to the
smallest relevant document chain instead of scanning all files under the
documentation root.

## Repository Declarations

A repository may declare Stepwise settings in its instruction file (`AGENTS.md`
or the host equivalent) as bullets under a `Stepwise` heading. Every setting is
optional; when it is absent the default applies and behavior is unchanged.

- `Documentation root`: any repository-relative path, including a hidden
  directory. Default: `docs/`.
- `Documentation index`: repository-relative path of the index to read first.
  Default: `<documentation root>/README.md`.
- `Planning root`: any repository-relative path. Default: resolved by Planning
  Root Resolution.
- `Profile`: `full` (default) or `light`; see Execution Record Contract.
- `Decision log`: repository-relative path of the project's decision log.
- `Output markers`: `on` (default) or `off`.
- `Frontmatter`: `on` (default) or `off`; see Document Frontmatter. Where a
  skill says "under `Frontmatter: on`", it means this setting is in effect,
  declared or by default.

Example:

```markdown
## Stepwise

- Documentation root: .docs/
- Planning root: .docs/roadmap/
- Profile: light
```

Read the instruction file first and use declared values as written. Do not scan
for alternatives to a declared path. If a declaration is contradictory,
unrecognized (for example `Profile: minimal`), or points to a path outside the
repository, stop and ask the user.

`Output markers`: a declared value always applies. When it is not declared and
the repository instructions ask for plain output, markers are `off`; otherwise
they are `on`.

Written artifacts (plans, task documents, records, code comments) follow the
language required by the repository instructions; when they do not state one,
use the language of the existing documentation, or the user's language if there
is none. Chat replies follow the user's language.

When a decision log is declared, treat each entry not marked superseded,
rejected, or proposed as a confirmed constraint and cite it by its identifier in
the `Related` field of task and plan documents (the `decisions` key under
`Frontmatter: on`; in a brainstorming brief, in the constraints list). A decision introduced by brainstorming or planning is
labeled `Proposed` in the document that introduces it until the user confirms
it. Agents do not edit the decision log without the user's approval.

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

If the instruction file declares a `Planning root`, use it and skip candidate
scanning; create it with a `README.md` only when absent. Otherwise inspect
`docs/plans/`, `docs/work/`, and root-level `work/` (with a declared
documentation root in place of `docs/`) before creating a planning directory,
then apply this single rule:

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
`Dependencies` entries. Unless `Frontmatter: off` is declared, this section is
replaced by the document frontmatter and is omitted.

Agents should normally traverse this chain:

```text
AGENTS.md
  -> documentation index
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

## Document Frontmatter

Applies unless the instruction file declares `Frontmatter: off`, which keeps
`Document Context` and the record fields as defined elsewhere in this document. It lets Obsidian properties, Dataview, and Bases read planning
documents directly.

Every epic, milestone, phase, and task document begins with one YAML frontmatter
block containing exactly these keys in this order. Use `null` or `[]` for an
empty value; never omit a key or add another. The keys marked task only are
absent from other levels.

| Key | Value | Written by |
| --- | --- | --- |
| `type` | `epic`, `milestone`, `phase`, or `task` | planner |
| `id` | full path identifier such as `E1.M1.P1.T2`; the `E`, `M`, `P`, `T` identifiers are unchanged and an `id` never changes | planner |
| `title` | document name without the identifier | planner |
| `status` | `Planned`, `In Progress`, `Blocked`, or `Completed` | implementer; `Completed` only by the manager |
| `review` | task only: `Not Reviewed`, `Changes Requested`, `Accepted`, or `Not Required` | manager |
| `review_note` | task only: bounded basis for `review`, or `null` | manager |
| `created` | `YYYY-MM-DD` | planner |
| `updated` | `YYYY-MM-DD`; set by whoever changes the document | any writer |
| `resolved` | `YYYY-MM-DD` when `status` is `Completed`, otherwise `null` | manager |
| `priority` | `Critical`, `High`, `Medium`, `Low`, or `null` | user |
| `assignee` | role or name used by the repository, or `null` | user or manager |
| `labels` | list of allowed values, or `[]` | user, or planner from the user's stated values |
| `areas` | list of allowed values, or `[]` | user, or planner from the user's stated values |
| `parent` | `id` of the parent document, `null` for an epic | planner |
| `depends_on` | list of `id` values | planner |
| `related` | list of repository-relative paths or decision-log identifiers | planner |
| `decisions` | list of decision-log identifiers | planner |
| `next` | repository-relative path of the next document to read, or `null` | planner |

Rules:

- **One home per value.** A value in the frontmatter does not appear anywhere
  else in the document. The body therefore has no `Document Context` section,
  and the `Execution Record` omits `Status`, `Last Updated`, and `Review
  Outcome`. Wherever this document names those three record fields, read
  `status`, `updated`, and `review` with `review_note`.
- **Dependencies.** `depends_on` is the authoritative `Depends on` of
  high-level nodes and lists only hard-dependency identifiers for tasks. A
  task's `Prerequisites` section then holds only readiness conditions that are
  not identifiers and is omitted when there are none.
- **Derived data is not stored.** Children, progress, and counts are obtained by
  querying `parent` and `status`. The location of a document follows from its
  `id` and the hierarchy.
- **No invented values.** Agents never invent or change `priority`, `assignee`,
  `labels`, or `areas`. `labels` and `areas` use only the values listed in the
  `Allowed Values` section of the planning-root index; when none are listed,
  leave them empty.
- **Consistency.** `Completed` still requires every completion gate in the
  Execution Record Contract; `resolved` is set at that moment and only then.
- **Existing documents.** A document without frontmatter is tolerated. The next
  agent that edits it adds the frontmatter and removes the body fields it now
  duplicates in the same change.

## Execution Record Contract

### Profiles

The `full` profile applies unless the instruction file declares `Profile:
light`. The two profiles differ in one rule only: under `light`, a phase with
three or more tasks may keep its tasks inline in the phase document instead of
requiring one standalone document per task. Standalone task documents remain
allowed. Everything else, including the nine-field `Execution Record`, its
ownership, and its completion rules, is identical in both profiles.

Under `light`, each inline executable task carries its own `Execution Record`
beneath the task, and `Phase Implementation State` applies only to a phase whose
tasks are standalone documents.

### Applicability and location

Every stable-ID task has one authoritative location. When a phase has three or
more tasks, create one standalone task document for every task at planning time.
The phase document keeps only the ordered task links and concise phase-level
status, progress, blockers, and decisions; each task document owns its task
intent and `Execution Record`. In a phase with one or two tasks, a merely
planned inline task may remain under its parent plan without an `Execution
Record`. Before work begins, add the record beneath that inline task or extract
the task to one standalone document when it needs independent writer ownership
or an isolated cross-session handoff.

When extracting an inline task:

- create `<parent-id>/T<N> - <name>.md` beside the parent document's ID-only
  directory, preserving the normal hierarchy (for example,
  `E1/M1/P1/T1 - Task name.md` for a phase task);
- move, rather than copy, `Purpose`, optional `Prerequisites`, `Changes`,
  `Acceptance Checks`, and `Risks` into the task document;
- replace the former inline body with one link to the authoritative task
  document and, when `Document Context` is used, add that link to the parent's
  `Children` entry; and
- keep the `Execution Record` only at the authoritative task location.

Do not extract every planned inline task preemptively when its phase has one or
two tasks. A phase with three or more tasks is the required exception: create
all of its task documents at planning time. Parallel tasks that need different
writers must use different standalone task documents; sequential roles may
update one record only through explicit handoff of writer ownership.

When a phase grows to three or more tasks, extract every remaining inline task
before further work using the steps above.

### Phase Implementation State

A phase with standalone task documents carries one `Phase Implementation State`
section with `Progress` (manager-accepted completed tasks only), `Active task or
next action`, and `Phase-level blockers or decisions`. It is a concise roll-up,
never a copy of task scope, evidence, deviations, or `Execution Record` content.
The task-index checkbox is the only per-task state the phase document carries.

- The manager alone sets the task-index checkbox, the `Progress` count, and the
  phase `Status`, and only on acceptance.
- An implementer may refresh `Active task or next action` and blockers only when
  it is the sole active writer for that phase; with parallel tasks the manager
  maintains the roll-up from the implementers' reports.

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
- Implemented Scope:
  - <one bounded completed outcome per bullet>
- Relevant Files and Symbols:
  - [<repository-relative path>](<relative link>) — `<Symbol>`
- Acceptance Evidence:

  | Acceptance Check | Status | Class | Evidence |
  | --- | --- | --- | --- |
  | <exact Acceptance Checks checkbox text> | <Verified \| Failed \| Blocked \| Not Run \| Not Applicable> | <Automated \| Simulated Host \| Native Host \| Browser \| Manual \| Documentation \| Human Assessment> | <bounded evidence reference or reason> |
- Decisions and Deviations:
  - <Decision | Deviation | Not Applicable>: <bounded statement> — <reference>
- Review Outcome: <Not Reviewed | Changes Requested | Accepted | Not Required> — <bounded disposition or reason>
- Remaining Work:
  - <one specific remaining item per bullet>
- Resume or Closure: <Resume by <exact next action> | Closed: <closure statement>>
```

Field shapes are fixed:

- A list field with nothing to record is written as one line, `- <Field>: None`;
  otherwise it holds nested bullets as shown. The list fields are `Implemented
  Scope`, `Relevant Files and Symbols`, `Decisions and Deviations`, and
  `Remaining Work`.
- `Acceptance Evidence` is a table with one row per acceptance check. The first
  cell repeats the checkbox text verbatim. A `|` inside a cell is written `\|`.
- `Resume or Closure` is one sentence that starts with `Resume by` while the
  task is not `Completed` and with `Closed:` once it is.
- `Last Updated` is a portable calendar date, not a machine-local timestamp. In
  a standalone task document, `Execution Record` status must match `Document
  Context` status. Records in the earlier free-form layout are tolerated and are
  rewritten into these shapes the next time their writer updates them. Under
  `Frontmatter: on`, `Status`, `Last Updated`, and `Review Outcome` are not
  record fields.

### Evidence and completion rules

- Evidence status is limited to `Verified`, `Failed`, `Blocked`, `Not Run`, or
  `Not Applicable`. `Not Applicable` requires a bounded reason.
- Evidence class is limited to `Automated`, `Simulated Host`, `Native Host`,
  `Browser`, `Manual`, `Documentation`, or `Human Assessment`. One class must
  not be promoted to another.
- `Human Assessment` is a person's judgment of a subjective quality (for example
  visual polish, tone, or usability feel); `Manual` is a hand-run check of an
  objectively defined expected result. `Human Assessment` cannot be promoted to
  or replaced by an automated class. A check that depends on subjective
  judgment is `Verified` only with a recorded human decision (decider role or
  name and a bounded outcome reference). Until then its status is `Not Run` or
  `Blocked`, never `Not Applicable`, and the task cannot be `Completed`.
- Each acceptance check must have one evidence row. Reference the command,
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
