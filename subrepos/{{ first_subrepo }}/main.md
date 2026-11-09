---
subrepo: {{ first_subrepo }}
parent: {{ project_title }}     # points to the global spec (../../main.md)
status: draft            # draft | active | completed | archived
version: 0.1.0
owner: <owner>
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
tags: [<tag1>, <tag2>]
---

# {{ first_subrepo }} — sub-repo spec

> Spec of **one sub-repo** of the ecosystem: a per-component view. The whole-project
> view is the global `../../main.md`. Keep this file aligned with the global goal.

## Purpose / role in the ecosystem

`<What this sub-repo does within the larger project, in one or two sentences.>`

## Goal

`<What this sub-repo is trying to achieve.>`

## Scope

- **In scope:** `<what is built or done here>`
- **Out of scope:** `<what is handled elsewhere, e.g. in other sub-repos>`

## Interfaces with other sub-repos

`<How this sub-repo connects to the rest of the ecosystem: APIs, contracts, data, dependencies.>`

## Decisions

Choices specific to this sub-repo, one line each, with date. Project-wide decisions go in
`../../architecture.md`.

- `<YYYY-MM-DD>`: `<decision and why>`

## Code location

- **Remote:** `<git URL>`
- **Local path:** `<project>/{{ first_subrepo }}` (sibling of the specs repo)
- **Default branch:** `<main>`

Tasks in `tasks/` target that repo.

## Dev commands

```bash
<setup>      # e.g. uv sync
<test>       # e.g. uv run pytest
<run/lint>
```

## Referenced specs

- `tasks/README.md`: task board
- `tasks/TASK-001-<slug>.md`: `<one-line description>`
- `../../main.md`: the global (ecosystem) spec
