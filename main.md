---
project: {{ project_title }}
status: draft            # draft | active | completed | archived
version: 0.1.0
owner: <owner>
created: <YYYY-MM-DD>
updated: <YYYY-MM-DD>
tags: [<tag1>, <tag2>]
---

# {{ project_title }} — global spec

This is the entry point. It is the **global, whole-project** view: it summarizes the
project and points to the spec of each sub-repo. Read `conventions.md` next, then
`environment.md`, then the `main.md` of the sub-repo you work on.

## Goal

`<What the whole project is trying to achieve, in one or two sentences.>`

## Scope

- **In scope:** `<what is built or done across the ecosystem>`
- **Out of scope:** `<what is explicitly not covered>`

## Context

`<Background, motivation, constraints, links to related work.>`

## How this repo works

A project is an **ecosystem of sub-repos** driven by **one specs repo**. Specs live
outside the code repos, so they track the project over time. Code lives in **sibling
repos**; each `subrepos/<name>/main.md` records its remote and local path.

```
PROJECT/
├── <project>-specs/             # this repo
│   ├── main.md                  # GLOBAL view: whole project + its sub-repos
│   ├── architecture.md          # (optional) whole-ecosystem architecture and ADRs
│   ├── conventions.md           # (optional) naming, commits, audits, merge requests
│   ├── environment.md           # (optional) where tasks run: tools, limits
│   ├── audit/                   # (optional) dated audits, from AUDIT-template.md
│   └── subrepos/
│       ├── {{ first_subrepo }}/
│       │   ├── main.md          # spec of this sub-repo
│       │   └── tasks/           # board (README.md) + TASK-*.md, the exchange contracts
│       └── <name-2>/
├── .worktrees/                  # one git worktree per running task
├── {{ first_subrepo }}/         # code of this sub-repo (sibling repo)
└── <name-2>/
```

### Workflow

1. **Project start**: fill in this file and one `subrepos/<name>/main.md` per sub-repo.
2. **Task creation**: the orchestrator writes one `TASK-<NNN>-<slug>.md` per unit of work,
   in the sub-repo it targets, with an explicit `input_contract` and `output_contract`,
   and commits it when delegating. The task spec is the prompt the coder receives.
3. **Execution**: a coder reads the task, works in a git worktree of the target code
   repo, fills in the Result and sets `status`. Transient artifacts go in
   `tasks/.exchange/` (git-ignored).
4. **Tracking**: each sub-repo's `tasks/README.md` is its status board. At review the
   orchestrator commits the task spec again with its Result; what a task taught that
   must outlive it (decision, finding, open point) moves into the sub-repo `main.md`.
   The code repo's commits are the trace of the work.

### What is versioned

- **Versioned:** `main.md`, `architecture.md`, `conventions.md`, `environment.md`,
  `audit/`, `subrepos/*/main.md`, `subrepos/*/tasks/README.md` and the `TASK-*.md` specs.
- **Ignored** (see `.gitignore`): `tasks/.exchange/`, `logs/`, transient agent state.

## Sub-repos

Each sub-repo is a component of the ecosystem, specified under `subrepos/<name>/`.
Keep this table in sync with that folder: one row per sub-repo.

| Sub-repo | Code repo | Role in ecosystem | Spec |
|----------|-----------|-------------------|------|
| `{{ first_subrepo }}` | `<path>` | `<what it does>` | [`subrepos/{{ first_subrepo }}/main.md`](subrepos/{{ first_subrepo }}/main.md) |

## Referenced specs

- `subrepos/{{ first_subrepo }}/main.md`: `<one-line description>`
- `architecture.md`: architecture decisions (whole ecosystem)
- `conventions.md`, `environment.md`: shared rules, where tasks run

## Decisions

Dated, one line each, newest last. Sub-repo decisions go in its own `main.md`.

- `<YYYY-MM-DD>`: `<decision and why>`
