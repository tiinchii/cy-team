# cy-team project setup

Goal: give the current project a `.claude/team.md` (and, if wanted, a conventions skill) so the
team can work on it. Explain briefly what you find as you go. Never commit, push, or change code
inside the project's repos during setup.

## 1. Find the workspace

- Use the current folder unless the user named another one.
- If `.claude/team.md` already exists here: show a short summary of it, compare it with
  `templates/team.template.md`, mention any template sections it lacks, and ask whether to update
  it. Do not overwrite without a yes.
- List the direct subfolders. A workspace is either one repo (the folder itself is a git repo)
  or several repos side by side (subfolders are git repos). Note which applies.

## 2. Inspect each repo

For each repo, read only what you need:
- **Stack and build:** `pom.xml` / `mvnw`, `build.gradle` / `gradlew`, `package.json` (+ lockfile),
  `requirements.txt` / `pyproject.toml`, `composer.json`, `go.mod`, `*.csproj`, etc.
- **Role:** backend, frontend, or other (mobile, infra, shared library). Frontend signals: React,
  Vue, Angular, Svelte, Vite, Next, Tailwind. Backend signals: Spring, Express, Nest, Django,
  Laravel, .NET, a database driver.
- **Checks:** existing lint, test, verify, typecheck scripts. Prefer the narrowest fast check
  (e.g. lint changed files, `./mvnw verify`, `npm run lint`, `npm test`). Note anything slow or
  risky (full builds, tests that need a real database).
- **Git:** current branch, default branch, remote host (`git remote -v` -> GitHub, GitLab, other).
  Pick the merge/pull request command: `gh pr create` for GitHub, `glab mr create` for GitLab,
  "user opens them" otherwise.
- **Existing rules:** `AGENTS.md`, `CLAUDE.md`, `CONTRIBUTING.md`, `.claude/skills/*/SKILL.md`,
  lint configs, a changelog file and its format, commit message style (`git log --oneline -15`).

## 3. Draft team.md

Fill `templates/team.template.md`:
- Repos table: folder, owner agent (`backend-dev` / `frontend-dev`), stack, base branch.
  Repos that fit neither role: list them as read-only for the team and say so.
- Conventions: every existing skill or rules file found in step 2.
- Checks: the commands from step 2, split into "agents run these without asking" (fast, local,
  safe) and "needs the user's permission" (slow builds, anything touching a real database).
- Git: branch pattern `feature/<slug>`, commit style copied from the repo's history, changelog
  rule if a changelog exists, the MR/PR command, and the attribution rule (ask the user).
- Never: no secrets in files or logs, no real personal data, no seeding or migrations against a
  real database, plus anything the existing rules forbid.
- Write `<TODO>` for anything you could not find.

## 4. Confirm with the user

Show the draft and ask, in one message:
1. Is the repo/role mapping right?
2. Which checks may agents run without asking?
3. Should commits and MR/PR descriptions include AI attribution (Co-Authored-By) or not?
4. Should the lead open merge/pull requests, or will the user?
5. Any `<TODO>` items.

Save to `.claude/team.md` only after the user confirms. Create the feature docs folder
(`docs/features/` unless team.md says otherwise).

## 5. Conventions skill (optional)

If no conventions skill or rules file was found, offer to create one:
`.claude/skills/<project>-conventions/SKILL.md`. If the user agrees, write it from the code itself:
package/folder structure, naming, layering, error handling, DTO/component patterns, styling,
lint rules, test style - each rule backed by what the existing code already does, with file
examples. Keep it under ~150 lines. Show it and save after the user confirms, then add it to the
Conventions list in team.md.

## 6. Finish

Report what was saved and the next step:
`/cy-team <a small first feature>`.
