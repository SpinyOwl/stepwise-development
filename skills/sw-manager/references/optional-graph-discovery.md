# Optional Graph Discovery Policy

This policy is the single normative contract for optional Graphify-assisted
discovery. Its canonical source identity is
`skills/sw-implementer/references/optional-graph-discovery.md`.
Byte-identical package projections under `sw-planner` and
`sw-manager` exist only so each independently installed workflow package
remains self-contained; the dependency-free repository checker rejects drift.

Graphify is an optional, query-first discovery accelerator. Markdown plans and
task records, current repository files, Git, and current verification evidence
remain authoritative. A workflow must operate completely when Graphify is
absent or unusable.

## Detection and No-Mutation Boundary

Use Graphify only when an existing command or configured integration is already
available and its use is permitted. Detection is read-only. Never install,
configure, update, repair, authenticate, start, or host Graphify, and never
create or refresh an index as a workflow side effect.

When availability or permission is uncertain, skip Graphify and use direct
repository inspection. Do not delay the workflow merely to obtain graph access.

## Privacy and Backend Gate

Before a query, establish the backend location, repository scope, data owner,
access boundary, and whether query content or results leave the local machine.
Do not submit credentials, secrets, raw conversations, private reasoning,
unapproved repository content, or machine-specific paths. If backend,
ownership, privacy, tenant, or data-egress status is unknown or unacceptable,
skip the query and use direct inspection.

Use the narrowest query that can locate candidate files or relationships.
Graph output is generated, disposable discovery data. Do not commit it, treat it
as workflow state, or paste raw output into a plan, task record, review, or
handoff.

## Freshness and Completeness Gate

Before relying on a result even as a hint, establish the indexed repository
identity, branch or revision, last refresh point, and relevant path coverage.
Treat the index as unusable when any of those facts is missing, stale,
mismatched, ambiguous, or incomplete.

An unusable index requires direct file and repository search. Partial results
must not be interpreted as proof that a file, dependency, change, risk, or
relationship is absent.

## Source Verification and Fallback

Treat every graph result as a candidate locator only. Verify each material claim
against the applicable authoritative Markdown document, current repository
file, Git state, or current verification evidence before using it. When graph
output conflicts with a direct source, the direct source wins.

The deterministic fallback is direct repository inspection with file listing,
text search, targeted file reads, Git inspection, and focused verification as
appropriate. Prefer `rg --files` and `rg` when available, with a native
read-only equivalent otherwise. Use the fallback whenever Graphify is absent,
unavailable, unpermitted, stale, mismatched, ambiguous, incomplete, or unable to
answer the bounded discovery question.

## Workflow Authority Gates

Graphify output cannot:

- change a task or plan status, checkbox, scope, dependency, or next action;
- supply Verified acceptance evidence or satisfy a completion gate;
- resolve or approve a material decision;
- resolve a review finding, establish current-diff identity, or satisfy
  reviewer independence;
- satisfy a prerequisite, authorize mutation, or replace user or manager
  approval.

Only the applicable authoritative source and required evidence or decision can
satisfy those gates. Record the verified source, not graph output, in the
Execution Record.
