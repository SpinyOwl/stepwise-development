# Install or Update Stepwise

This is the canonical installation and update procedure for Stepwise. It
publishes only explicitly selected skill packages and optional custom-agent
definitions to native host locations. Installation makes definitions
discoverable; it does not activate Stepwise in any repository. Repository
opt-in is a separate, explicitly approved instruction change described in the
[activation procedure](skills/sw-setup/references/activation.md).

This file is the only installation procedure. It is read as a document, not
invoked as a skill, because it must work before any Stepwise skill exists in
the host. The `sw-setup` skill covers repository activation only.

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
- the host: the agent system running this procedure, identified from its own
  runtime, or the one the user names;
- the directly selected skills and optional agents;
- copy for project definitions, or one explicitly selected copy or link
  strategy for user definitions.

At least one component must be selected. Resolve dependencies before inspecting
destinations:

| Selected agent | Required definitions in the same scope |
| --- | --- |
| s-brainstormer | sw-brainstorm and sw-planner skills |
| s-implementer | sw-implementer skill |
| s-manager | sw-manager and sw-implementer skills, plus the s-implementer agent |
| s-planner | sw-planner skill |
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
- an agent is the shipped adapter whose format matches the host
  (`agents/<name>/codex.toml`, `opencode.md`, or `claude.md`); a host with no
  shipped adapter follows the fallback in section 3.

Missing or ambiguous source content is a conflict and stops the operation.

## 3. Resolve Native Destinations

This procedure is written for the host agent. Determine the native locations of
the system you run in; do not assume a fixed list of hosts.

1. Identify the host (name, and version when available) from your own runtime.
   When you are not sure, ask the user.
2. For the selected scope, determine the host's native skill directory and, when
   agents are selected, its agent directory. Accept as evidence, in this order:
   the host's own documentation or configuration; a definition the host already
   discovers (not merely a file on disk) in a sibling location; a row for the
   same host in the known-host table below.
3. Classify each resolved path as confirmed (at least one piece of evidence,
   named in the plan) or unconfirmed (memory or analogy only). An unconfirmed
   path is marked as such in the plan and needs its own explicit approval.
4. When you cannot determine a skill directory, stop and ask for it. Do not
   publish into a directory only because it exists.

Known hosts are a verification aid, not a limit; the table is neither complete
nor authoritative over the host's own documentation:

| Component | Project destination | User destination |
| --- | --- | --- |
| Codex skills | `.agents/skills/<name>/` | `<user-home>/.agents/skills/<name>/` |
| OpenCode skills | `.opencode/skills/<name>/` | `<user-home>/.config/opencode/skills/<name>/` |
| Claude skills | `.claude/skills/<name>/` | `<user-home>/.claude/skills/<name>/` |
| Codex agent | `.codex/agents/<name>.toml` | `<user-home>/.codex/agents/<name>.toml` |
| OpenCode agent | `.opencode/agents/<name>.md` | `<user-home>/.config/opencode/agents/<name>.md` |
| Claude agent | `.claude/agents/<name>.md` | `<user-home>/.claude/agents/<name>.md` |

Resolve all destinations to absolute paths for the plan. A selected definition
and all of its dependencies use the same scope.

A skill is a portable `SKILL.md` package and installs to any host once its path
is resolved. Agents are host-specific. For a host with no shipped adapter,
install the skills only, and offer to draft an adapter from the package's
`prompt.md` following the adapter contract in
[agents/README.md](agents/README.md). Show the full draft in the plan
as a draft that is not verified in a live host, install it only with explicit
approval of that file, and never derive it from another host's adapter. Project definitions are
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
- every absolute source and destination, each with its path evidence and
  confirmed or unconfirmed status;
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
- repository instructions were not added or modified;
- the host discovers each installed definition, checked with the host's own
  listing and after a restart when the host requires one; when discovery cannot
  be confirmed, report the item as installed with discovery unverified.

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
[activation procedure](skills/sw-setup/references/activation.md).

## Migration from Former Skill Names

The skills `stepwise-brainstorm`, `stepwise-planner`, `stepwise-implementer`,
and `stepwise-manager` were renamed `sw-brainstorm`, `sw-planner`,
`sw-implementer`, and `sw-manager`, and `s-setup` was renamed `sw-setup`. The new names are separate packages, so an
update installs them beside any old ones; it never renames or removes an old
package. Migrate in this order:

1. Inspect every native location for installed `stepwise-*` and `s-setup` skill directories
   and for repository instructions that name them (`AGENTS.md`, host
   equivalents, and documentation such as `docs/guides/`).
2. Install the selected `sw-*` skills through sections 1-7 above.
3. Plan the removal of each old `stepwise-*` or `s-setup` package that is an exact copy or
   exact link of a former release, following Removal. Treat a modified old
   package as a conflict and leave it for the user.
4. Plan an instruction change that replaces the old names with the new ones in
   the repository instructions, and obtain approval separately, as for
   activation.
5. Verify that each old name is gone from the discovered skills and from the
   repository instructions, that no name is discovered twice, and that a second
   inspection is a no-op.

Custom agents (`s-planner`, `s-manager`, and others) keep their names; update
them in the same plan so they reference the `sw-*` skills.
