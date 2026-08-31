# Install or Update Stepwise

This is the canonical installation and update procedure for Stepwise. It
publishes only explicitly selected skill packages and optional custom-agent
definitions to native host locations. Installation makes definitions
discoverable; it does not activate Stepwise in any repository. Repository
opt-in is a separate, explicitly approved instruction change described in the
[activation procedure](activation.md).

Every installation or update follows one sequence:

    inspect -> classify -> plan -> approve -> apply -> verify

Do not infer approval from an earlier installation. Do not install unrelated
bundled components, alter repository instructions, or create product state as
an installation side effect.

## 1. Record the Desired State

Before changing anything, record:

- the absolute Stepwise source checkout and expected remote and branch;
- exactly one scope, project or user, and the target project when project scope
  is selected;
- one host: codex, opencode, or claude;
- the directly selected skills and optional agents;
- copy for project definitions, or one explicitly selected copy or link
  strategy for user definitions.

At least one component must be selected. Resolve dependencies before inspecting
destinations:

| Selected agent | Required definitions in the same scope |
| --- | --- |
| s-brainstormer | stepwise-brainstorm and stepwise-planner skills |
| s-implementer | stepwise-implementer skill |
| s-manager | stepwise-manager and stepwise-implementer skills, plus the s-implementer agent |
| s-planner | stepwise-planner skill |
| s-reviewer | None |

Show direct selections and resolved dependencies separately. A dependency is
part of the proposed selection and does not authorize replacement of an
existing destination.

## 2. Inspect the Source

The credential-neutral remote is
https://github.com/SpinyOwl/stepwise-development.git.

If the approved source directory is absent, propose the exact clone target and
obtain approval before creating it. If it exists, inspect its remote, current
branch, worktree state, and current revision. Stop when the directory is not
the expected checkout or has unresolved work. Fetching, pulling, switching a
branch, or changing a remote is a separate source change and needs separate
approval.

Validate each selected source:

- a skill is the complete `skills/<name>/` directory, including metadata,
  references, templates, scripts, and assets;
- an agent is the selected host adapter:
  `agents/<name>/codex.toml`, `opencode.md`, or `claude.md`.

Missing or ambiguous source content is a conflict and stops the operation.

## 3. Resolve Native Destinations

Use only the selected host's native locations:

| Component | Project destination | User destination |
| --- | --- | --- |
| Codex skills | `.agents/skills/<name>/` | `<user-home>/.agents/skills/<name>/` |
| OpenCode skills | `.opencode/skills/<name>/` | `<user-home>/.config/opencode/skills/<name>/` |
| Claude skills | `.claude/skills/<name>/` | `<user-home>/.claude/skills/<name>/` |
| Codex agent | `.codex/agents/<name>.toml` | `<user-home>/.codex/agents/<name>.toml` |
| OpenCode agent | `.opencode/agents/<name>.md` | `<user-home>/.config/opencode/agents/<name>.md` |
| Claude agent | `.claude/agents/<name>.md` | `<user-home>/.claude/agents/<name>.md` |

Resolve all destinations to absolute paths for the plan. A selected definition
and all of its dependencies use the same scope. Project definitions are
complete copies suitable for version control. User definitions may be managed
links or approved copies.

## 4. Classify Every Destination

Inspect every selected destination and any alternate location the host may
discover. Classify each as exactly one of:

- missing;
- exact-link-to-source;
- exact-copy-of-source;
- different-copy;
- different-link;
- non-definition-conflict.

Compare complete directory trees for skills and exact bytes for agent files.
Include hidden entries and link targets. Preserve local metadata and
customizations unless replacing them is explicitly approved. A different copy,
different link, duplicate discovered name, or non-definition entry is a
conflict; do not overwrite, merge, delete, or retarget it automatically.

## 5. Show One Bounded Plan

Before mutation, show:

- source identity and any separately approved source change;
- selected scope, host, strategy, direct selections, and dependency closure;
- every absolute source and destination;
- every destination classification;
- exact creates, copies, replacements, links, or no-ops;
- conflicts and the recovery path;
- verification steps;
- an explicit statement that repository activation is not included.

Ask for approval of this exact plan. Any source revision, destination state,
selection, scope, strategy, or action change invalidates approval and requires
fresh inspection and a new plan.

## 6. Apply Safely

Apply only approved actions. Publish complete packages atomically where the
host permits it: prepare a sibling temporary entry, verify it, then replace the
approved destination in one bounded operation. For an approved replacement,
retain a recoverable sibling backup until verification succeeds. Never leave a
partially copied package as the discoverable definition.

An exact existing link or copy is a no-op. Do not touch unselected definitions,
repository instructions, documentation, or unrelated host configuration.

## 7. Verify and Reinspect

After applying, verify:

- every selected skill is a complete package and its frontmatter name matches
  the selected name;
- every selected agent is the correct native adapter;
- all required dependencies exist in the same scope;
- copied content exactly matches its approved source, or links resolve to that
  source;
- no unselected destination changed;
- repository instructions were not added or modified.

Repeat the destination inspection. It must now classify each selected item as
an exact copy or exact link and yield no further action. Report created,
updated, preserved, skipped, and conflicting paths separately. Remove a
temporary backup only after successful verification and only when its exact
path was included in the approved plan.

## Removal

Removal uses the same inspect, classify, plan, approve, apply, and verify
sequence. Remove only a verified installed definition selected by the user.
Uninstallation never removes repository-owned instructions or documentation;
deactivation is a separate repository instruction change governed by the
[activation procedure](activation.md).
