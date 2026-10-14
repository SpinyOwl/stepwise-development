---
name: sw-setup
description: Guide an explicitly requested Stepwise repository activation or deactivation through the package's canonical activation procedure.
---

# Stepwise Setup Guide Entry Point

This skill configures one repository for Stepwise through the package-local
canonical [activation procedure](references/activation.md). It does not install
or update Stepwise definitions; that is the separate procedure in the
repository's root `INSTALL.md`, which works before any skill is installed.
Neither operation implies the other.

## Operating Contract

1. Read the canonical activation procedure completely before proposing any
   action.
2. Inspect the repository's existing instructions, documentation index, and
   planning directories, and any existing conflicts, before planning.
3. Show every file to create or edit, the exact instruction diff, the resolved
   planning root, preserved state, recovery, and verification in one bounded
   plan.
4. Obtain approval for that exact plan. Reinspect and re-plan whenever the
   repository or instruction state changes.
5. Apply only approved repository-instruction and baseline-file changes.
6. Verify the exact repository opt-in, then run a second no-op inspection.
7. Stop on ambiguous ownership, conflicting instruction precedence, or a
   missing shared convention.

Activation changes only approved repository instructions (including declared
Stepwise settings) and optional missing documentation baseline files, or only
the instructions in integrate-only mode; it never publishes, updates, or
removes installed definitions.
