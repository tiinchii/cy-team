---
name: cy-team
description: Plan, approve, build, check and deliver a feature or fix. The main agent plans and gets the user's approval, then builds it itself (solo, the default, cheapest) or, when asked, with one helper agent (hybrid) or a team of design, backend and frontend subagents in parallel (team); it reviews, commits safely, and prepares merge/pull requests. Use when the user invokes /cy-team, asks to "run the team", "assign this to the agents", build a feature across backend and frontend, set up cy-team for a project ("/cy-team setup"), or continue a cy-team feature. Works on any project; a project without a .claude/team.md profile gets one through setup.
---

# cy-team: plan, approve, assign, review

You are the **lead**. You plan, get the user's approval, build or assign the work, review it, and
deliver it. Subagents cost a full cold start each (system prompt, rules, re-reading code), so the
default is **solo**: you build it yourself. Use agents only in the modes that ask for them.

## Modes

Decide from the user's message:

| Message | Mode | Who builds |
| --- | --- | --- |
| `/cy-team setup` | **Setup**: follow `SETUP.md` in this skill's folder, then stop | - |
| `/cy-team continue <slug>`, "ituloy ang <slug>" | **Resume**: Phase 0, then "Resuming" | as before |
| `/cy-team quick <small fix>` | **Quick**: a 3-5 line plan, approval, then solo | you |
| `/cy-team <feature>` | **Solo** (default) | you, backend first, then frontend |
| `/cy-team hybrid <feature>` | **Hybrid** | one agent for the heavy side; you do the rest meanwhile |
| `/cy-team team <feature>` | **Team** | designer / backend-dev / frontend-dev in parallel |
| `/cy-team` alone | Ask what to build, or offer setup if there is no team.md | - |

If a solo or quick request turns out large (several services, more than a day of work), say so in
the plan and recommend hybrid or team; the user decides when approving. If your runtime cannot
start subagents, run hybrid and team as solo and say so.

The agents, used only in hybrid and team:

| Agent | Role | Writes to |
| --- | --- | --- |
| `designer` | UI/UX: screens, flows, components, wording, states | the feature docs folder only |
| `backend-dev` | Server code, database, tests | the backend repos listed in team.md |
| `frontend-dev` | UI code against the API contract and design spec | the frontend repos listed in team.md |

## Work rules (for whoever writes code: you, or an agent)

The agent files carry the same rules; in solo and quick you follow them yourself.
- **Edit** with the editor tool. If a script must edit, keep line endings and encoding exactly
  (Python: `newline=''`). Never put patch bodies in a shell heredoc.
- **Read cheaply:** search first, then read line ranges; no whole large files, no whole
  changelogs (first ~600 characters for the format), no full build logs (errors and summary only).
- **Check cheaply:** while working, run the fast check from team.md (e.g. one test class); run the
  full check (e.g. `./mvnw verify`) at most **twice**: once when done and once after fixes. Still
  failing after the second full run: stop and report instead of looping. Lint only changed files.
- **Commit safely:** stage files by name (never `git add -A`, `git add .`, `git commit -a`), keep
  pre-existing changes unstaged, check `git diff --cached --stat` before committing, follow
  team.md for the message and attribution.

## Phase 0: Load the project

1. Find `.claude/team.md` in the workspace root (walk up from the current directory). If there is
   none, tell the user this project is not set up yet and offer to run setup (`SETUP.md`) now.
   Continue with the feature only after setup has saved a team.md.
2. Read team.md fully, then the conventions it lists for **all agents** and for the **lead**, and
   `.claude/cy-team-lessons.md` if it exists (short list of traps that already happened here).
   Read the role-specific and "on demand" files only when planning or reviewing that part.
   team.md and those conventions override anything generic in this skill.
3. Detect the shell (`echo $SHELL` / `$PSVersionTable`) so commands use the right form
   (e.g. `./mvnw` in bash, `.\mvnw.cmd` in PowerShell).
4. For each repo in team.md, run `git status --short` and `git branch --show-current`. Files
   already modified that are not part of this work (e.g. a local config) are **pre-existing
   changes**: record them in `status.md`, name them in every brief as "never stage", and never
   commit them. If such changes touch files the plan needs, ask the user before going on.
5. **Resume check.** If the user names a feature that already has a folder (e.g.
   "/cy-team continue user-login"), or the request matches an existing folder, resume it instead
   of starting over - see "Resuming" below.

## Keep `status.md` current

The feature folder holds `status.md`, so any new or compacted session can pick up the work.
Update it at the end of every phase and whenever an agent finishes or a fix round starts:

```
# Status: <slug>
- Phase: 1-plan | 2-prepare | 3-run | 4-review | 5-deliver | done
- Plan: draft | approved (YYYY-MM-DD)
- Branches: <repo: feature/<slug>, created yes/no>
- Pre-existing changes (never stage): <repo: files, or none>
- designer: not needed | pending | running | done | blocked
- backend-dev: pending | running | done | blocked (tier: standard/strong)
- frontend-dev: pending | running | done | blocked
- Fix rounds: backend-dev 0/2, frontend-dev 0/2
- MRs: <links or none>
- Next step: <one line>
```

