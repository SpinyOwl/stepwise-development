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

## Standard Sections

Use these sections when they are needed:

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
  plans/
```

- `architecture/` contains system boundaries, module structure, and technical
  architecture documentation.
- `decisions/` contains accepted architectural or product decisions.
- `guides/` contains user, contributor, and operational how-to documentation.
- `references/` contains stable API, domain, and technical reference material.
- `design/` contains active design proposals that are more developed than
  drafts but not yet accepted decisions.
- `drafts/` contains incomplete or exploratory documents.
- `plans/` contains epics, milestones, phases, and implementation tasks.

Optional sections should not be created until they contain a document.
Each created section should have a `README.md` index.

## Planning Root Resolution

New projects must use `docs/plans/` as the planning root.

For compatibility with existing projects, resolve the planning root in this
order:

1. Use existing `docs/plans/`.
2. Otherwise use existing `docs/work/`.
3. Otherwise use existing root-level `work/`.
4. If none exists, create and use `docs/plans/`.

Never create a new `work/` directory. If more than one candidate already
exists, stop and ask the user which planning root is authoritative.

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

Phase contents are implementation task nodes, not unnamed steps.

## Document Navigation

Every index should identify:

- What belongs in its section.
- What does not belong there.
- The documents currently available.
- The recommended reading path.
- Which documents are authoritative.

Every planning document should include a `Document Context` section with
links to its parent, children when applicable, related architecture or
decision documents, and the next document to read.

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

## Graph Metadata

Every planning graph node must include:

- `Depends on`
- `Enables`
- `Parallelizable with`

Use `None` when a relationship is empty. Dependency fields are authoritative;
Mermaid diagrams are derived views and must match them.

## Document Lifecycle

- Move exploratory material from `drafts/` to `design/` when it becomes a
  coherent proposal.
- Move accepted proposals from `design/` to `decisions/` when they become
  authoritative decisions.
- Keep stable, reusable technical knowledge in `references/` or
  `architecture/`, not in temporary drafts.
- Update the nearest index whenever a document is created, moved, promoted, or
  archived.
