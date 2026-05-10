<!--
PRD template — consumed by /write-prd.
{{placeholders}} are filled in by the skill.
HTML comments are guidance for the skill and stripped from final output.
Voice and tone come from identity/voice-profile.md, not from this template.
-->

# {{release_title}}

<!-- One-line summary of what this release does and who it's for. Avoid jargon. -->
**TL;DR:** {{tldr}}

---

## Context

<!-- What's the situation that prompted this work? Reference relevant company OKRs/priorities, customer signal, or strategic shift. Keep this brief — the reader should understand "why now" in two paragraphs. -->

{{context}}

## Problem

<!-- One-paragraph problem statement. Concrete, not abstract. The thing that gets worse if we don't ship this. -->

{{problem}}

### Who's affected
{{affected_users}}

### Evidence
<!-- Customer quotes, support tickets, metrics, research findings. Cite sources. -->
{{evidence}}

## Goals

<!-- What outcomes does this release produce? State as outcomes (not outputs). Tie to KPIs where possible. -->

- {{goal_1}}
- {{goal_2}}
- {{goal_3}}

### Non-goals
<!-- Explicitly out of scope. Helps reviewers focus their feedback. -->
- {{non_goal_1}}
- {{non_goal_2}}

## Success metrics

<!-- How we'll know this worked. Reference product KPIs from products/<x>/about.md. If no KPI exists for what we're trying to measure, ask the user — don't invent. -->

| Metric | Current | Target | Measurement |
|---|---|---|---|
| {{metric_1}} | {{current_1}} | {{target_1}} | {{measurement_1}} |

## Solution

<!-- What we're building. Structure varies — sometimes one approach, sometimes options to choose from. Use whichever the user's voice profile prefers. -->

{{solution}}

### Scope
<!-- The specific changes in this release. -->
{{scope}}

### User flow
<!-- Step-by-step from the user's perspective. Use whatever fidelity the user typically uses (mocks, written, both). -->
{{user_flow}}

## Open questions

<!-- Things we don't know yet. Honest about uncertainty — reviewers will respect this more than false confidence. -->
- {{question_1}}
- {{question_2}}

## Stakeholders & approvals

<!-- Pulled from product about.md frontmatter. Roles: sponsor, reviewer, decision-maker, etc. -->

{{stakeholders}}

## Risks

<!-- What could go wrong. Mitigations where known. -->
- {{risk_1}}
- {{risk_2}}

## Timeline

<!-- High-level only. Detailed timeline lives in tickets and the release's about.md. -->
{{timeline}}

---

<!-- Appendix sections (research, prior decisions, alternatives considered) go below if relevant. Skill should ask whether to include them based on user's voice profile defaults. -->
