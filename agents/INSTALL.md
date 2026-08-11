# Agent Installation

Use this guide to install repository agents into OpenCode, Codex, Claude Code,
or another agentic tool. Agent registration is tool-specific, so an external
repository reference alone does not make an agent invocable.

Choose one installation method for each agent:

1. Local wrapper adapter: recommended when symbolic links are unavailable or
   undesired.
2. Native link: recommended when the local tool supports links and should track
   repository updates automatically.

## Prerequisites

Replace these placeholders in commands and templates:

```text
<repository-root> = G:/Programming/OpenCode/stepwise-development
<agent-name>      = s-brainstormer
```

The canonical behavioral prompt for an agent is:

```text
<repository-root>/agents/<agent-name>/prompt.md
```

The following Stepwise agents currently include complete OpenCode adapters:

```text
s-brainstormer
s-manager
s-planner
s-reviewer
s-implementer
```

Install only the adapters required by the intended workflow. `s-planner` is an
optional standalone entry point for planning. The brainstorming workflow
invokes the `stepwise-planner` skill in the brainstormer's existing context,
so it does not require or delegate to the `s-planner` agent.

## Existing Installations

Before installing, inspect the target tool directories and determine whether
each skill or agent is absent, already installed from this repository, or
owned by another source.

- If the exact repository link already exists, leave it in place.
- If the exact repository file already exists, leave it in place and verify it
  against the current source.
- If a different file or directory uses the same name, do not overwrite it.
  Ask whether to replace it, choose another name, or skip that item.
- If a skill directory exists, copy only missing supporting files by default.
  Do not overwrite local `SKILL.md`, templates, references, scripts, or other
  assets without approval.
- If an agent definition exists, preserve its local model, permission,
  sandbox, description, and other tool-specific settings unless replacement is
  explicitly approved.
- If a previous installation used a wrapper, do not silently replace it with a
  native link. Ask before changing the installation method.
- If the same skill or agent is installed at both project and user scope,
  identify which one the tool will load and avoid modifying the other scope
  implicitly.
- Treat `folder-structure.md` the same way: compare it with the canonical
  convention and ask before replacing a locally modified copy.

An update may be performed after approval by replacing the complete managed
package or recreating the native link. Preserve unrelated files and report
every skipped, preserved, replaced, and newly created path.

## Option 1: Local Wrapper Adapter

Create a small, tool-local agent definition that instructs the agent to read
the canonical `prompt.md` before work begins. This avoids symbolic links while
keeping the behavioral instructions versioned in the repository.

Use the matching wrapper template:

| Tool | Wrapper template | Local agent location |
| --- | --- | --- |
| OpenCode | `templates/agent/opencode-wrapper.md` | `.opencode/agents/<agent-name>.md` or `~/.config/opencode/agents/<agent-name>.md` |
| Codex | `templates/agent/codex-wrapper.toml` | `.codex/agents/<agent-name>.toml` or `~/.codex/agents/<agent-name>.toml` |
| Claude Code | `templates/agent/claude-wrapper.md` | `.claude/agents/<agent-name>.md` or `~/.claude/agents/<agent-name>.md` |

For each wrapper:

1. Replace `<repository-root>` and `<agent-name>`.
2. Set the local description and tool or permission policy appropriate to the
   agent's role.
3. Keep model settings omitted so the local tool selects or inherits its model.
4. Restart the tool if it does not detect a newly created agent directory or
   definition.

The wrapper dynamically follows changes to the repository's `prompt.md`.
Permissions, sandbox settings, models, names, and descriptions remain local
configuration and must be updated separately when needed.

### OpenCode Wrapper Access

The wrapper's process must be able to read the repository path. The current
global OpenCode configuration provides this through the `spinyowl-ai`
reference. A reference grants repository access, but does not register an
agent; the local wrapper remains required.

```jsonc
{
  "references": {
    "spinyowl-ai": {
      "path": "G:/Programming/OpenCode/stepwise-development",
      "description": "Use for versioned reusable skills, agent adapters, and workflows."
    }
  }
}
```

## Option 2: Native Link

Create a native symbolic link from the tool's local agent directory to the
tool-specific adapter in this repository. This keeps behavior, metadata, and
permissions fully repository-managed.

### OpenCode

Link the repository's `opencode.md` to the agent name OpenCode loads:

```bash
ln -s "<repository-root>/agents/<agent-name>/opencode.md" \
  "~/.config/opencode/agents/<agent-name>.md"
```

On Windows, create the link from an elevated Command Prompt or after enabling
Developer Mode:

```text
mklink "%USERPROFILE%\.config\opencode\agents\<agent-name>.md" "G:\Programming\OpenCode\stepwise-development\agents\<agent-name>\opencode.md"
```

OpenCode has no external `agents.paths` setting. A native link is therefore
the only no-copy way to load an external OpenCode adapter as an installed
agent.

### Codex

When an agent package includes `codex.toml`, link it into Codex's agent
directory:

```bash
ln -s "<repository-root>/agents/<agent-name>/codex.toml" \
  "~/.codex/agents/<agent-name>.toml"
```

### Claude Code

When an agent package includes `claude.md`, link it into Claude Code's agent
directory:

```bash
ln -s "<repository-root>/agents/<agent-name>/claude.md" \
  "~/.claude/agents/<agent-name>.md"
```

## Bundled Skill Assets

When installing a skill, copy or link its complete directory rather than only
`SKILL.md`. A skill may include required `references/`, `templates/`, scripts,
or other supporting assets.

The shared folder convention is installed alongside the skills. Copy
`conventions/folder-structure.md` to the root of the installed skills
directory as `folder-structure.md`.

At runtime, `s-setup` checks for the convention in the target project's
`docs/folder-structure.md`, then at the installed skills root, then at
`conventions/folder-structure.md` in a direct source-repository checkout. This
allows the same skill package to run against an existing project copy, a
normal installation, or this repository without a separate install step.

The `s-setup` skill must be installed with all of these files intact:

```text
skills/s-setup/
  SKILL.md
  templates/AGENTS.md
  templates/docs/README.md
  templates/docs/<section>/README.md
  templates/planning-root/README.md
folder-structure.md
```

After installation, invoke `s-setup` explicitly in the target project to copy
the documentation template. Installing the skill alone must not modify
the target project.

The setup skill applies the planning-root rule in the shared convention. It
uses the only existing candidate among `docs/plans/`, `docs/work/`, and
root-level `work/`; asks which is authoritative when multiple candidates
exist; and creates `docs/plans/` only when none exists. It creates or preserves
`README.md` only in the resolved root, so legacy projects are not given a
second `docs/plans/` root.

## Other Agentic Tools

Use the tool's native agent registration mechanism. When it supports an
external agent file or symbolic link, link the repository adapter. Otherwise,
create a local wrapper that reads the canonical `prompt.md` from this
repository before working.

No universal standard exists for agent registration, permissions, or sandbox
configuration. The portable `prompt.md` is the shared behavior source; each
tool-local adapter controls its own metadata and capabilities.

## Uninstallation

See [UNINSTALL.md](UNINSTALL.md) for safe removal instructions for native
links, wrappers, copied adapters, skills, and shared assets.

## Verification

1. Restart the target tool when required.
2. Confirm the agent appears in its agent selector or subagent list.
3. Invoke the agent with a small task and confirm it reads its canonical
   `prompt.md` before acting.
4. For linked agents, change a harmless line in the repository adapter and
   confirm the installed definition reflects the change after reload.
