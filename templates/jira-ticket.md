<!--
JIRA ticket template — consumed by /write-jira-tickets.
One file per ticket, named <PROJECT-KEY>.md (e.g., PROJ-123.md).
The skill writes one of these per concrete unit of work derived from the PRD.
-->

---
key: {{ticket_key}}
title: {{ticket_title}}
type: {{ticket_type}}              # e.g., Story, Bug, Task, Spike
parent: {{epic_or_parent}}         # Epic key if applicable
release: {{release_version}}
status: To Do
assignee: {{assignee_or_unassigned}}
estimate: {{estimate_or_tbd}}
labels: []
---

# {{ticket_title}}

## Summary
<!-- One sentence. What this ticket delivers. -->
{{summary}}

## Context
<!-- Why this ticket exists. Link back to PRD section if relevant. -->
{{context}}

## Acceptance criteria
<!-- Testable conditions. Use Gherkin style if user's examples do, otherwise bullets. -->
- [ ] {{criterion_1}}
- [ ] {{criterion_2}}

## Technical notes
<!-- Anything dev needs to know. Skill should leave this minimal unless the user's example tickets show heavy technical context. -->
{{technical_notes}}

## Out of scope
<!-- Explicit non-goals for this ticket — prevents scope creep during dev. -->
- {{out_of_scope}}

## Dependencies
<!-- Other tickets, external teams, design files, etc. -->
- {{dependency_1}}

## Definition of done
<!-- Project-wide DoD goes in company/procedures.md; ticket-specific additions go here. -->
{{definition_of_done}}
