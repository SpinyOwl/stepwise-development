# Workflows

Store vendor-neutral procedures as lowercase, hyphen-separated Markdown files:

```text
workflows/release-check.md
workflows/plan-and-implement.md
```

A workflow should define:

- its purpose and trigger
- required inputs and expected outputs
- ordered execution steps
- decision points and stop conditions
- verification and recovery guidance

Workflows may reference skills by their directory name, but should not depend
on vendor-specific tool names or undocumented machine paths. Describe required
capabilities such as "search files" or "run tests" and put platform mappings in
a clearly labeled compatibility section only when necessary.

Use a workflow for a process that people and different AI systems can follow as
documentation. Use a skill when the process should be discovered, loaded, or
invoked by an Agent Skills-compatible tool.

Start from `templates/workflow-template.md`.
