# Agent Uninstallation

Use this guide to remove Stepwise agents and skills from OpenCode, Codex,
Claude Code, or another agentic tool without deleting repository source files
or unrelated user configuration.

Uninstallation is not repository deactivation. Installed components never
activate a workflow by themselves, and removing them must not edit the
repository-owned instruction that opted in. Follow the canonical
[activation procedure](../skills/s-setup/references/activation.md#deactivation-and-uninstallation)
for a separately approved deactivation change.

## Safety Rules

Uninstall only tool-local files created for this repository. Do not delete the
repository source, shared tool directories, unrelated agents, or project
configuration owned by the user.

Before removing an item:

1. Identify whether it is a native link, a wrapper, or a copied adapter.
2. Confirm that its name and contents correspond to this repository's agent.
3. Check whether a project-level installation shadows a user-level one.
4. Ask before removing an item that has local changes or cannot be verified.

## Native Links

Remove only the link from the tool's agent directory. Do not remove the target
file under `<repository-root>/agents/`.

On Unix-like systems:

```bash
rm "<agent-install-dir>/<agent-name>.<extension>"
```

On Windows, remove only the link itself:

```text
del "<agent-install-dir>\<agent-name>.<extension>"
```

If the path is a regular file rather than a link, stop and use the copied-file
procedure below instead.

## Wrappers and Copied Adapters

Remove the local wrapper or copied adapter only after verifying that it was
created for this repository. Preserve any local permissions, model settings,
descriptions, or unrelated configuration that cannot be separated safely.

Remove the file from the applicable location:

| Tool | Project scope | User scope |
| --- | --- | --- |
| OpenCode | `.opencode/agents/<name>.md` | `~/.config/opencode/agents/<name>.md` |
| Codex | `.codex/agents/<name>.toml` | `~/.codex/agents/<name>.toml` |
| Claude Code | `.claude/agents/<name>.md` | `~/.claude/agents/<name>.md` |

Do not remove the containing agent directory if it contains other definitions.

## Skills

When uninstalling the Stepwise skill bundle, remove only the installed
Stepwise skill directories. Required conventions are bundled inside the
dependent skill directories; there is no loose shared `folder-structure.md`
asset to remove.

Do not remove generated project documentation such as `docs/`,
`docs/folder-structure.md`, or `AGENTS.md` as part of tool uninstallation.
Those files belong to the project and require a separate user-directed
deactivation or cleanup approval.

## Verification

After uninstallation:

1. Restart the target tool.
2. Confirm the removed agents no longer appear in its selector or subagent list.
3. Confirm unrelated agents and project-level installations still work.
4. Confirm the repository's `agents/` source files are unchanged.
