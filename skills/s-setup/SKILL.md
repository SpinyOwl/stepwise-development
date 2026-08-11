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
2. Read the installed shared `folder-structure.md` file, located beside the
   `s-setup/` skill directory in the skills directory, as the source of truth
   for the generated structure.
3. Preserve existing files and directories. Never overwrite an existing file
   without explicit user approval.
4. Create only missing files and directories.
5. Use `docs/plans/` for new projects.
6. If an existing `docs/plans/`, `docs/work/`, or root-level `work/` directory
   is present, use the planning-root resolution rule from the convention
   document. If multiple candidates exist, ask the user to choose one.
7. Report every created, preserved, skipped, and ambiguous path.

## Generated Structure

Create the following baseline files and directories when absent:

```text
AGENTS.md
docs/
  README.md
  folder-structure.md
  architecture/README.md
  decisions/README.md
  guides/README.md
  references/README.md
  plans/README.md
  guides/stepwise-development-workflow.md
```

Use `templates/AGENTS.md` as the starting point for a missing project
instruction file and the files under `templates/docs/` as the starting point
for documentation indexes and the default workflow document.

Create `design/README.md` and `drafts/README.md` only when the user requests
those optional sections or when existing documents require them.

The generated `AGENTS.md` must instruct agents to start with `docs/README.md`
and follow the documented chain rather than scanning all project documents.
It must also install the default workflow rule from `templates/AGENTS.md`:
ambiguous or high-impact requests go through `s-brainstormer`, while
well-scoped changes, approved plan tasks, and routine Git or file operations
may proceed directly.
The generated `docs/README.md` must link to the convention document and every
created section index, and must identify the resolved planning root.

Copy the installed shared `folder-structure.md` file to
`docs/folder-structure.md`.

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
