# Skill Installation

This is the skill package reference for the canonical
[installation procedure](../INSTALL.md). It does not authorize an independent
installation or update.

Install each selected skill as its complete directory. Do not publish only
SKILL.md; metadata, references, templates, scripts, and assets are part of the
package.

The supported workflow bundle contains:

    sw-setup
    sw-brainstorm
    sw-implementer
    sw-manager
    sw-planner

Skills formerly named `stepwise-*` or `s-setup` are now `sw-*`; follow the migration
section of the canonical [installation procedure](../INSTALL.md#migration-from-former-skill-names).

Each skill is independently selectable. Agent selections may add required
skills through the dependency table in the canonical
[installation procedure](../INSTALL.md). Discoverable skills do
not activate a repository; activation requires the separately approved
instruction change in the canonical
[activation procedure](sw-setup/references/activation.md).

## Native Locations

The host agent determines its own locations as described in section 3 of the
canonical [installation procedure](../INSTALL.md). The
table lists known hosts as a verification aid, not a limit.

| Host | Project | User |
| --- | --- | --- |
| Codex | .agents/skills/<name>/ | <user-home>/.agents/skills/<name>/ |
| OpenCode | .opencode/skills/<name>/ | <user-home>/.config/opencode/skills/<name>/ |
| Claude | .claude/skills/<name>/ | <user-home>/.claude/skills/<name>/ |

Project packages are complete copies. User packages use the approved copy or
managed-link strategy. Do not mix scopes within one selected dependency
closure.

## Package Inspection

Before approval:

1. inspect the complete source entry set, including hidden files and links;
2. inspect every location the selected host may discover;
3. compare an existing destination with the complete source tree;
4. classify it using the canonical procedure's destination states;
5. stop on duplicate names, local modifications, unrelated links, or
   non-directory conflicts.

The frontmatter name must equal the selected directory name. A complete package
comparison, not one matching entry file, establishes an exact copy.

## Publication and Verification

The bounded plan must name each selected package, absolute destination,
strategy, classification, action, and dependency. After exact approval, publish the
complete package atomically where possible and preserve a recoverable backup
for an approved replacement until verification succeeds.

Verify the complete tree, frontmatter name, same-scope dependency closure, and
second no-op inspection. Report unselected packages as preserved. Installation
stops after definition convergence and does not edit repository instructions.
