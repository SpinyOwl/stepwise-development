# Stepwise Development Workflow

Use the shortest workflow that preserves the decisions and verification the
change actually needs:

    ambiguous/high-impact request -> stepwise-brainstorm skill -> approved design -> same-context stepwise-planner skill -> implementation
    well-scoped request or approved task -> direct planning or implementation

Authoritative repository documents hold scope, dependencies, decisions,
Execution Records, and cross-session handoffs. Host approval and permission
boundaries remain authoritative for every tool effect.

## Use Brainstorming

Start the stepwise-brainstorm skill when work has one or more of these
characteristics:

- Materially unclear product or API behavior.
- Competing architecture or ownership choices.
- Broad or uncertain scope.
- Migration, compatibility, security, privacy, or data-loss risk.
- A user request to brainstorm or compare approaches.

The design must be approved before the brainstorming workflow invokes the
stepwise-planner skill in its existing context. Reuse the project context
already gathered. A separate planning-brief approval is needed only when the
brief introduces a new material decision.

Brainstorming otherwise remains read-only. Its project-artifact exceptions are
explicitly approved draft or design documents, approved mockups, and plan
artifacts created by the same-context planner after design approval.

An installed s-brainstormer custom agent may host this workflow, but it is
optional. The stepwise-brainstorm skill remains the portable contract.

## Bypass Brainstorming

Well-scoped work with clear intended behavior does not require brainstorming.
Examples include focused bug fixes, small refactors, localized documentation
or configuration changes, implementation of approved plan tasks, and routine
Git or file operations.

Ask one focused question instead of starting a full brainstorm when a single
answer would make the work safe. Do not repeat brainstorming for an approved
design or plan unless implementation exposes a new material decision.

## Navigation

Read docs/README.md and follow the smallest relevant document chain before
substantive work. Do not scan all project documentation.
