# Agentic AI Library

Portable source library for reusable skills, agent configurations, and
development workflows. OpenCode, Codex, and Claude Code are the primary
targets, while the source formats remain usable with other agentic AI tools.

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
  workflow-template.md
```

- `skills/` follows the open [Agent Skills specification](https://agentskills.io/specification).
  This is the most portable executable format in the repository.
- `agents/` stores one neutral prompt and optional platform adapters per agent.
- `workflows/` stores tool-neutral process documents. A workflow that should be
  automatically discovered or invoked belongs in `skills/` instead.
- `templates/` contains non-loadable starter files.

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

### Agents

Install the adapter matching the target tool and rename it to the agent name:

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
