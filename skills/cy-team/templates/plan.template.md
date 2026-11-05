# Plan: <feature title>

- Slug: `<slug>`
- Branch: `feature/<slug>`
- Status: DRAFT  <!-- DRAFT -> APPROVED (date) -->
- Requirements: `requirements.md` (or link)

## Goal
<2-3 lines: what the user gets and why.>

**Out of scope:** <what this does not do>

## System design

### Data model
| Table | Change | Columns / notes |
| --- | --- | --- |
| `example_table` | new | `id`, `fiscal_year`, `status` (enum, length 30) |

Manual SQL needed (things the ORM cannot do): <none / file name under scripts/>

### API contract
#### `GET /example?year=2027`
- Who may call: <roles / scope rule, enforced in the service>
- Response 200:
```json
{ "id": 1, "fiscalYear": 2027, "status": "DRAFT" }
```
- Errors: 400 <when>, 404 <when>, 409 <when>

<repeat per endpoint>

### Authorization and workflow
- <who can view / edit / review; where it is enforced>
- Status transitions: <from -> to, who>

### Patterns reused
- Reuse: <existing pattern / helper / component>
- Different from that pattern: <what and why>

## Tasks

### Models
| Agent | Tier | Reason |
| --- | --- | --- |
| designer | fast / skip | <e.g. "one simple screen"> |
| backend-dev | standard / strong | <light or heavy, and why> |
| frontend-dev | standard | |

### designer  (needed: yes/no)
- <screens / states / wording to specify>

### backend-dev
- Repos: <repo list>
- <task> - main files: <paths>

### frontend-dev
- Repos: <repo list>
- <task> - main files: <paths>

## Checks
- backend: <command from team.md>
- frontend: <command from team.md>

## Risks and open questions
- <question the user should answer, or risk to watch>
