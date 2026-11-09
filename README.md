# template-multi-repo-specs

A [Copier](https://copier.readthedocs.io) template for the specs repo of a project made of
several code repos. One specs repo holds a global `main.md`, one folder per code repo under
`subrepos/`, and task files that are the contract between an orchestrator and a coder.

It is generic: it contains nothing specific to one language, team or agent tool.

## Use it

```bash
uv run --with copier copier copy https://github.com/SpecsFirst/template-multi-repo-specs my-project
```

Copier asks for the project title and the name of the first sub-repo, then writes the
files below into `my-project/`. Read `main.md`, then `conventions.md`. To pull later
template changes into the project:

```bash
uv run --with copier copier update
```

## What you get

```
my-project/
├── .copier-answers.yml                  your answers, used by `copier update`
├── .gitignore
├── main.md                              global spec: goal, scope, how the repo works, table of sub-repos
├── architecture.md                      whole-ecosystem architecture and ADRs
├── conventions.md                       naming, commit trailers, audits, merge requests
├── environment.md                       where tasks run: tools, limits
├── audit/
│   └── AUDIT-template.md                model for dated audits
└── subrepos/
    └── <first_subrepo>/                 named after your answer
        ├── main.md                      spec of this sub-repo
        └── tasks/
            ├── README.md                task board
            └── TASK-000-template.md     model for one task, the exchange contract
```

`README.md`, `LICENSE` and `copier.yml` stay in the template and are not copied.

## Add a sub-repo

Copy `subrepos/<first_subrepo>/`, rename the copy to the new sub-repo's name, adapt its
`main.md`, then list it in the sub-repos table of the root `main.md`.

## Develop the template

```bash
uv run --with copier copier copy --defaults --vcs-ref HEAD \
  --data project_title="Test" --data first_subrepo=api . <empty dir>
```

Files keep their real names (`_templates_suffix: ""`), so `main.md` is readable on GitHub
and in the gallery. Only `{{ project_title }}` and `{{ first_subrepo }}` are substituted.

## License

CC0-1.0, see `LICENSE`.
