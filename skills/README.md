# Skills

Skills follow the open [Agent Skills specification](https://agentskills.io/specification)
and are the preferred format for behavior that should be reusable across
OpenCode, Codex, Claude Code, and other compatible tools.

Store each skill as a self-contained package:

```text
skills/<skill-name>/SKILL.md
```

The directory name and frontmatter `name` must match. Use lowercase ASCII
letters, numbers, and single hyphens, with a maximum name length of 64
characters.

Every `SKILL.md` starts with:

```yaml
---
name: example-skill
description: Use when a request needs this skill; state what it does and when it applies.
---
```

Write descriptions with concrete trigger terms. Keep scripts, references, and
other required assets inside the same skill directory so the package remains
portable.

The supported Stepwise packages are `sw-setup`, `sw-brainstorm`,
`sw-planner`, `sw-implementer`, and `sw-manager`. Each package
is independently selectable unless an optional agent adds a dependency through
the installation guide. Repository Markdown remains authoritative for plans,
decisions, and cross-session handoffs.

Keep standard frontmatter limited to `name`, `description`, `license`,
`compatibility`, and string-valued `metadata`. Vendor-specific fields may be
ignored by other tools; use them only when needed and document the reduced
portability.

Recommended cross-tool installation locations:

| Tool | Project | User |
| --- | --- | --- |
| OpenCode | `.opencode/skills/` | `~/.config/opencode/skills/` |
| Codex | `.agents/skills/` | `~/.agents/skills/` |
| Claude Code | `.claude/skills/` | `~/.claude/skills/` |

See [INSTALL.md](INSTALL.md) for conflict-safe link and copy instructions.
Start new skills from `templates/skill-template.md`.
