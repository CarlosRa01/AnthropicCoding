# Dataverse data model — solution "Stakeholders"

Environment `hackathon2-team2`, solution `Stakeholders` 1.0.0.0 (publisher prefix `new_`).
Read from the live environment on 2026-09-25.

```mermaid
erDiagram
    account ||--o{ new_project : "new_account (required)"
    account |o--o{ new_stakeholders : "new_accountlookup"
    new_project |o--o{ new_stakeholders : "new_projectlookup"

    account {
        guid accountid PK
        string name "required"
        choice new_company_type "custom"
        string new_ownership_structure "custom"
        money new_balance_sheet "custom"
        string websiteurl
    }
    new_project {
        guid new_projectid PK
        string new_name "primary"
        string new_projectname "required"
        lookup new_account FK "account, required"
        string new_description
        datetime new_startdate
        status statuscode "Active, Inactive, On Hold, Completed"
    }
    new_stakeholders {
        guid new_stakeholdersid PK
        string new_name "primary"
        string new_firstname "required"
        string new_lastname "required"
        string new_role "required"
        lookup new_accountlookup FK "account"
        lookup new_projectlookup FK "new_project"
        string new_account "required, text copy of account"
        string new_projectname "required, text copy of project"
        choice new_quadrant "required"
        choice new_engagementrisk "Low, Medium, High"
        choice new_risklevel "Low, Medium, High"
        choice new_confidencelevel "High, Low"
        int new_skilllevel
        bool new_isteammember "required"
        date new_reportdate
        datetime new_lastresearchdate
        text report_columns "12 multiline report fields"
    }
```

All tables are user-owned (`ownerid` → systemuser/team) with standard audit lookups.

## Open issues

- `new_stakeholders.new_account` / `new_projectname` are required text copies of the lookups and can drift.
- A stakeholder reaches an account directly and via its project; nothing enforces consistency.
- Schema vs. skill: `new_quadrant` has no "Not assessable" option and is required; `new_confidencelevel`
  lacks Medium; `new_skilllevel` cannot hold a range; `new_risklevel` and `new_engagementrisk` overlap.

## Outside the solution

- `contact` and `opportunity` were removed from the solution. Custom profile columns on contact
  still exist, and the flow "Save Stakeholder Profile to Dataverse" still writes to contact.
- `new_opportunitycontact` (contact × opportunity) is a custom table not in the solution; empty.
