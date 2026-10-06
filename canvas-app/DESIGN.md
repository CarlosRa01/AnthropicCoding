# Canvas app "Stakeholder Intelligence" — MVP design

Opened by PMs in Microsoft Teams (Power Apps tab). English labels. Segoe UI. Tablet
layout, sizes relative to `Parent` so it also works in a narrow Teams tab.

Decisions (2026-10-06): PMs create projects, contacts and project stakeholders in the
app. PMs see **only their own projects** (owner = signed-in user). Report generation
comes later (button disabled for now).

## Data sources (Dataverse, Stakeholders solution)

| Power Fx name | Table |
|---|---|
| `Projects` | `new_project` |
| `'Project Stakeholders'` | `new_stakeholders` |
| `Contacts`, `Accounts`, `Users`, `Notes` | standard |

Names above are the display (plural) names Power Apps shows; confirm in Studio.

## App-level formulas (App.Formulas)

```
gblMe = LookUp(Users, 'Primary Email' = User().Email);
```

## Screens

```
scrProjects ─► scrProject ─► scrReport
     │             └────────► scrAddStakeholder
     └─► scrNewProject
```

### scrProjects — "My Projects"
- Search box `txtSearch` (hint "Search projects").
- Button "New project" → `Navigate(scrNewProject)`.
- Gallery `galProjects`, Items:
  `Sort(Filter(Projects, Owner = gblMe, StartsWith('Project Name', txtSearch.Text)), 'Start Date', SortOrder.Descending)`
  Row: project name, account name, status chip (Active green, On Hold amber,
  Completed grey, Inactive grey), start date. OnSelect: `Set(gblProject, ThisItem); Navigate(scrProject)`.
- Empty state: "No projects yet. Create your first project."

### scrNewProject
Inputs: `txtProjectName` (required), `cmbAccount` (Items `Accounts`, search on name,
required), `txtDescription` (multi-line, max 1000), `dpStart`.
Save (disabled until name and account set):
```
Set(gblProject, Patch(Projects, Defaults(Projects), {
  'Project Name': txtProjectName.Text,
  Account: cmbAccount.Selected,
  Description: txtDescription.Text,
  'Start Date': dpStart.SelectedDate }));
Navigate(scrProject)
```
Owner is set to the creator by Dataverse, so the project shows up in "My Projects".

### scrProject
- Header: project name, account, status chip, description, back button.
- Button "Add stakeholder" → `Navigate(scrAddStakeholder)`.
- Gallery `galStakeholders`, Items:
  `Sort(Filter('Project Stakeholders', Project = gblProject), Name)`
  Row: contact name, job title, role in project, chips for quadrant / engagement
  risk / confidence, report date. OnSelect: `Set(gblPS, ThisItem); Navigate(scrReport)`.
- Chip colours: Manage Closely red, Keep Satisfied amber, Keep Informed blue, Monitor
  grey, Not assessable outlined grey; risk High red, Medium amber, Low green.
- Empty state: "No stakeholders yet. Add the first one."

### scrAddStakeholder
- `cmbContact`: Items `Filter(Contacts, 'Company Name' = gblProject.Account)` (search on
  full name). Link "Contact not listed? Create one" shows the inline panel:
  `txtFirst`, `txtLast`, `txtJob`; "Create contact" =
  `Set(gblNewContact, Patch(Contacts, Defaults(Contacts), {'First Name': txtFirst.Text, 'Last Name': txtLast.Text, 'Job Title': txtJob.Text, 'Company Name': gblProject.Account}))`
  and then selects it in `cmbContact` (DefaultSelectedItems = gblNewContact).
- `ddRole` (optional): Items = choices of Role in Project.
- Save (disabled until a contact is chosen). Duplicate guard first:
  `If(!IsBlank(LookUp('Project Stakeholders', Project = gblProject && Contact = cmbContact.Selected)), Notify("This person is already a stakeholder in this project.", NotificationType.Warning), Patch(...) ; Back())`
  Patch values: `Name` = `cmbContact.Selected.'Full Name' & " – " & gblProject.'Project Name'`,
  `Contact`, `Project` = gblProject, `'Role in Project'`; quadrant and risk start as
  "Not assessable".

### scrReport
- Header: contact name, role, project; chips (quadrant, risk, confidence); report date.
- `htmlReport` (HTML text control), `Html` = `If(IsBlank(gblPS.'Report HTML'), "<p style='font-family:Segoe UI'>No report yet.</p>", gblPS.'Report HTML')`.
  Keep `AutoHeight` on, inside a scrollable container.
- "Earlier reports" list: `Sort(Filter(Notes, Regarding = gblPS), Created On, SortOrder.Descending)`
  showing subject and date only (attachment content cannot be decoded in Power Fx).
- Button "Generate report" — disabled, tooltip "Coming soon".

## Visual style
Page #f6f5f2, cards white (14 px radius, 1 px #e4e1da border), text #1f2a37, accent
#2d6a8a, chips as in the report skill. Icons from the built-in icon set. Header 56 px.

## To verify in Studio after pasting (could not be tested outside Studio)
1. Display names of tables/columns/choices in Power Fx (autocomplete will show them).
2. `Owner = gblMe` shows no delegation warning (blue underline). If it does, use
   `Filter(Projects, Owner.'Primary Email' = User().Email)`.
3. The customer lookup `'Company Name'` on Contact may require the polymorphic form
   shown by autocomplete.
4. `Regarding = gblPS` on Notes.
5. Dataverse permissions: PMs need create/read/write on Project, Project Stakeholder,
   Contact, Account (read) and Note (read) — part of the PM security role (go-live item).
6. Premium licence (Dataverse) — see CLAUDE.md.
