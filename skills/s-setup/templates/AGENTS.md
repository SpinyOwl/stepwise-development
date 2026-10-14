# Project Instructions

## Documentation

Before planning, designing, implementing, or reviewing:

1. Read the documentation index declared in this file; otherwise `README.md` in
   the declared documentation root; otherwise `docs/README.md`.
2. Follow its document map to the relevant section.
3. Read the parent document chain to the assigned document.
4. Read only explicitly linked supporting documents required for the task.

Do not scan all files under `docs/`.

## Default Development Workflow

Use the `sw-brainstorm` skill before planning or implementation when the
request has materially unclear product behavior, competing architecture
choices, broad or uncertain scope, migration or compatibility risk, or other
decisions that need user agreement. Also use it whenever the user explicitly
asks to brainstorm or compare approaches.

When a custom `s-brainstormer` agent is installed, it may host this skill. The
skill is the portable workflow contract; the custom agent is optional.

Proceed directly when the request is well scoped and the intended behavior is
clear. This includes focused bug fixes, small refactors, localized
documentation or configuration changes, and implementation of an already
approved plan or assigned Stepwise task. Ask one focused question instead
of starting a full brainstorm when a single answer would remove the ambiguity.

Do not invoke brainstorming for routine Git or file-operation requests, such as
status, diff, log, add, commit, branch operations, listing files, reading
files, searching files, copying files, or moving files. Perform those directly
when requested.

Choose the shortest responsible path:

```text
ambiguous/high-impact request -> sw-brainstorm skill -> approved design -> same-context sw-planner skill -> implementation
well-scoped request or approved task -> direct planning or implementation
```

After design approval, the brainstormer invokes the `sw-planner` skill in
its existing context rather than delegating planning to a separate agent.
Already gathered project context should not be redundantly reloaded.
Brainstorming otherwise remains read-only except for explicitly user-approved
draft, design, or mockup artifacts and plan artifacts created by the planner
skill after design approval.

Do not repeat brainstorming for work that already has an approved design or
plan unless implementation exposes a new material decision. If the
`sw-brainstorm` skill is unavailable, briefly explain the missing
capability and ask whether to proceed with focused clarification instead.
