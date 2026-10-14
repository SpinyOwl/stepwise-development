# Agent Installation

This is the native-adapter reference for the canonical
[installation procedure](../INSTALL.md), not an independent installation or
update. Custom agents are optional; a valid Stepwise installation may contain
none. Publishing an agent definition does not activate a repository.

## Available Adapters

Each agent directory contains a canonical prompt.md plus native adapters for the
hosts listed here. A host not listed installs skills only unless the user
approves a drafted adapter; see the canonical procedure and the adapter
contract in [README.md](README.md#adapter-contract).

| Host | Source adapter | Project destination | User destination |
| --- | --- | --- | --- |
| Codex | agents/<name>/codex.toml | .codex/agents/<name>.toml | <user-home>/.codex/agents/<name>.toml |
| OpenCode | agents/<name>/opencode.md | .opencode/agents/<name>.md | <user-home>/.config/opencode/agents/<name>.md |
| Claude | agents/<name>/claude.md | .claude/agents/<name>.md | <user-home>/.claude/agents/<name>.md |

The available optional agents are:

    s-brainstormer
    s-implementer
    s-manager
    s-planner
    s-reviewer

Install only the adapter matching the host, using the destinations resolved and
confirmed in the canonical procedure. The canonical prompt is
the parity source, not a separately installed component.

## Dependencies

Resolve the complete same-scope closure before destination inspection:

| Agent | Required installed definitions |
| --- | --- |
| s-brainstormer | sw-brainstorm and sw-planner skills |
| s-implementer | sw-implementer skill |
| s-manager | sw-manager and sw-implementer skills, plus s-implementer |
| s-planner | sw-planner skill |
| s-reviewer | None |

s-planner is an optional standalone entry point. The brainstormer applies the
planner skill in its own context and does not require s-planner.

## Preserve Native Policy

Adapter metadata carries native host permissions, tools, sandboxing, and
delegation policy. Do not translate one host adapter into another or normalize
away host-specific metadata. OpenCode skill allowlists name only the workflow
skills used by that role. Reviewer read-only capability gaps remain explicit:
when a host cannot expose the current diff read-only, the reviewer reports the
gap and the manager verifies current-diff identity before acceptance.

Automated adapter parity demonstrates source consistency, not live-host
behavior. Host approval and permission boundaries remain authoritative.

## Inspect, Publish, and Verify

Follow the canonical
[installation procedure](../INSTALL.md):

1. inspect the selected source adapter and every discovered destination;
2. classify exact copies, conflicts, and missing definitions;
3. show the same-scope dependency closure and exact action plan;
4. obtain approval for the complete plan;
5. publish only approved adapter files, preserving local metadata unless its
   replacement was explicitly approved;
6. verify exact bytes, dependencies, adapter/canonical body parity, and a
   second no-op inspection.

Do not overwrite a different definition or retarget a link automatically.
Report every created, updated, preserved, skipped, and conflicting path.
Activation, deactivation, and uninstallation remain separately approved
operations; see the canonical
[activation procedure](../skills/sw-setup/references/activation.md) and
[UNINSTALL.md](UNINSTALL.md).
