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

Repo instruction files (each tool loads only its own; the other tool must read it):
| File | Claude Code | Codex |
| --- | --- | --- |
| <e.g. `api-service/CLAUDE.md`> | auto-loaded | read it |
| <e.g. `api-service/AGENTS.md`> | read it | auto-loaded |

## Reading limits
- Large file: over 1,000 lines. Search first, then read only the needed methods or line ranges.

## Checks (run without asking)
- Fast check while working: backend `<e.g. ./mvnw test -Dtest=<TestClass>>`; frontend `<e.g. eslint on one file>`
- Full check when done (at most twice): backend `<command>` in each changed backend repo;
  frontend `<command>` on changed files

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

## Known traps
One line each, at most 20. The lead adds a line when a trap bites or the user corrects it.
- <e.g. "web-app files use CRLF; keep each file's line endings">
