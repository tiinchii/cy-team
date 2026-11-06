# Task brief: <agent> on `<slug>`

Workspace root: `<path>`
Branch (already checked out by the lead): `feature/<slug>`

## Read first, in this order
1. `.claude/team.md`
2. Your role's conventions from team.md ("All agents" + yours)
3. On-demand files this feature needs: <list, or none>
4. `<feature folder>/plan.md` - especially "API contract" and your task section
5. <`<feature folder>/design-spec.md` - frontend only>
6. Reference code to mirror, with line ranges for large files:
   <e.g. `service/ExampleService.java` lines 120-260 (`save`, `submit`)>

## You own (write only here)
- <repo>/<folder>
Everything else is read-only for you.

## Never stage (pre-existing changes)
- <repo>/<file>, or "none"

## Tasks
1. <task>
2. <task>

## Done when
- <observable result, e.g. "GET /x returns the contract shape; rejected path tested">
- Checks pass: <exact commands>
- `changelog.md` updated in every repo you changed
- Committed on the feature branch following team.md commit rules

## Report
Your final reply is the report, in the format from your agent instructions. The lead saves it to
`<feature folder>/reports/<agent>.md`.
