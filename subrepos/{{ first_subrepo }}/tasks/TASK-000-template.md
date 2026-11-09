---
id: TASK-000
title: <short task title>
status: open             # open | in_progress | blocked | done | needs_review | rejected
parent: main             # or subrepos/<name>/main.md for sub-repo tasks
subrepo: <name>          # the sub-repo this task targets (empty for cross-cutting)
issue:                   # optional: group/project#N
assignee: <orchestrator | coder | human>
model: <set by the orchestrator>
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
---

# TASK-000: <short task title>

> One task = one goal. This file is the **exchange contract** between the
> orchestrator (writer) and the atomic coder (reader/executor). Do not mix
> several goals in one task.

## Goal

The single thing this task must achieve, in one sentence.

## Context

Background needed to do the task. Point to `main.md`, other tasks, or files.

## Input contract

Exactly what the executor **receives**. What is available (repo path, files,
data, previous task outputs).

```yaml
repo: <path>
worktree: <path>           # created by the orchestrator; the coder works only here
branch: task/<NNN>-<slug>
depends_on: [TASK-001, TASK-002]
inputs:
  - <file or data the executor starts from>
scope:
  write: [<paths the coder may change>]
  read_only: [<paths the coder may read but not change>]
checks:
  - <command that must pass, e.g. the test command>
```

## Output contract

Exactly what the executor must **produce and return**, nothing more. Explicit
enough that a stateless coder can complete without asking.

```yaml
artifacts:
  - <file(s) to produce>
contract:
  - <what must be true when done>
returns: <the artifact to hand back>
```

## Steps (suggested)

1. ...
2. ...
3. ...

## Attempts

> Filled in by the orchestrator, one line per (re)launch: date, outcome,
> correction given. A relaunch only says "read the task"; the spec stays the
> whole contract.

- <YYYY-MM-DD> #1: <outcome> / <correction for next attempt>

## Result

> Filled in by the executor, **always**, even on failure: set `status` to
> `done` or `blocked` before exiting. Do not delete the contract above.

### Outcome

<what was actually done>

### Artifacts produced

- <path to produced artifacts>

### Opinion / concerns

<honest senior-coder assessment, if any>

### CLARIFICATION NEEDED

<only if blocked: the precise question that must be answered>
