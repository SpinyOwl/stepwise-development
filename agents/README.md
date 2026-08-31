# Agents

Custom agent configuration is not portable across tools. Store each agent as a
package with a canonical prompt and platform adapters:

```text
agents/code-reviewer/
  prompt.md
  opencode.md
  codex.toml
  claude.md
```

## Source Of Truth

`prompt.md` contains only portable behavioral instructions. It is the source of
truth for responsibilities, process, constraints, and output. It can also be
used directly as a system prompt by tools without a repository adapter.

Each adapter must be standalone after installation, so copy the canonical
prompt into its prompt body. Keep behavioral differences out of adapters unless
a platform cannot express the same behavior.

## Durable Markdown Handoffs

Canonical Stepwise roles use authoritative task documents and their Execution
Records as bounded cross-session handoffs. Delegated prompts add only assigned
IDs, repository-relative paths, and immediate deltas. Handoffs do not carry
full conversations, raw evidence, private reasoning, or implementation traces.

Reviewer evidence is input, not an acceptance verdict. Reviewers inspect the
current diff when the host provides a read-only route; otherwise they report
the capability gap and require manager verification of current-diff identity.
This documents portable behavior only and does not claim native-host discovery.

## Adapters

| File | Format | Installation |
| --- | --- | --- |
| `opencode.md` | YAML frontmatter and Markdown | `.opencode/agents/<name>.md` or `~/.config/opencode/agents/<name>.md` |
| `codex.toml` | TOML with `developer_instructions` | `.codex/agents/<name>.toml` or `~/.codex/agents/<name>.toml` |
| `claude.md` | YAML frontmatter and Markdown | `.claude/agents/<name>.md` or `~/.claude/agents/<name>.md` |

See [installation instructions](INSTALL.md) for native definition installation
across OpenCode, Codex, Claude Code, and other tools.
See [uninstallation instructions](UNINSTALL.md) for removing installed agents
and skills without deleting repository source files.

Capability controls are semantic mappings, not exact equivalents:

- OpenCode uses `permission` and `mode`.
- Codex uses settings such as `sandbox_mode`.
- Claude Code uses `tools`, `disallowedTools`, and `permissionMode`.

Default to inherited models so an agent remains usable across accounts and
providers. Pin a model only when its behavior is required and testable.

Start by copying the complete `templates/agent/` directory. Replace
`agent-name`, update the description in every adapter, and copy the completed
canonical prompt into every adapter body.

## Migrated Stepwise Agents

The Stepwise agents migrated from OpenCode use model-neutral role names:

| Original OpenCode agent | Repository package |
| --- | --- |
| `s-manager` | `s-manager` |
| `s-planner` | `s-planner` |
| `s-reviewer` | `s-reviewer` |
| `s-implementer` | `s-implementer` |

All five packages include `prompt.md`, `opencode.md`, `codex.toml`, and
`claude.md`. The Claude adapter uses the documented `.claude/agents/`
destination and is fixture-validated; manually confirm host discovery before
reporting a native installation as working.

Install the Stepwise adapters needed for the intended workflow. The
manager adapter delegates code changes to `s-implementer` and may delegate
independent review to `s-reviewer`. `s-planner` remains available as an
optional standalone planning agent; `s-brainstormer` invokes the
`stepwise-planner` skill in its own context and does not require that agent.

Do not commit API keys, tokens, credentials, or absolute machine-specific
paths in an agent definition.
