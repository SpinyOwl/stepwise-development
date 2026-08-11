# Stepwise Development Workflow

Use the shortest workflow that preserves the decisions and verification the
change actually needs:

```text
ambiguous/high-impact request -> s-brainstormer -> approved design -> s-planner -> implementation
well-scoped request or approved task -> direct planning or implementation
```

## Use Brainstorming

Start `s-brainstormer` when work has one or more of these characteristics:

- Materially unclear product or API behavior.
- Competing architecture or ownership choices.
- Broad or uncertain scope.
- Migration, compatibility, security, privacy, or data-loss risk.
- A user request to brainstorm or compare approaches.

The design must be approved before invoking `s-planner`. The brainstormer may
prepare and self-review the planner handoff without a second approval unless it
introduces a new material decision.

## Bypass Brainstorming

Well-scoped work with clear intended behavior does not require brainstorming.
Examples include focused bug fixes, small refactors, localized documentation
or configuration changes, implementation of approved plan nodes, and routine
Git or file operations.

Ask one focused question instead of starting a full brainstorm when a single
answer would make the work safe to execute. Do not repeat brainstorming for an
approved design or plan unless implementation exposes a new material decision.

## Navigation

Read `docs/README.md` and follow the relevant document chain before substantive
work. Do not scan all project documentation.
