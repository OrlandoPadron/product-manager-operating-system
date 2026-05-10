---
name: new-release
description: Coached creation of a new release for an existing product. Scaffolds the release folder with all standard artifacts (PRD, tickets, announcement, teams-post, changelog, release-review) and writes a release about.md with metadata, stakeholders, scope summary, target date.
---

# /new-release

## When to use
- Starting work on a new release for a product
- Setting up a release scaffold so subsequent skills (`/write-prd`, `/write-jira-tickets`, etc.) have a place to write to

## What this skill does
Creates `products/<product>/releases/<version>/` with:
- `about.md` — release metadata (stakeholders, target date, scope summary, status)
- Empty placeholder files: `prd.md`, `announcement.md`, `teams-post.md`, `changelog.md`, `release-review.md`
- Empty `tickets/` folder

The release `about.md` is filled in via coached interview. The other files are scaffolded as `<run /skill-name to fill in>` placeholders, ready to be populated by their respective skills.

## Required inputs
- **Product slug** — argument or asked interactively. Must match an existing product folder.
- **Version** — argument or asked. Free-form (e.g., `v1.2.0`, `2026-q2`, `discovery-launch`). Lowercased, hyphenated for the folder.

## Behavior

### 1. Validate context

- Check `products/<product>/` exists. If not: *"Product `<product>` doesn't exist. Want to create it first via `/new-product`?"*
- Check `products/<product>/releases/<version>/` doesn't already exist. If it does: *"Release already exists. Did you mean to refine it? (yes → evolve mode / no → cancel)"*
- Read `identity/voice-profile.md` and warn if empty.
- Read `products/<product>/about.md` to ground the release in product context.

### 2. Gather release basics

- *"What's this release about in one sentence? (Will become the TL;DR throughout the release's artifacts.)"*
- *"What's the scope — list the 2–5 biggest things shipping in this release."*
- *"What's the target date or window?"*
- *"What's the release status right now? (Discovery / Scoping / In dev / In review / Ready / Released / Archived)"*
- *"Is this release tied to a specific company OKR or KPI? If so, which?"*

### 3. Stakeholders

- Read `products/<product>/about.md` frontmatter for product-level stakeholders. *"The product has these stakeholders: [list]. Are all of them involved in this release, or is the cast different?"*
- For release-specific stakeholders not in the product list: read `company/stakeholders/` and offer existing profiles or chain into `/new-stakeholder`.
- For each selected: *"What role on this release? (approver / reviewer / sponsor / design-partner / etc.)"*

### 4. Workflow

- Read `workflows/`. *"Is this release following a specific workflow? (default: discovery-to-release.md)"*
- If user picks one, store reference in release `about.md` frontmatter so subsequent skills can read the workflow context.

### 5. Draft `about.md`

Generate the release `about.md`:

```yaml
---
product: {{product-slug}}
version: {{version}}
title: {{release_title}}
status: {{status}}
target_date: {{target_date}}
workflow: {{workflow_file}}
stakeholders:
  - name: {{stakeholder-slug}}
    role: {{role}}
related_okr: {{okr_or_null}}
---

# {{release_title}}

## Scope summary
{{scope_summary}}

## Target date
{{target_date_with_context}}

## Status
{{status_with_notes}}

## Stakeholders
{{stakeholder_table_with_roles}}

## Notes
{{free_form_notes}}
```

Show full draft. Iterate until user approves.

### 6. Scaffold the release folder

Create:
```
products/<product>/releases/<version>/
├── about.md              # the drafted content
├── prd.md                # placeholder: "Run /write-prd <product> <version> to fill this in."
├── tickets/              # empty folder with .gitkeep
├── announcement.md       # placeholder
├── teams-post.md         # placeholder
├── changelog.md          # placeholder
└── release-review.md     # placeholder
```

Each placeholder file is a one-line stub pointing at the skill that populates it.

### 7. Confirm and offer chain

*"✓ Created release at `products/<product>/releases/<version>/`.

Scaffolded files:
- `about.md` — release metadata (saved)
- `prd.md` — empty, run `/write-prd <product> <version>` to populate
- `tickets/` — empty folder
- `announcement.md`, `teams-post.md`, `changelog.md`, `release-review.md` — all empty placeholders

Want to start the workflow now? Based on `workflows/<workflow>.md`, the first step is typically `/write-prd <product> <version>`."*

Append to `CHANGELOG.md`:
```
- `products/<product>/releases/<version>/` — Created release scaffold *(via /new-release)*
```

### 8. Evolve mode (if release exists and user confirmed)

- Read existing `about.md`.
- *"What about this release do you want to update? (status / scope / stakeholders / target date / all)"*
- Update only the requested sections. Preserve existing content elsewhere.
- Diff before saving.

## Save location
- Canonical: `products/<product>/releases/<version>/`
- Release `about.md` is iterated in conversation before saving — no separate draft step needed for the metadata file
- Other files are stub placeholders pointing at their respective skills

## Missing context hints
- Product doesn't exist → offer `/new-product`
- Voice profile empty → hint
- Workflow file referenced doesn't exist → fall back to `discovery-to-release.md` and note this

## Idempotency
- Existing release → evolve mode
- Partial scaffolds → checks each expected file and creates missing ones without touching existing ones

## Naming convention
- Folder name: lowercase, hyphenated. `v1.2.0` stays as `v1.2.0`. `2026 Q2` → `2026-q2`.
- Display title preserved in frontmatter `title:`.

## Next action hint
Almost always the next step is `/write-prd <product> <version>`. Suggest it explicitly.
