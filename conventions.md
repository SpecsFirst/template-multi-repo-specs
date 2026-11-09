# Conventions

> Shared conventions so all tasks and agents stay consistent across the project. Edit to
> match the project's reality.

## Naming

- Task files: `TASK-<NNN>-<slug>.md` (zero-padded, hyphenated slug).
- Branches: `feature/<slug>`, `fix/<slug>`, `task/<NNN>-<slug>`.
- Worktrees: `<project>/.worktrees/<repo>-<slug>` (named by intent).
- Commits: follow the target repo's existing style (default: conventional `feat:`,
  `fix:`, `docs:`, `chore:`).
- Agent commits end with **3 trailers, plus `Refs` when the task has an issue**, stamped
  by `<agent tooling>`, never typed by the agent:

  | Trailer | Value | Always? |
  |---------|-------|---------|
  | `Refs` | `#N` (same project) or `group/project#N`, from the task's `issue:` | only with an issue |
  | `Task` | `https://<forge>/<namespace>/<project>-specs/-/blob/<short sha>/subrepos/<subrepo>/tasks/TASK-<NNN>-<slug>.md`: the prompt, at the commit that delegated it | coder commits (none on the orchestrator's own spec commits) |
  | `Co-authored-by` | `<agent identity> <address>`: the vendor's official noreply address when it exists, else `noreply@<provider>.invalid` (RFC 2606: never registrable) | yes |
  | `Assisted-by` | `<agent tooling URL>/-/commit/<short sha> (<tool> <version>)`, `; tooling dirty` inside the parentheses when it has local changes | yes |

  `Task` and `Assisted-by` are full https URLs built from the repos' `origin` (GitLab
  `/-/blob/`, `/-/commit/`; GitHub `/blob/`, `/commit/`), clickable on any forge. Not
  `<namespace>/<project>@<sha>` references: GitLab links them only within its own instance
  and GitHub reads them as GitHub repos. Check:
  `git log --format='%h %(trailers:key=Task,valueonly,separator=)'`.
- Task specs are versioned. The orchestrator commits a spec when delegating
  (`task(<subrepo>): TASK-<NNN> <slug>`) and again after review
  (`task(<subrepo>): TASK-<NNN> <status>`); both messages carry `Refs: <group/project>#N`
  when the spec has an `issue:`, so the issue timeline links the spec, the code commits
  and the MR without any API call.

## Audits

- File: `audit/<YYYY-MM-DD>-<slug>.md`, from `audit/AUDIT-template.md`.
- An audit is a dated snapshot of the revisions listed in `audited`
  (`<namespace>/<project>@<short sha>`). Once `final`, it is never edited to track later
  changes: write a new audit instead.
- An agent audit carries its identity in the `Co-authored-by` and `Assisted-by` fields
  (remove them on a human audit), with the same values as its commit trailers, produced
  by `<agent tooling>`, never typed by the agent.
- Finding references use a prefix and a number: **B** bug, **D** debt, **S** security,
  **P** performance, plus project-specific prefixes declared here: _none yet_ (`B1`,
  `D2`…). They are stable: backlog lines, tasks and ADRs cite them, and the audit's
  **Follow-up** table records where each one went.

## Process

- One logical change per commit.
- Run linters / typecheck / tests before marking a task done.
- A task that changes runtime behaviour lists the check the **operator** runs (e.g. a
  deploy); coders never deploy.
- Security-relevant decisions go to `architecture.md` as an ADR.
- Never force-push, never rewrite shared history, never commit secrets.

## Merge requests

The agent opens the MR (GitHub: PR), the operator merges. The orchestrator writes it from
the task spec and its Result, never the coder.

- **Granularity:** the orchestrator picks it so each MR reads in one sitting and tells
  one story. Issue: tasks merged by the orchestrator into `issue/<N>-<slug>`, one MR
  `Closes #N`, or one MR per coherent step (`Refs #N`, the last `Closes #N`). No issue:
  one MR per task. A `sprint` / `dev` branch only when the operator asks for it.
- **Title:** `TASK-<NNN>: <task title>`; issue MR `#N: <issue title>` (no task: the
  commit style).
- **Target:** `main`; a stacked MR targets the previous task branch.
- **Merge:** merge commit only (no squash, no rebase from the UI: trailers and task
  history must survive); source branch deleted.
- **Open on GitLab:** `git push -o merge_request.create -o merge_request.target=main
  -o merge_request.title=… -o merge_request.description=…` (no token). Push options
  cannot carry newlines: send the description as single-line HTML (`<h2>`, `<ul>`,
  `<p>`), trailers in a `<ul>` list (GitLab drops `<br>`), never in `<pre>` or code
  (GitLab does not link references inside code).
- **Open on GitHub:** push the branch, then `gh pr create --base <target> --head
  <branch> --title … --body-file <file>`. The body is the Markdown below as is, trailers
  as plain lines (GitHub breaks single newlines), with a blank line before `---`. Merge
  commits only and head-branch deletion are repo settings there, not per-PR options.
- **Description:**

  ```markdown
  ## Goal
  <one sentence, from the task>

  ## Changes
  - <what changes>

  ## Checks
  - [x] <contract checks, with result>

  ## After merge (operator)
  <what the operator runs or checks, e.g. a deploy command, or "no change expected">

  ## Notes
  <risks, scope deviations, orchestrator fixes; omit if none>

  ---
  <trailer block of the task: Refs / Task / Co-authored-by / Assisted-by>
  ```

  `Refs: #N` only with an issue; `Closes #N` only on the issue's last MR.

## Definitions

| Term | Meaning |
|------|---------|
| spec | A durable, versioned description of intent or a task. |
| contract | The explicit input/output of a task (see task template). |
| operator | The human owning the infrastructure, secrets and forge accounts. |
| agent | Any LLM-driven process working on the project. |

## Tool-agnostic note

These specs stay independent of any specific model or agent tool. Agent behavior, roles
and tooling live in `<agent tooling>`; this repository holds only the *what* and *why*.
