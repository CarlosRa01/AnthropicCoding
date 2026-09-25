# Project conventions

## Dataverse naming
- Publisher prefix: `new_` (solution "Stakeholders", environment hackathon2-team2).
- New tables and columns: schema/logical names are all lowercase, with no underscore
  between words — e.g. `new_roleinproject`, not `new_role_in_project` or `new_RoleInProject`.
  The connector derives the schema name from the display name, so create the column with
  the display name written as the desired name (e.g. `roleinproject`) and let the user
  set the readable display name in the maker portal afterwards.
- The Dataverse connector cannot delete tables/columns/flows or add components to a
  solution; list those steps for the user to do in the maker portal.

## Data model
See `docs/data-model.md`. The skill lives in `.claude/skills/stakeholder-intelligence/`.
