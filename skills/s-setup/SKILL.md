---
name: s-setup
description: Initialize a project with the Stepwise documentation structure, indexes, planning-root compatibility rules, and local agent navigation instructions. Use when setting up Stepwise development in a new or existing project.
---

# Stepwise Project Setup

Initialize the current project with the bundled Stepwise documentation
conventions. This skill changes the project only when explicitly invoked.

## Setup Rules

1. Identify the project root and inspect its existing documentation and
   planning directories before creating anything.
2. Locate the shared folder convention by checking these paths in order and
   use the first file that exists:
   - `<project-root>/docs/folder-structure.md` (existing project copy).
   - `<skills-root>/folder-structure.md` (installed beside the `s-setup/`
     skill directory).
   - `<repository-root>/conventions/folder-structure.md` (direct source
     checkout; from `skills/s-setup/`, this is `../../conventions/`).
   If none exists, stop and report the missing required asset.
3. Preserve existing files and directories. Never overwrite an existing file
   without explicit user approval.
4. Create only missing files and directories.
5. Resolve the planning root exactly once using the shared convention. Do not
   restate or substitute a different precedence rule.
6. Create or preserve `README.md` in the resolved planning root only. In
   particular, when `docs/work/` or root-level `work/` is resolved, do not
   create `docs/plans/` or `docs/plans/README.md`.
7. Report every created, preserved, skipped, and ambiguous path.

## Mandatory Baseline

Create only the following baseline files and directories when absent:

```text
AGENTS.md
docs/
  README.md
  folder-structure.md
<resolved-planning-root>/
  README.md
```

Use `templates/AGENTS.md` as the starting point for a missing project
instruction file, `templates/docs/README.md` for a missing documentation
index, and `templates/planning-root/README.md` for a missing index in the
resolved planning root. Replace template placeholders with paths relative to
the target file. Do not overwrite an existing planning-root index.

## Optional Documentation

Create `architecture/`, `decisions/`, `guides/`, `references/`, `design/`, or
`drafts/` and their indexes only when the user requests them or existing
documents require an index. The default
`guides/stepwise-development-workflow.md` is optional documentation, not part
of the mandatory baseline. Use the corresponding files under
`templates/docs/` when creating requested optional documentation.

The generated `AGENTS.md` must instruct agents to start with `docs/README.md`
and follow the documented chain rather than scanning all project documents.
It must also install the default workflow rule from `templates/AGENTS.md`:
ambiguous or high-impact requests go through `s-brainstormer`, while
well-scoped changes, approved plan tasks, and routine Git or file operations
may proceed directly.
The generated `docs/README.md` must link to the convention document, the
resolved planning-root index, and every optional section index that actually
exists. It must identify the resolved planning root. Do not leave links to
optional sections that were not created.

When `docs/folder-structure.md` is absent, copy the convention found by the
lookup rule to that path. An existing project copy is preserved in place.

## Existing Projects

When the project already has documentation:

- Integrate links into the existing `docs/README.md` when safe.
- Do not move or rename existing documents automatically.
- Do not create a second planning root.
- Preserve existing project instructions and add the Stepwise navigation rule
  to them only with user approval when the file already contains instructions.
- Ask before changing an existing `AGENTS.md`, `opencode.json`, or equivalent
  tool configuration.
- If an existing instruction file has a different default workflow, report the
  conflict and ask before adding the Stepwise workflow rule.

## Verification

After setup, verify that:

- `docs/README.md` exists and links to the convention document.
- Every created section has an index.
- The resolved planning root is stated in `docs/README.md`.
- The navigation chain starts from the project's instruction file.
- No existing file was overwritten.

Report manual follow-up when the project has an existing index or instruction
system that could not be changed without approval.
