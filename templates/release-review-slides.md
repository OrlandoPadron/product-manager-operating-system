<!--
Release review slide deck — consumed by /draft-release-review.
Format: markdown with `---` separating slides.
Exportable to PPTX/PDF later via Marp, Pandoc, or similar.
The skill should follow the structure of examples in identity/examples/release-reviews/
when those exist; this template is the fallback structure.
-->

---
title: {{release_title}} — Release Review
date: {{review_date}}
audience: {{audience}}
presenter: {{presenter}}
---

# {{release_title}}
## Release Review

{{presenter}} · {{review_date}}

---

## What we shipped

{{one_line_summary}}

---

## The context

<!-- Why we did this. Tie to OKRs / priorities / customer signal. -->
{{context_bullets}}

---

## What's in the release

<!-- Higher-level than tickets. Group into 3–5 themes. -->
- {{theme_1}}
- {{theme_2}}
- {{theme_3}}

---

## Highlights

<!-- The 1–2 things you're most proud of or that delivered the most value. Use specifics, not adjectives. -->

{{highlight_1}}

---

## Metrics

<!-- Pull from products/<x>/about.md KPIs. Show movement, not just current state. -->

| Metric | Before | After | Δ |
|---|---|---|---|
| {{metric_1}} | {{before_1}} | {{after_1}} | {{delta_1}} |

---

## What worked

<!-- Process or product wins worth carrying forward. -->
- {{worked_1}}
- {{worked_2}}

---

## What didn't

<!-- Honest about issues. Skill should pull from any retro notes if they exist. -->
- {{didnt_work_1}}

---

## What's next

<!-- Forward-look. Tease v+1 if applicable. -->
- {{next_1}}
- {{next_2}}

---

## Thank you

<!-- Recognize the people who made it happen. Pull from release stakeholders + linked tickets' assignees. -->
{{thanks}}

---

## Questions?
