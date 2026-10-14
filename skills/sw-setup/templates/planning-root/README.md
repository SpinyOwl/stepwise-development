# Plans

This is the resolved planning root. Follow the
[folder convention]({{folder-convention-path}}) for the epic, milestone,
phase, and task hierarchy.

## Allowed Values

Omit this section and `Boards` when `Frontmatter: off` is declared. Agents set `labels` and `areas`
only from these lists and leave them empty when a list is empty.

- Labels: <none yet>
- Areas: <none yet>

## Boards

Requires Dataview. Replace
`{{planning-root}}` with the folder path of this planning root from the vault
root.

```dataview
TABLE status, review, priority, assignee, updated
FROM "{{planning-root}}"
WHERE type = "task" AND status != "Completed"
SORT id ASC
```

```dataview
TABLE status, review_note, resolved
FROM "{{planning-root}}"
WHERE type = "task" AND status = "Blocked"
SORT id ASC
```
