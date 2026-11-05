# cy-team install instructions (for the agent)

You are Claude Code or Codex. The user pasted a request to install or update cy-team from this
repository. Follow these steps, explain briefly what you are doing, and ask before anything
marked **ASK**. This installs cy-team for the user's account only; it does not touch any project.

**Two places, both required:**
- `~/.cy-team` is only the **source copy** of this repo, used for updates. Tools do not read it.
- The install is **not done** until the files are copied into the tool's own folders
  (step 3A for Claude Code, step 3B for Codex). Never stop after the clone.

## 1. Get or update the kit

- Kit folder: a folder named `.cy-team` **inside** the home folder.
  - Windows: `C:\Users\<name>\.cy-team`. In PowerShell write it as `Join-Path $HOME '.cy-team'`
    or `"$HOME\.cy-team"`, never `"$HOME.cy-team"` (that drops the backslash and creates
    `C:\Users\<name>.cy-team`).
  - macOS/Linux: `$HOME/.cy-team`.
- If a wrongly named `<home>.cy-team` folder exists next to the home folder from an earlier
  install, tell the user and **ASK** before deleting it.
- Not there yet: `git clone <this repo's URL> ~/.cy-team`.
- Already there: `git -C ~/.cy-team pull`.
- If cloning fails because of access, tell the user to log in to GitHub on this computer
  (`gh auth login`, or Git Credential Manager on Windows) and stop.

Paths below are relative to the kit folder unless they start with `~`.

## 2. Which tool(s) to install for

1. **Yourself first, without asking.** You know whether you are Claude Code (-> step 3A) or
   Codex (-> step 3B). Only if you truly cannot tell, ask the user.
2. **The other tool: ASK.** Check whether the other tool is on this computer (`~/.codex/` exists
   or `codex` is on the PATH; `~/.claude/` exists or `claude` is on the PATH). If it is, ask once:
   "I found <other tool> here too. Install cy-team for it as well?" Do its step too if they say
   yes. If the user already said "both" in their message, do both without asking.
3. Say at the start which tool(s) you are installing for.

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

## 4. Check (required)

Verify by listing the real folders, not from memory. Show the user the full paths:
- Claude Code: `~/.claude/skills/cy-team/SKILL.md`, `SETUP.md`, `templates/`, and
  `~/.claude/agents/designer.md`, `backend-dev.md`, `frontend-dev.md`.
- Codex: the `cy-team` skill in Codex's user skills folder, and the four agent files in
  `~/.codex/agents/` with each agent's model.
If anything is missing, go back and finish that step before reporting.
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
