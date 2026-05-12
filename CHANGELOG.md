# Changelog

A human-readable log of meaningful changes the OS makes to itself. Skills append a one-line entry here when they save a significant artifact (PRD, release scaffold, stakeholder profile, knowledge entry, new skill, etc.). Manual edits to context files (e.g., updating `okrs.md` by hand) don't auto-log — git history is the source of truth for those.

Format:

```
## YYYY-MM-DD
- `<file>` — <one-line summary> *(via /skill-name)*
```

## 2026-05-12
- `products/mentoring/releases/43.26.0-mentoring-in-speexx-manager/prd.md` — Converted from PDF (33 pages), full PRD with requirements, KPIs, roles & permissions *(via /new-release)*
- `products/mentoring/releases/43.26.0-mentoring-in-speexx-manager/` — Created release scaffold *(via /new-release)*
- `products/mentoring/releases/43.26.0-mentoring-in-speexx-manager/changelog.md` — Populated from release changelog *(via /new-release)*
- `products/mentoring/releases/43.26.0-mentoring-in-speexx-manager/announcement.md` — Populated from identity/examples *(via /new-release)*
- `products/mentoring/releases/43.26.0-mentoring-in-speexx-manager/tickets/jira-tickets.md` — Converted 158 tickets from .doc *(via /new-release)*
- `.claude/skills/generate-release-review/SKILL.md` — Created skill *(via /new-action)*
- `README.md` — Added /generate-release-review to Generation skill index *(via /new-action)*

## 2026-05-11
- `company/about.md` — Speexx company overview, solutions, and strategic focus *(via /setup)*
- `company/teams.md` — org structure, reporting line, cross-functional partners *(via /setup)*
- `company/tools.md` — toolstack (Jira, Confluence, Teams, Figma, MS Office) *(via /setup)*
- `company/okrs.md` — OKR structure scaffolded (values to fill in) *(via /setup)*
- `company/kpis.md` — KPI structure scaffolded with Mentoring KPIs as reference *(via /setup)*
- `company/procedures.md` — release process, scope change and comms conventions *(via /setup)*
- `company/glossary.md` — full Speexx internal glossary (products, acronyms, domain concepts) *(via /setup)*
- `products/mentoring/about.md` — Speexx Mentoring product brief, vision, audience, KPIs *(via /setup)*
- `products/mentoring/roadmap.md` — Mentoring roadmap scaffold *(via /setup)*
- `products/cms/about.md` — Speexx CMS / Content Tool product brief, Dacapo deprecation context *(via /setup)*
- `products/cms/roadmap.md` — CMS roadmap scaffold with known deferred backlog *(via /setup)*
- `config.md` — default_product set to mentoring *(via /setup)*

## 2026-05-11
- `identity/about-me.md` — filled in role, responsibilities, focus areas, working style, background *(via /setup)*
- `identity/values.md` — filled in professional values, principles, hard lines, flex areas *(via /setup)*
- `identity/voice-profile.md` — generated voice profile from writing samples *(via /setup)*

## 2026-05-10
- `config.md` — set output language (en), voice formality (professional-casual), time zone (Europe/Madrid) *(via /setup)*

Most recent entries on top.

---

## 2026-05-10
- Initial scaffolding of the Product Operating System (folder structure, root files, templates, first slice of skills).
- Added `/write-jira-tickets`, `/generate-changelog`, `/draft-announcement`, `/plan-roadmap` skills (pulled forward from slice 2). README and CLAUDE.md skill indexes updated.
- Added custom-template override pattern. `/generate-changelog`, `/draft-announcement`, and `/plan-roadmap` now check `templates/` for user-provided overrides before falling back to inline defaults. Pattern documented as convention 15 in `docs/conventions.md` and indexed in `templates/README.md`.
