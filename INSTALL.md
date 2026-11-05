# cy-team install instructions (for the agent)

You are Claude Code or Codex. The user pasted a request to install or update cy-team from this
repository. Follow these steps, explain briefly what you are doing, and ask before anything
marked **ASK**. This installs cy-team for the user's account only; it does not touch any project.

## 1. Get or update the kit

- Kit folder: `~/.cy-team` (Windows: `%USERPROFILE%\.cy-team`).
- Not there yet: `git clone <this repo's URL> ~/.cy-team`.
- Already there: `git -C ~/.cy-team pull`.
- If cloning fails because of access, tell the user to log in to GitHub on this computer
  (`gh auth login`, or Git Credential Manager on Windows) and stop.

Paths below are relative to the kit folder unless they start with `~`.

## 2. Which tool are you?

- **Claude Code** -> step 3A.
- **Codex** -> step 3B.
If the user asked for both, do both.

## 3A. Claude Code

Copy, creating folders as needed:
- `skills/cy-team/` (whole folder) -> `~/.claude/skills/cy-team/`
- `agents/designer.md`, `agents/backend-dev.md`, `agents/frontend-dev.md` -> `~/.claude/agents/`

If a destination file exists and is not from cy-team (its frontmatter `name:` differs, or the
`cy-team` skill folder has no `SETUP.md`), **ASK** before overwriting. Files from an earlier
cy-team install are replaced without asking; this is how updates work.

## 3B. Codex

Follow `codex/SETUP-PROMPT.md` (everything below its line), with the kit folder as the base path.
For model names, use `~/.cy-team-local/models.toml` if it exists (the user's own copy, which
survives updates); otherwise `codex/models.toml`. When the user gives model names for the first
time, save them to `~/.cy-team-local/models.toml`.

## 4. Check

- Claude Code: confirm the files exist in `~/.claude/skills/cy-team/` and `~/.claude/agents/`.
- Codex: list the agent files written and each agent's model.
- Report whether `git` is available, and whether `gh` and/or `glab` are installed and logged in
  (`gh auth status`, `glab auth status`). They are only needed for opening pull/merge requests.

## 5. Report

Short report:
- kit version: `git -C ~/.cy-team log -1 --format="%h %cs"`
- what was installed or updated, what was skipped and why
- next steps for the user:
  1. Restart Claude Code / Codex.
  2. Open it in a project folder and run `/cy-team setup` (Codex: "use the cy-team skill: setup").
  3. Then `/cy-team <a small first feature>`.
