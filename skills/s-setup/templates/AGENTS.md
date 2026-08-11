# Project Instructions

## Documentation

Before planning, designing, implementing, or reviewing:

1. Read `docs/README.md`.
2. Follow its document map to the relevant section.
3. Read the parent document chain to the assigned document.
4. Read only explicitly linked supporting documents required for the task.

Do not scan all files under `docs/`.

## Default Development Workflow

Use `s-brainstormer` before planning or implementation when the request has
materially unclear product behavior, competing architecture choices, broad or
uncertain scope, migration or compatibility risk, or other decisions that need
user agreement. Also use it whenever the user explicitly asks to brainstorm or
compare approaches.

Proceed directly when the request is well scoped and the intended behavior is
clear. This includes focused bug fixes, small refactors, localized
documentation or configuration changes, and implementation of an already
approved plan or assigned Stepwise task node. Ask one focused question instead
of starting a full brainstorm when a single answer would remove the ambiguity.

Do not invoke brainstorming for routine Git or file-operation requests, such as
status, diff, log, add, commit, branch operations, listing files, reading
files, searching files, copying files, or moving files. Perform those directly
when requested.

Choose the shortest responsible path:

```text
ambiguous/high-impact request -> s-brainstormer -> approved design -> s-planner -> implementation
well-scoped request or approved task -> direct planning or implementation
```

Do not repeat brainstorming for work that already has an approved design or
plan unless implementation exposes a new material decision. If brainstorming
is warranted but `s-brainstormer` is unavailable, briefly explain the missing
capability and ask whether to proceed with focused clarification instead.
