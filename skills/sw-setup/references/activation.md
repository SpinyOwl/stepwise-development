# Activate Stepwise in a Repository

This is the canonical activation and deactivation procedure. Activation is an
explicit repository opt-in. Installing or discovering skills and optional
custom agents does not activate Stepwise. A repository without an approved
instruction continues to use its existing workflow.

Use this sequence:

    inspect -> plan -> approve -> apply -> verify

## 1. Inspect Before Editing

Identify the repository root and inspect its existing instruction files, host
equivalents, documentation index, and candidate planning directories. Never
overwrite an existing instruction file or create a parallel instruction system
silently.

Locate the shared convention in this order and use the first existing file:

1. `<documentation-root>/folder-structure.md` (`docs/` unless the instruction
   file declares another documentation root);
2. `<installed-s-setup>/references/folder-structure.md`;
3. `<source-root>/conventions/folder-structure.md`.

If none exists, stop. Resolve the planning root once through that convention,
which uses a declared planning root before scanning candidates. Preserve the
existing root and do not create a second planning tree.

## 2. Plan the Repository Instruction

Show the exact instruction to add or integrate into `AGENTS.md` or the selected
host's equivalent. A suitable baseline is:

> For ambiguous or high-impact software changes, use sw-brainstorm
> before planning. For approved Stepwise plan tasks, use
> sw-implementer; use sw-manager only when delegation and
> dependency coordination are requested. Well-scoped changes and routine Git
> or file operations may proceed directly.

Also offer to record Stepwise declarations from the convention's Repository
Declarations section in the same instruction file, under a `Stepwise` heading.
Record only values the user states or the repository already uses, such as a
documentation root or index, planning root, `Profile`, `Decision log`,
`Output markers`, or `Frontmatter`. Frontmatter is on by default; when
`Frontmatter: off` is approved, omit the `Allowed Values` and `Boards` sections
of the planning-root template. Omit any setting that keeps its default.

Offer **integrate only** mode when the repository already has its own
documentation index. In this mode, add only the workflow rule and the
approved declarations to the instruction file, skip section 3, and create no
baseline documentation or planning files. Always record at least the
`Documentation index` and the `Planning root` (declared or resolved) as
declarations so the skills find the repository's own layout; create a missing
planning directory only when the first plan is written.

Optional custom agents must not become mandatory merely because they are
installed. Integrate with existing vocabulary and precedence. If an existing
instruction conflicts with the proposed workflow, show the conflict and ask
which rule should govern.

The plan must list every file to create or edit, the exact instruction diff,
the resolved planning root, preserved files, and verification steps. State
that no installation destination or unrelated repository file is included.
Obtain approval for this exact plan before applying it.

## 3. Optional Missing Baseline

Skip this section in integrate-only mode. When the repository lacks an
instruction or documentation baseline, propose only the missing paths (using a
declared documentation root in place of `docs/`):

    AGENTS.md
    docs/
      README.md
      folder-structure.md
    <resolved-planning-root>/
      README.md

Use assets under the installed `sw-setup/templates/` directory. Copy the
resolved convention to `docs/folder-structure.md` only when absent. Replace
placeholders with paths relative to the target file. Create only approved
missing files and preserve every existing file.

Create optional documentation sections only when separately requested. Every
created section needs an index. The documentation index must link only to
sections that exist and identify the single resolved planning root.

## 4. Apply and Verify

Apply only the approved instruction and baseline changes. Then verify:

- the repository instruction says exactly when Stepwise skills apply;
- existing instructions remain coherent and unrelated content is preserved;
- the documentation index, when created by this activation, links to the convention and the single
  planning-root index;
- no second planning root was created;
- no installation destination or unrelated host configuration changed.

Report every created, integrated, preserved, skipped, and ambiguous path.

## Deactivation and Uninstallation

Deactivation is another repository-owned instruction change. Remove or revise
only the approved Stepwise workflow rule and preserve unrelated instructions
and documentation.

Uninstallation removes separately verified installed definitions. It does not
deactivate or delete repository-owned instructions automatically. Follow the
[installation procedure](../../../INSTALL.md).
