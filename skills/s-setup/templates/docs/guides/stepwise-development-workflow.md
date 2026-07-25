# Stepwise Development Workflow

Substantive project requests follow this sequence:

```text
user request -> s-brainstormer -> approved design -> s-planner -> implementation
```

## Use Brainstorming

Start `s-brainstormer` before planning, designing, or implementing:

- Features.
- Bug fixes.
- Refactors.
- Architecture changes.
- Workflow changes.
- Documentation changes that affect project behavior or structure.

The design must be approved before preparing a planning brief. The planning
brief must be approved before invoking `s-planner`.

## Bypass Brainstorming

Routine Git and file operations do not require brainstorming. Examples include
checking status, viewing a diff or log, staging or committing changes, branch
operations, listing files, reading files, searching files, copying files, and
moving files.

When the request combines routine file operations with substantive changes,
perform the routine operation first and route the substantive change through
the default workflow.

## Navigation

Read `docs/README.md` and follow the relevant document chain before substantive
work. Do not scan all project documentation.
