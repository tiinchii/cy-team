# Team profile: <project name>

The cy-team lead and agents read this file first. Rules here override the generic agent
instructions. Keep it short; put detailed coding rules in the conventions files.

## Workspace
- Root: the folder that contains this `.claude/` folder
- Feature docs folder: `docs/features`

## Repos and ownership
| Repo folder | Owner agent | Stack | Base branch |
| --- | --- | --- | --- |
| `api-service` | backend-dev | <e.g. Spring Boot 3, Java 21, Maven, PostgreSQL> | `main` |
| `web-app` | frontend-dev | <e.g. React 18, Vite, Tailwind> | `main` |

## Conventions: who reads what
Each agent reads only "All agents" plus its own list. "On demand" files are read only when the
condition applies or a task brief names them.

| Who | Files |
| --- | --- |
| All agents | `.claude/skills/<project>-conventions/SKILL.md` (shared rules: wording, changelog, commits) |
| lead | <planning checklist, project map> |
| designer | <UI/wording guide, if separate> |
| backend-dev | `.claude/skills/<project>-conventions/backend.md` |
| frontend-dev | `.claude/skills/<project>-conventions/frontend.md` |

On demand:
- `<file>` - when <condition, e.g. "a new form or a change to form logic">

Auto-loaded, do not re-read: <e.g. `api-service/CLAUDE.md` (Claude Code), `AGENTS.md` (Codex)>

## Reading limits
- Large file: over 1,000 lines. Search first, then read only the needed methods or line ranges.

## Checks (agents run these without asking)
- backend-dev: `<command>` in each changed backend repo
- frontend-dev: `<command>` on changed files

## Checks that need the user's permission
- <e.g. full production build, anything that touches a shared database>

## Git
- Branch: `feature/<slug>` in every touched repo
- Commit message: <format>
- Attribution: <e.g. "no Co-Authored-By or AI attribution lines">
- Changelog: <file and format, or "none">
- Merge/pull requests: <"open with: gh pr create --fill --base main" / "user opens them">

## Never
- <project-specific prohibitions>
