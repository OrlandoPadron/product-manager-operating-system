# Changelog

A human-readable log of meaningful changes the OS makes to itself. Skills append a one-line entry here when they save a significant artifact (PRD, release scaffold, stakeholder profile, knowledge entry, new skill, etc.). Manual edits to context files (e.g., updating `okrs.md` by hand) don't auto-log — git history is the source of truth for those.

Format:

```
## YYYY-MM-DD
- `<file>` — <one-line summary> *(via /skill-name)*
```

Most recent entries on top.

---

## 2026-05-10
- Initial scaffolding of the Product Operating System (folder structure, root files, templates, first slice of skills).
- Added `/write-jira-tickets`, `/generate-changelog`, `/draft-announcement`, `/plan-roadmap` skills (pulled forward from slice 2). README and CLAUDE.md skill indexes updated.
- Added custom-template override pattern. `/generate-changelog`, `/draft-announcement`, and `/plan-roadmap` now check `templates/` for user-provided overrides before falling back to inline defaults. Pattern documented as convention 15 in `docs/conventions.md` and indexed in `templates/README.md`.