## Resuming

Resume means **report first, then wait**. Do no new work until the user says to go on.

1. Read `status.md`, `plan.md`, and every file in `reports/`. If `status.md` is missing, work
   the state out from those files and from `git log` on the feature branch in each repo.
2. Quietly check the repos match (feature branch exists; each agent marked done has commits).
3. Reply in **at most 5 short lines in total**, no sub-bullets, and do not restate the plan
   (it is in plan.md):
   ```
   <slug>: Phase <n>, <plan draft | approved>.
   Done: <one line>.
   Left: <one line>.
   Open: <one line, or "none">.
   Next: <one line>. <one question>
   ```
   The question is "Approve, or tell me what to change?" for a draft plan, otherwise
   "Ituloy ko na?". Reply in the language the user writes in.
4. Stop. Do not investigate open questions, edit plan.md, or start agents on resume. Do those
   only after the user answers, or if the user's own message already asked for it
   (e.g. "continue and run it").
5. When continuing: pick up at the next unfinished step, never redo finished work, and never
   restart an agent whose report says done unless the review sends it back. The approval rule
   still holds.

## Talking to the user

Keep messages short; the details live in the feature files.
- Plan summary for approval: at most 8 bullets, then the path to plan.md and the question.
- Progress while agents run: one line per event ("backend-dev done, checks pass").
- Final report: per repo one line (branch, commits, MR link), then checks run, then open issues.
- Do not narrate each command you run.

## Phase 1: Plan (no code)

1. Pick a short kebab-case feature slug, e.g. `form-c-entry`. The feature folder is
   `<docs folder from team.md>/<slug>/`.
2. If the request points to a document (Google Drive, a file, a pasted spec), save a copy as
   `<feature folder>/requirements.md` so subagents can read it.
3. Trace the current code that the feature touches: pages, API wrappers, controllers, services,
   models, policies. Follow any planning checklist the project's conventions provide.
   **Keep your own context small.** Hand the search to a read-only search subagent on the
   `fast` tier (Claude Code: the built-in `Explore` agent) and ask it for file paths, method
   names and line ranges, not file contents. Read code yourself only for the few spots the plan
   depends on, in line ranges. If your context is already large when a run starts (e.g. other
   work earlier in the session), suggest `/clear` or `/compact` before planning.
4. Write `<feature folder>/plan.md` from `templates/plan.template.md`. It must contain:
   - the goal in 2-3 lines and what is out of scope
   - the system design: data model / schema changes, API contract (every endpoint with method,
     path, request and response JSON, error codes), authorization rules, workflow/status rules
   - which existing patterns are reused and what differs
   - the task list per agent, with the exact repos and main files each will touch
   - the mode (quick / solo / hybrid / team) and, for hybrid and team, which agents and the model
     tier of each (see "Choosing models"), with a one-line reason
   - the branch name (`feature/<slug>` unless team.md says otherwise)
   - risks and open questions
5. **STOP.** Show the user a short summary of plan.md and its path, then ask:
   "Approve, or tell me what to change?" Do not create branches, edit code, or start any
   subagent until the user clearly approves (e.g. "approved", "go", "sige"). If they ask for
   changes, update plan.md and ask again.

## Phase 2: Prepare

1. In every repo the plan touches, create and check out the feature branch from the base branch
   named in team.md. Do this yourself before any subagent starts, so they never race on git.
2. Hybrid and team only: write one brief per agent, `<feature folder>/tasks/<agent>.md`, from
   `templates/task.template.md`. Make it a **packet**: everything the agent needs so it does not
   have to explore - its role conventions from team.md, only the "on demand" files this feature
   needs, and the exact code to touch or mirror as paths with line ranges, method and field names
   and signatures you already found. Keep it under about 8 KB. Also list the repos and folders it
   owns, the shared files it must not edit, the never-stage files, the tasks, the fast and full
   checks, and the commit rules.

## Phase 3: Build

### Quick and solo
Build it yourself, following "Work rules": backend first (with its tests), then frontend, then
the changelogs. Update `status.md` after each part. If your context is getting large between the
backend and frontend parts, save `status.md` and suggest the user run
`/compact continue cy-team on <slug>` before you go on.

### Hybrid
Give the heavier, self-contained side (usually the backend) to one agent with a packet brief, run
it in the background, and build the other side yourself while it runs. Do not edit files the
agent owns. Review its result as in Phase 4.

### Team
Start agents in parallel wherever the dependencies allow, by issuing their launches together:

- **Wave 1 (parallel):** `designer` and `backend-dev`. The backend works from the API contract,
  so it does not need the design.
- **Wave 2:** `frontend-dev`, as soon as `design-spec.md` exists (the backend may still be
  running). If no designer is needed, start `frontend-dev` in wave 1 too.
- At most 4 agents at once.

### Launching agents (hybrid and team)
Each launch prompt is short: "You are on feature `<slug>`. Read `<feature folder>/tasks/<agent>.md`
and follow it. Workspace root: `<path>`." Do not paste the whole plan into the prompt; point to files.
Agents never edit shared files outside their ownership (e.g. a helper other pages use); they ask
for the change in their report and you apply it after they finish.

