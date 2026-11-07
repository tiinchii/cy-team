# cy-team

A planned, approved, checked way to build features with Claude Code and Codex. The **lead** (the
main agent) plans, waits for your approval, builds it itself by default, or hands parts to
subagents when you ask (hybrid or team), reviews, commits safely, and prepares the merge/pull
requests. You approve and merge.

```
/cy-team "<feature>"
   |
 Lead: reads .claude/team.md + your conventions -> writes plan.md -> STOPS for your approval
   |
   +-- wave 1 (parallel): designer ....... design-spec.md (UI/UX)
   |                      backend-dev .... server code + tests, runs checks, commits
   +-- wave 2:            frontend-dev ... UI code, runs checks, commits
   |
 Lead: reviews contract match, checks, changelogs -> pushes -> opens MR/PR -> summary
```

Works with any stack. Each project describes itself in `.claude/team.md`, which `/cy-team setup`
writes for you.

## 1. Install (once per computer)

Open Claude Code or Codex anywhere and paste:

```
Install cy-team from https://github.com/tiinchii/cy-team.git. Read INSTALL.md in that repo and follow it.
```

The agent clones the kit to `~/.cy-team` and installs the skill and agents for your account.
**To update later, paste the same line again.**

## 2. Set up a project (once per project)

Open Claude Code or Codex in the project folder (one repo, or a folder that holds several repos)
and run:

```
/cy-team setup
```

The lead inspects the repos (stack, build tool, lint/test scripts, git host, commit style, existing
rules), drafts `.claude/team.md`, asks you to confirm, and can also write a conventions skill
from your existing code if you don't have one.

To save tokens, each agent reads only the rules for its role: shared rules for everyone,
backend rules for `backend-dev`, frontend rules for `frontend-dev`. Setup can split an existing
conventions skill that mixes them (it backs up first and asks before saving). Large code files
are searched first and read in parts, never whole.

## 3. Use

```
/cy-team Add a user profile page where users can edit their name and photo
```

The lead shows you `docs/features/<slug>/plan.md`. Reply **approved**, or say what to change.
Nothing touches the code before you approve.

Each feature keeps its files in `docs/features/<slug>/`:
`status.md`, `requirements.md`, `plan.md`, `design-spec.md`, `tasks/`, `reports/`, `summary.md`.

### Modes

| Command | Who builds | When |
| --- | --- | --- |
| `/cy-team quick <fix>` | the lead, short plan | labels, fields, small bug fixes |
| `/cy-team <feature>` | the lead (solo, default) | most work; cheapest |
| `/cy-team hybrid <feature>` | one agent for the heavy side, the lead does the rest meanwhile | big backend plus a small UI |
| `/cy-team team <feature>` | designer, backend and frontend agents in parallel | large, truly separate work, when speed matters more than usage |

Each agent is a full cold start (rules and code read again), so team mode uses several times
more of your usage than solo. Every mode keeps the plan and approval, safe commits, checks,
`status.md`/resume, and the summary with manual test steps. Traps that already happened in a
project are kept in a short "Known traps" section at the end of `.claude/team.md`, read at the start of every run.

### Saving tokens: compact, clear, resume

- During a feature: `/compact continue cy-team on <slug>`
- After a feature: `/clear`, then `/cy-team <next feature>`
- In a new session, or after `/clear`:
  ```
  /cy-team continue <slug>
  ```
  The lead reads `status.md`, tells you where the work is, and continues without redoing
  finished steps.

## Models

Plans use **tiers**, not model names, so the kit keeps working when new models come out.

| Who | Tier | Claude Code | Codex |
| --- | --- | --- | --- |
| Lead | strong | the session's model (pick a strong one, e.g. `/model opus`) | the session's model |
| designer | fast | `haiku` | `fast` in models.toml |
| backend-dev | standard, or strong when the work is heavy | `sonnet` / `opus` | `backend-dev` / `backend-dev-heavy` |
| frontend-dev | standard | `sonnet` | `standard` in models.toml |

"Heavy" backend work means workflow/status rules, authorization, computations and totals,
multi-table schema changes, changes in more than one service, or report logic. The lead decides
per feature and shows it in plan.md for your approval. To override once, say so when approving,
e.g. "approved, but use strong for the frontend".

- **Claude Code:** `opus`, `sonnet`, `haiku` always mean the newest version. Nothing to update.
- **Codex:** model names live only in `~/.cy-team-local/models.toml`. When a new model comes out,
  edit that file and tell Codex "update cy-team agent models from models.toml".

## Overrides

- One project needs a special agent: put a file with the same name in
  `<project>/.claude/agents/`. It wins over the installed one.
- Project rules always win: `team.md` and the conventions it lists override the generic agents.

## Running it from your phone

`claude remote-control` in the project folder makes the session reachable from the Claude app.
Useful when your repos or database only exist on your work computer.

## What's in this repo

| Path | Purpose |
| --- | --- |
| `INSTALL.md` | Instructions the agent follows to install or update |
| `skills/cy-team/SKILL.md` | The lead's playbook |
| `skills/cy-team/SETUP.md` | How the lead sets up a project |
| `skills/cy-team/templates/` | team.md, plan, and task brief templates |
| `agents/` | designer, backend-dev, frontend-dev (Claude Code format) |
| `codex/` | Codex setup prompt and the models file |
