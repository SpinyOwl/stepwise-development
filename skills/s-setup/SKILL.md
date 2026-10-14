---
name: s-setup
description: Guide an explicitly requested Stepwise definition installation, update, or repository activation through the package's canonical procedures.
---

# Stepwise Setup Guide Entry Point

This skill coordinates two separate operations:

- definition installation or update through the package-local canonical
  [installation procedure](references/installation.md);
- explicit repository activation through the package-local canonical
  [activation procedure](references/activation.md).

Neither operation implies the other.

## Operating Contract

1. Read the applicable package-local canonical procedure completely before
   proposing any action.
2. Record selected scope, host, skills, optional agents, and copy/link strategy
   for installation; resolve the exact dependency closure.
3. Inspect sources, destinations, existing instructions, and conflicts before
   planning.
4. Show absolute paths, classifications, exact actions, preserved state,
   recovery, and verification in one bounded plan.
5. Obtain approval for that exact plan. Reinspect and re-plan whenever source,
   destination, selection, strategy, or instruction state changes.
6. Apply only approved filesystem and repository-instruction changes, using
   complete packages and recoverable publication.
7. Verify exact definition convergence or the exact repository opt-in, then run
   a second no-op inspection.
8. Stop on ambiguous ownership, local modifications, duplicate discovered
   names, conflicting instruction precedence, or missing dependencies.

Installation stops after publishing selected definitions and never changes
repository instructions. Activation changes only approved repository
instructions (including declared Stepwise settings) and optional missing
documentation baseline files, or only the instructions in integrate-only mode; it never
publishes or updates definitions.