Launch each agent at the model tier approved in plan.md (see "Choosing models").

### Choosing models

Plans name **tiers**, not model names, so the skill keeps working when new models come out.

| Tier | Claude Code | Codex |
| --- | --- | --- |
| `strong` | `opus` | `strong` in `codex/models.toml` |
| `standard` | `sonnet` | `standard` in `codex/models.toml` |
| `fast` | `haiku` | `fast` in `codex/models.toml` |

These apply to agents in hybrid and team. In quick and solo everything runs on the session's
model: suggest `/model sonnet` for light work and a strong model for heavy planning.
The defaults save tokens; only the backend moves up to `strong`, and only when the work is heavy.
team.md may change these rules.

| Agent | Default tier | Use `strong` when |
| --- | --- | --- |
| `designer` | `fast` | never by default; use `standard` if the screen has many states or a complex flow |
| `backend-dev` | `standard` | the work is **heavy** (see below) |
| `frontend-dev` | `standard` | only if the user asks |

How to launch at a tier:
- **Claude Code:** start the agent with the tier's model as the launch's model override when it
  differs from the agent file's default (e.g. `backend-dev` with `opus` for heavy work).
- **Codex:** each tier is baked into the agent file, so pick the agent: `backend-dev` (standard)
  or `backend-dev-heavy` (strong). Same for any other `-heavy` variants that exist.
- **No subagents:** do the role yourself; tiers do not apply.

Backend work is **heavy** if any of these apply:
- workflow or status transitions, edit locks, approvals, amendments
- authorization, office/scope rules, or role capabilities
- computations, totals, consolidation, ceilings, or variance checks
- schema changes across several tables, or data migrations
- changes in more than one service
- report aggregation or export logic

Otherwise it is **light**: CRUD that follows an existing pattern, a new field, a single endpoint,
or a bug fix with a known cause.

The lead's own model is whatever the user selected for the session; recommend a `strong` model
for the lead in the plan summary if the session is on a smaller one and the plan is heavy.

When an agent replies, save its reply as `<feature folder>/reports/<agent>.md` yourself (agents
may not write report files) and update `status.md`.

Rules while they run:
- An agent that finds the contract wrong or incomplete must stop and report back, not invent a
  different contract. You decide; if the change is material, ask the user, update plan.md, and
  tell every affected agent.
- Never let two agents own the same repo at the same time.

## Phase 4: Review

When the work is done (your own in quick/solo, or each agent's report in hybrid/team), check it
the same way:

1. Read each agent's report (`<feature folder>/reports/<agent>.md`). For the diffs, start with
   `git diff --stat <base>...HEAD` per repo and open only the parts you need to check; do not
   read whole diffs of large files.
2. Check:
   - the line counts in `--stat` fit the change: a file with hundreds of changed lines for a
     small edit means line endings or encoding changed; send it back
   - only the intended files were committed, and no pre-existing change (from `status.md`) is
     in any commit
   - commit messages follow team.md: run `git log --format=%B <base>..HEAD` and check the
     format and that no forbidden attribution (e.g. `Co-Authored-By`) is present
   - every endpoint in the contract exists in the backend with the same path, method, field
     names and types, and the frontend calls it exactly that way
   - authorization lives where the conventions require (server side), not only in the UI
   - the conventions' required checks ran and passed (as reported, and re-run if in doubt)
   - changelogs and any required docs were updated
   - no secrets, credentials, connection strings, or real personal data were written anywhere
3. If something fails, send it back to the same agent (continue that agent if your runtime
   allows, otherwise start it again with a fix brief). Allow at most 2 fix rounds per agent, then
   report the remaining problem to the user instead of looping.

## Phase 5: Deliver

1. Confirm each repo's commits follow team.md (message format, attribution rules). Commits are
   made by the agents; do not squash or rewrite them unless team.md says so.
2. If team.md enables merge/pull requests, push each feature branch and open one request per
   repo with the command team.md gives. Link the requests to each other in their descriptions
   when the feature spans repos.
   If no MR/PR tool is available, push nothing; write `<feature folder>/mr-description.md` per
   repo instead (title, summary, how to test, links to sibling branches) so the user can paste it.
3. Write `<feature folder>/summary.md` with:
   - what was built, per repo, with branch and MR/PR links
   - the checks that actually ran and their results, and any checks that did not run and why
   - **How to test by hand**: short steps the user can follow in the running app (which page,
     what to enter, what should appear), since agents usually cannot start the services
   - open issues, follow-ups, and anything the user must do (e.g. run an SQL script)
   Reply to the user with the short version (see "Talking to the user").
4. Lessons: if a trap bit during this run (e.g. line endings, a wrong path, a check that needs a
   flag) or the user corrected you, add one line to `.claude/cy-team-lessons.md` (create it if
   missing). Keep it at most 20 lines; merge or drop old lines rather than growing it.

## Never

- Start coding or create branches before the plan is approved.
- Claim a check passed without running it.
- Let anyone stage files with `git add -A`, `git add .` or `git commit -a`.
- Run against production data, seed databases, or print secrets.
- Merge requests yourself; the user approves and merges.
