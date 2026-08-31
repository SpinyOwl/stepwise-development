# Agentic AI Library

Portable source library for reusable skills, agent configurations, development
workflows, and authoritative Markdown handoffs. OpenCode, Codex, and Claude
Code are the primary targets, while the source formats remain usable with
other agentic AI tools.

Stepwise is a skills-only, file-first workflow library. It does not require a
Stepwise runtime, CLI, persisted runtime state, or generated index. Project
documents, the repository, Git, and current verification evidence remain the
authoritative sources for continuing work across agent sessions. The current
product consists only of the definitions and conventions listed below.

## Layout

```text
skills/
  <skill-name>/
    SKILL.md
agents/
  <agent-name>/
    prompt.md
    opencode.md
    codex.toml
    claude.md
workflows/
  <workflow-name>.md
templates/
  skill-template.md
  agent/
    prompt.md
    opencode.md
    codex.toml
    claude.md
    opencode-wrapper.md
    codex-wrapper.toml
    claude-wrapper.md
  workflow-template.md
conventions/
  folder-structure.md
```

- `skills/` follows the open [Agent Skills specification](https://agentskills.io/specification).
  This is the most portable executable format in the repository.
- `agents/` stores one neutral prompt and optional platform adapters per agent.
- `workflows/` stores tool-neutral process documents. A workflow that should be
  automatically discovered or invoked belongs in `skills/` instead.
- `templates/` contains non-loadable starter files.
- `conventions/` contains shared rules referenced by multiple skills and
  bundled with setup skills when they must be installed into a target project.

## Portability Model

Skills are shared directly because all three primary tools support
`<skill-name>/SKILL.md`. Keep standard frontmatter portable and isolate any
vendor extension behind an explicit compatibility note.

Custom agent formats are not standardized. Each agent package therefore uses
`prompt.md` as the canonical, vendor-neutral behavior definition and includes
standalone adapters for supported tools. Adapter prompt bodies intentionally
duplicate `prompt.md` because the tools do not share a portable include
mechanism.

For another agentic AI system:

1. Use `SKILL.md` directly if it supports the Agent Skills standard.
2. Use an agent's `prompt.md` as its system or developer prompt.
3. Follow files in `workflows/` as procedural instructions.
4. Add a thin adapter only when the system requires proprietary metadata.

## Installation

Give a generic agent the self-contained [installation guide](INSTALL.md). The
supported boundary installs only selected skills and optional custom-agent
definitions, classifies native destinations, requires narrow approval,
preserves unrelated files, and re-inspects for no-op convergence.

Installation never opts a repository into the Stepwise workflow. Use the
separate [activation guide](ACTIVATE.md) only when a repository owner asks to
add or integrate an explicit Stepwise instruction. Deactivation is an
instruction change, while uninstallation removes verified installed
components; neither operation implies the other.

`INSTALL.md` and `ACTIVATE.md` are stable root entrypoints to the canonical
package-local procedures shipped with `s-setup`. The package contains one
install/update owner and one workflow opt-in owner, so copied definitions work
without creating duplicate procedure authorities. Neither operation changes
unrelated repository-owned legacy data.

### Skills

Copy or link each complete skill directory into a supported location:

| Tool | Project | User |
| --- | --- | --- |
| OpenCode | `.opencode/skills/` | `~/.config/opencode/skills/` |
| Codex | `.agents/skills/` | `~/.agents/skills/` |
| Claude Code | `.claude/skills/` | `~/.claude/skills/` |

OpenCode also discovers skills from `.agents/skills/` and `.claude/skills/`,
making either location useful when tools share a project.

OpenCode can alternatively load this repository directly with `skills.paths`:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "skills": {
    "paths": ["/path/to/agentic-ai-library/skills"]
  }
}
```

Skills are packages, not just `SKILL.md` files. Install the complete skill
directory, including `references/`, `templates/`, and any scripts or other
supporting assets. See the [skill installation guide](skills/INSTALL.md) for
package boundaries and native destination details inside the canonical
installation procedure.
Installing a skill does not activate a target repository.

### Agents

Use the [agent installation guide](agents/INSTALL.md) for native adapter details
inside the canonical installation procedure. Custom agents are optional
extensions, and their
required skills must use the same selected scope. OpenCode, Codex, and Claude
Code adapters are committed.

When an adapter is installed directly, use the target tool's standard agent
location:

| Adapter | Project | User |
| --- | --- | --- |
| `opencode.md` | `.opencode/agents/<name>.md` | `~/.config/opencode/agents/<name>.md` |
| `codex.toml` | `.codex/agents/<name>.toml` | `~/.codex/agents/<name>.toml` |
| `claude.md` | `.claude/agents/<name>.md` | `~/.claude/agents/<name>.md` |

Restart a tool when a newly installed definition is not discovered. OpenCode
requires a restart after configuration-time changes.

## Adding Content

1. Copy the relevant starter from `templates/`.
2. Follow the guidance in the destination directory's README.
3. Keep portable behavior in the canonical source and vendor settings in
   adapters.
4. Test on every platform claimed by the content.
5. Commit required supporting files, but never credentials, tokens, generated
   output, or machine-specific runtime state.
