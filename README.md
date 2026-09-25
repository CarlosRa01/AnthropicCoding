# Stakeholder Intelligence Agent

Decision-support agent that helps project managers build source-backed stakeholder
profiles and communication recommendations.

## Layout

- `.claude/skills/stakeholder-intelligence/SKILL.md` — the agent's skill definition
  (data gathering, analysis framework, report format, GDPR guardrails). Claude Code
  loads it automatically when working in this repository.

## Principles

- Every claim in a profile must trace to a cited source; "not found" beats a guess.
- Profiles are GDPR-relevant personal data — stakeholders have access rights
  (Art. 15), and the DPO should sign off before profiles are stored systematically.
- LinkedIn/XING profile pages are never scraped; search-result snippets only.
