# Tasks

Index and global status of all tasks. Update this as tasks move through their lifecycle.
Each task is one file: `TASK-<NNN>-<slug>.md`, from `TASK-000-template.md`.

## Status legend

- **open**: defined, not started
- **in_progress**: being executed
- **blocked**: stopped by the executor (reason in the task's Result)
- **done**: completed and contract satisfied
- **needs_review**: done, awaiting human/orchestrator review
- **rejected**: closed without completion (reason in the task)

## Board

| Task | Title | Status | Depends on |
|------|-------|--------|------------|
| TASK-001 | `<title>` | open | none |

## Backlog

Planned tasks, one line each. Write the full `TASK-*.md` only when the task is about to
start, so its contract uses the actual outputs of its dependencies.

- `<title>` (after TASK-001)

## Transient exchange

Intermediate exchange between orchestrator and coders goes in `tasks/.exchange/`:
**git-ignored**, not part of the durable record. The `TASK-*.md` files are versioned
specs; what a task teaches (decisions, findings) goes into `../main.md`.
