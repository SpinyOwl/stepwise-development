# Project Instructions

## Documentation

Before planning, designing, implementing, or reviewing:

1. Read `docs/README.md`.
2. Follow its document map to the relevant section.
3. Read the parent document chain to the assigned document.
4. Read only explicitly linked supporting documents required for the task.

Do not scan all files under `docs/`.

## Default Development Workflow

Route every substantive request through `s-brainstormer` before planning,
designing, or implementing it. This includes feature requests, bug fixes,
refactors, architecture changes, workflow changes, and documentation changes
that affect project behavior or structure.

Do not invoke brainstorming for routine Git or file-operation requests, such as
status, diff, log, add, commit, branch operations, listing files, reading
files, searching files, copying files, or moving files. Perform those directly
when requested.

The normal path for substantive work is:

```text
user request -> s-brainstormer -> approved design -> s-planner -> implementation
```

Do not skip the brainstorming step because the request appears small unless it
is a routine Git or file operation. If `s-brainstormer` is not installed,
report that the default workflow cannot be followed and ask whether to proceed
directly or install the agent.
