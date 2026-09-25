---
name: stakeholder-intelligence
description: >
  Generates structured stakeholder profiles and communication recommendations for
  project managers. Use this skill whenever the user asks to analyze a stakeholder,
  create a stakeholder profile or report (Stakeholder-Bericht, Stakeholder-Analyse),
  assess stakeholder risk, prepare for a meeting with a difficult stakeholder, plan
  project communication, or build a stakeholder map — even if they just name a person
  and a project and ask "how should I handle them?". Also use it at project kickoff
  when stakeholders need to be assessed, or when the user wants profiles stored in
  or read from Dataverse / Dynamics 365.
---

# Stakeholder Intelligence Analysis

Help a project manager understand a stakeholder and communicate with them effectively.
The output is a structured stakeholder report. The PM is the decision-maker; the report
is decision support, never a verdict about a person.

**Core principle: only state what you can directly cite.** If a fact does not have a
specific source (a URL, a document, a direct PM quote, a named publication), do not
include it. Inferences from job titles, seniority, industry norms, or general
assumptions are not permitted. When information is not found, say so explicitly —
"no public information found" is a more valuable and honest output than a plausible
guess.

## Step 1: Gather data

Collect from these sources, in order of preference:

1. **Dataverse / Dynamics 365 connector** (if connected) — see "Data model" below:
   - Find the **contact** (the person) by name and account; read job title,
     account (`parentcustomerid`) and activities (emails, meetings, phone calls)
     regarding the contact.
   - Find the **project** (`new_project`) and its account.
   - Find the person's **Project Stakeholder** rows on *other* projects. If one has
     a report note, read the newest one: it is a starting point, not a source.
     Re-verify every fact you carry over and cite its original source; drop
     anything you cannot re-cite.
   Use `describe` on a table before querying it; column names may have changed.
2. **What the user provides**: pasted notes, meeting impressions, a CV or bio the
   stakeholder shared themselves, org charts.
3. **Public, professional information** via web search. Work through these source
   types — search only for professional context:
   - **Company website**: culture, products, values, strategic priorities, team pages
   - **Google search on the person**: news mentions, conference appearances,
     publications, quotes, event participation
   - **LinkedIn/XING snippets in search results**: job title, employer, sometimes a
     summary line — use only what appears in the search result snippet itself
     (XING is common in the DACH region and sometimes more openly indexed)
   - **News articles and interviews**: direct quotes, named experience, stated opinions
   - **Wikipedia**: company and industry background
   - **YouTube / social media**: public statements, talks, professional posts
   - **Company registers**: ownership, founding data (Austria: firmenabc.at /
     wirtschaft.at / evi.gv.at)
   - **Conference and event listings**: confirmed speaker or participant appearances

Do NOT open, fetch, or scrape LinkedIn or XING profile pages — these platforms
prohibit third-party profile access and it creates GDPR exposure for the user's
organization. Search-result snippets are acceptable; navigating to the profile is not.
If the user wants full profile data, ask them to paste information the stakeholder
has shared themselves. Cite the source URL or name for every fact taken from public
sources.

