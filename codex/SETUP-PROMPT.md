# Codex setup for cy-team

`INSTALL.md` sends Codex here. You can also paste everything below the line into Codex yourself,
from the kit folder (`~/.cy-team`). Running it again only rewrites the cy-team files.

To update models later, edit `~/.cy-team-local/models.toml` (or `codex/models.toml`) and tell
Codex: "update cy-team agent models from models.toml". It reruns step 3 only.

---

Set up the cy-team dev team for Codex from the kit folder `~/.cy-team`. Work only on the files
named here.

1. **Check the format first.** Use the current Codex documentation for custom agents
   (subagents) and skills, not memory: the agent file format (TOML, one file per agent), the
   required keys, how instructions, `model` and `model_reasoning_effort` are written, and the
   personal folders (`~/.codex/agents/` for agents and the user-level skills folder). If anything
   below conflicts with the docs, follow the docs and tell me what you changed.

2. **Skill.** Copy `skills/cy-team/` (with `SETUP.md` and `templates/`) into my user-level Codex
   skills folder as `cy-team`. Do not change its content.

3. **Agents.** Read the models file (`~/.cy-team-local/models.toml` if it exists, otherwise
   `codex/models.toml`). For each entry under `[agents]`, write one custom agent file in
   `~/.codex/agents/`:
   - `designer` from `agents/designer.md`
   - `backend-dev` from `agents/backend-dev.md`
   - `backend-dev-heavy` from `agents/backend-dev.md` (same instructions; its description must
     say it is for heavy backend work and that the cy-team lead picks it)
   - `frontend-dev` from `agents/frontend-dev.md`

   For each one:
   - name and description from the Markdown frontmatter
   - the agent's instructions = the Markdown body below the frontmatter, unchanged
   - `model` = the tier's name from `[tiers]`, `model_reasoning_effort` = the tier's value from
     `[effort]`
   - drop the Claude-only frontmatter keys (`tools`, `model`). Give `designer` the most
     restrictive sandbox that still lets it write files in the workspace, and the others
     workspace-write access, using the keys the docs define.

   If any tier still says `<...>`, ask me which models to use (offer the models this Codex
   install has), then save my answer to `~/.cy-team-local/models.toml`.

4. **Global settings.** If my Codex config has no `[agents]` limits, suggest values for running
   3 agents at once (e.g. max_threads 3) and wait for my OK before editing my config.

5. **Report.** List every file you wrote, the model and effort per agent, and anything in the
   docs that differed from these instructions.
