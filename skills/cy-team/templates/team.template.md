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

## Conventions (read before planning or coding)
- `.claude/skills/<project>-conventions/SKILL.md`

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