If data is thin after exhausting all source types, say so explicitly and ask the user
2–3 targeted questions (e.g., "How does this person typically react to schedule
changes?", "Who do they report to?") rather than padding the report with plausible
assumptions. PM-provided observations must be clearly labeled as such.

## Step 2: Analyze

Apply this framework. **Every judgment requires at least one directly cited piece of
evidence from Step 1.** If the evidence does not exist, omit the judgment entirely or
explicitly mark it as "not enough information" — do not substitute inference.

**Technical skill level (1–5)** relative to the project's subject matter:
1 = no exposure, 2 = aware of concepts, 3 = can discuss trade-offs, 4 = hands-on
practitioner, 5 = domain expert.

- Rate only what the evidence directly supports: a certification, a published article,
  a confirmed project, a direct quote describing work, a conference talk on the topic.
- A job title alone is never sufficient evidence for a skill rating. "Solution
  Architect" does not prove D365 expertise without a corroborating source.
- If evidence is weak or ambiguous, give a range (e.g., "3–4, one source") and cite
  the single source. If no evidence exists for a skill, do not rate it.

**Influence / interest matrix**: classify as Manage Closely (high/high),
Keep Satisfied (high influence, low interest), Keep Informed (low/high), or
Monitor (low/low). Base influence on confirmed role, confirmed budget authority, or
confirmed escalation paths. Base interest on how directly the project affects their
stated goals (from sources). If neither influence nor interest can be confirmed from
sources, state this explicitly instead of classifying.

**Strengths and support needs**: include only behaviors or capabilities with direct
source evidence (e.g., a PM observation, a quoted interview statement, a confirmed
project outcome). Frame these as observable, project-relevant behaviors — never
personality judgments, psychological assessments, or anything touching protected
characteristics (age, health, origin, beliefs, etc.). Write every sentence as if the
stakeholder will read it, because they have the legal right to (GDPR Art. 15).

**Engagement risk (low / medium / high)**: only assign a risk level if there is direct
evidence of risk factors (e.g., confirmed late-stage requirement changes in a previous
project, a PM observation of disengagement, a role with known competing priorities).
If no evidence of risk exists, state "no risk signals found in available sources"
rather than assigning a default rating.

## Step 3: Write the report

Use this exact structure. Write in English by default; use another language only if
the user explicitly asks for it.

Mark each major judgment with one of two confidence labels:
- ●●● **Evidenced** — directly cited from a named source
- ●●○ **Partial** — cited from a source but with limited detail or a single data point

Do not use ●○○ "inferred" — if something cannot reach at least ●●○ from a real
source, it does not belong in the report. State "not found" instead.

```
# Stakeholder Profile: [Name]

## Summary
Role, organization, relationship to the project (2–3 sentences, cited facts only)

## Background
Professional experience, areas of expertise, project history — verified facts only,
each with its source name and URL where available. Omit any section where no
source-backed information was found; label it "No public information found."

## Technical Skill Level
Rating [1–5] · Confidence [●●●/●●○] · Source: [name the source]
Reasoning citing only the evidence found. If no direct evidence: "Skill level not
assessable from available sources."

## Strengths
- [Strength] — Source: [cite directly]
Only include if directly supported by a source or PM input. Omit section if empty.

## Support Needs & Risk Factors
- [Factor] — Source: [cite directly]
Only include if directly supported. Omit section if empty.

## Communication Recommendation
Include only what is directly evidenced from sources or PM input:
- Stated preferences, formats, or working style from an interview or direct observation
- Communication channel or cadence if confirmed from sources
- Do's & Don'ts only if grounded in a cited source or confirmed PM observation

If no communication style information was found, state: "No communication style
information found in public sources. PM direct observation is the recommended input."

## Risk Assessment
Matrix quadrant: [Manage Closely / Keep Satisfied / Keep Informed / Monitor / Not
assessable — insufficient source data]
Engagement risk: [low/medium/high / Not assessable] · Source of risk signals: [cite]
Early-warning indicators: [only if evidenced, otherwise omit]

## Next Steps
2–4 concrete, schedulable actions for the PM — including actions to fill information
gaps identified during research.

---
Data sources: [list every source used, with URL where available] · Created: [date] ·
AI-generated decision support — to be validated by the PM
```

Save the report as a single-file HTML profile named
`Stakeholder-Profile_[LastName]_[YYYY-MM-DD].html` (markdown or docx only on request).

The report is shown to PMs inside a **Power Apps canvas app in Microsoft Teams**,
through the canvas **HTML text** control, and archived as a note attachment. That
control renders only a subset of HTML/CSS, so the same file must work in both places:

- **Inline styles only**: every style goes in a `style="…"` attribute. No `<style>`
  block, no classes, no `<script>`, no external fonts, stylesheets or images.
- **Tables for layout**: use `<table>` for side-by-side cards, chip rows and grids.
  Do not rely on CSS grid, flexbox, `position`, or media queries.
- **Web-safe fonts**: `font-family: 'Segoe UI', Arial, sans-serif` (Segoe UI is the
  Teams font).
- **No photos**: the avatar is a circle with the person's initials. Do not embed or
  link photos in stored reports (data minimisation, wrong-person risk).
- Keep the whole file under about 100 KB.

Look — light card-based design: page background #f6f5f2, white cards with 14px radius
and a 1px #e4e1da border, soft pastel chips (green = strength, blue = skill,
amber = caution, red = avoid). Sections stacked in this order:

1. **Hero card**: initials avatar; name; role · org · location; a data-confidence
   pill (High/Medium/Low based on source coverage); a row of up to 4 stat chips using
   only confirmed facts (omit a chip if the fact is not sourced).
