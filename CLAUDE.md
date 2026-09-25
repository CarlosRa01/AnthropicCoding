# Project conventions

## Dataverse naming
- Publisher prefix: `new_` (solution "Stakeholders", environment hackathon2-team2).
- Schema names of new tables and columns: all lowercase, no underscore between words
  — e.g. `new_roleinproject`, not `new_role_in_project` or `new_RoleInProject`.
- Display names are readable: capital letters allowed, multiple words separated by
  spaces ("Role in Project", "Confidence Level"). Never leave a display name in its
  schema-name form ("roleinproject").
- Choice columns ALWAYS use a global choice (choice column synced to a global
  choice). Never create a local choice. The global choice follows the same naming
  rule (e.g. `new_roleinproject`, display "Role in Project"). The connector can only
  create local choices, so creating a choice column is a portal step for the user:
  first the global choice, then the column synced to it.
- Caution: `update_table` with `updatable: true` on a choice column can overwrite the
  column's display name with the name passed in. Check display names afterwards.
- Known exceptions that cannot be changed (system-generated): the primary name column
  `new_Name` and primary key `new_stakeholdersId` on `new_stakeholders`.
- The connector derives the schema name from the display name, so create the column
  with the display name written as the schema name (e.g. `roleinproject`), then ask
  the user to set the readable display name in the maker portal.
- The Dataverse connector cannot delete tables/columns/flows or add components to a
  solution; list those steps for the user to do in the maker portal.

## Data model
See `docs/data-model.md`. The skill lives in `.claude/skills/stakeholder-intelligence/`.