2. **Sources Confirmed + Career Timeline** (two cards side by side): bullet list of
   every source actually used with URL where available; for sources that found
   nothing, list them as "searched — no results". Timeline table of confirmed career
   stations only — each row must cite its source.
3. **Technical Expertise & Skills card**: show only skills with direct source
   evidence. Each skill bar (a filled table cell whose width is the rating × 20%) must
   cite its source inline. If no skills are evidenced, replace this card with a
   "Skills — No source-backed information found" notice.
4. **Company Context + Risk Assessment** (two cards): facts table on the stakeholder's
   organization (cite each fact); risk card with overall-risk pill only if evidenced,
   short narrative citing sources, ✓-chips for confirmed strengths, ⚠-chips for
   confirmed cautions. Do not add chips for general role-based assumptions.
5. **Communication Style Recommendation card**: include only content backed by a
   named source or confirmed PM input. If no communication style data was found,
   say so in a single sentence and recommend the PM gather this through direct
   interaction.
6. **PM Action Items card**: 2×2 grid of concrete, checkable actions. Include items
   to actively fill information gaps (e.g., "Verify tenure via internal HR" or
   "Ask directly about project history").
7. Footer: all data sources with URLs, creation date, "AI-generated decision support
   — to be validated by the PM", GDPR Art. 15 note.

## Step 4: Store in Dataverse

### Data model

```
Account ─1:N─► Contact (OOB — the person)
   └─1:N─► Project (new_project) ─1:N─► Project Stakeholder (new_stakeholders) ◄─N:1─ Contact
                                             └─ Notes (annotation): one dated HTML report per run
```

- **Contact** is the person: identity only (name, job title, account, email). Never
  write profile content (assessments, do/avoid, warnings) onto the contact.
- **Project Stakeholder** (logical name `new_stakeholders`) is one row per person per
  project. It holds the structured, filterable fields the canvas app lists and sorts
  by: contact, project, role in project, matrix quadrant, engagement risk,
  confidence, report date, and the latest report HTML (multiline text) for display.
- **Notes** attached to the Project Stakeholder row hold the report history: one
  note per research run, never overwritten.

### How to store

Only store when the user asks to. Before the first write in a session, confirm the
contact, the project, and that the PM wants the profile persisted.

1. **Contact**: use the existing contact. If none exists, propose creating one
   (name, job title, account only) and wait for the PM's confirmation.
2. **Project Stakeholder**: look for a row with this contact and this project. Update
   it if it exists; otherwise create it. Set the structured fields from the report.
   Columns: `new_name` ("[Full Name] – [Project]"), `new_contact`,
   `new_projectlookup`, `new_roleinproject`, `new_quadrant`, `new_engagementrisk`,
   `new_confidencelevel` (High/Medium/Low), `new_reportdate`, `new_reporthtml`.
   If the quadrant or risk is not assessable, use the "Not assessable" option if the
   column has one, otherwise leave it empty — never pick a value to satisfy a
   required column; if the write fails for that reason, tell the PM instead of
   guessing. Put the full report HTML into `new_reporthtml`.
3. **Note**: always create a new note on the Project Stakeholder row:
   subject `Stakeholder Profile – [Full Name] – [YYYY-MM-DD]`, filename
   `Stakeholder-Profile_[LastName]_[YYYY-MM-DD].html`, mimetype `text/html`, and the
   HTML base64-encoded as the document body.

Run `describe` on `new_stakeholders` and `annotation` before writing; use the logical
column names it returns and numeric values for choice columns.

## Guardrails

- **No inference rule**: every claim in the report traces to a directly cited source.
  Role titles, seniority level, industry norms, and general assumptions are not
  evidence. If something is not found, write "not found" — never substitute a
  reasonable guess.
- **Empty sections are valid**: it is correct and honest to produce a profile where
  several sections say "No information found." A short, accurate profile is more
  useful than a long speculative one.
- This profiling falls under GDPR. Remind the user (once per session, briefly) that
  stakeholders have access rights to such records and that their DPO should sign off
  before profiles are stored systematically.
- Refuse requests to assess health, personality disorders, private life, or protected
  characteristics, and offer the project-relevant reframing instead.
- For multiple stakeholders, produce one report per person plus a one-page overview
  matrix (name × quadrant × risk × key recommendation) — only populated cells that
  are evidenced.
