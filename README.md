# Product Operating System

A personal operating system for product managers — a structured set of context files and Claude Code skills that turn your repeating PM work (PRDs, tickets, changelogs, announcements, retros, slides) into one-command operations that sound like *you*.

## What it does

Loads your identity, your company, your products, and your past writing into a coherent context that skills draw from. Instead of starting from a blank page every time, you run `/write-prd` or `/draft-teams-post` and get a draft that already knows your voice, your stakeholders, and your stage.

Goal: cut an 8-hour PM workday down to the 2–4 hours that actually require your judgment.

## How it's organized

```
.
├── identity/              # Who you are: voice, values, examples of your writing
├── company/               # Where you work: OKRs, KPIs, stakeholders, procedures
├── products/              # What you ship: per-product about, roadmap, releases
├── workflows/             # Your end-to-end processes (e.g., discovery → release)
├── templates/             # Scaffolds skills consume
├── knowledge.md           # Cross-cutting things you've learned
├── workspace/             # Drafts and works-in-progress
├── _template/             # Placeholder content for sharing the OS
├── docs/                  # Glossary, conventions
└── .claude/skills/        # The actual skills you invoke
```

Each folder has its own README explaining what lives there, when to add to it, and which skills consume it.

## Getting started

If this is your first time:

1. Run `/setup` — interactive walkthrough that fills in your identity, company, language preferences, and creates your first product
2. Run `/tour` — 5-minute conceptual walkthrough of how the OS thinks
3. Try `/write-prd` on something real to see the system in action

If you're returning, just invoke the skill you need. `CLAUDE.md` keeps a one-line cheatsheet at the top of every session.

## Skills

### Setup & navigation
- **`/setup`** — first-time interactive configuration (modular: identity, company, product, language, workflow)
- **`/tour`** — guided walkthrough of how the OS works

### Creation (coached)
- **`/new-product`** — add a new product to your portfolio
- **`/new-release`** — start a new release for an existing product
- **`/new-stakeholder`** — add a stakeholder profile, optionally linked to products/releases
- **`/link-stakeholder`** — fast path to link an existing stakeholder
- **`/new-action`** — teach the OS a new recurring action (creates a new skill)

### Knowledge
- **`/add-knowledge`** — capture something you've learned, classified into the right home

### Generation
- **`/write-prd`** — draft a PRD for a release, in your voice
- **`/write-jira-tickets`** — break a PRD into JIRA tickets, one file per ticket
- **`/generate-changelog`** — synthesize the release changelog from PRD + tickets (audience-aware: customer / internal / technical)
- **`/draft-announcement`** — draft the customer-facing announcement (channel-aware: email / in-app / blog / social)
- **`/generate-release-review`** — generate a PPTX release review deck from tickets + PRD, coached and styled from your own template

### Planning
- **`/plan-roadmap`** — coached roadmap planning, anchored to OKRs and KPIs

More skills will land in the second slice (`/draft-teams-post`, `/draft-release-review`, `/new-template`, `/new-workflow`).

## Conventions

The system has a few opinionated patterns. Read [docs/conventions.md](docs/conventions.md) once and you'll understand any skill at a glance:

- **Coached creation**: `/new-*` skills propose → critique → refine → save. They never produce blank scaffolds.
- **Voice everywhere**: every skill reads `identity/voice-profile.md` and relevant `identity/examples/`.
- **Drafts go to `workspace/` first**: nothing lands in canonical locations without your approval.
- **Forward links only**: products and releases reference stakeholders, not the other way around.
- **Ask, don't fabricate**: if a skill needs something it can't find, it asks you instead of inventing.

## Glossary

The OS overloads some common words. See [docs/glossary.md](docs/glossary.md) for the precise meaning of *skill*, *action*, *workflow*, *template*, *coached creation*, etc.

## Sharing this OS

The repo is built as a template. To share it with someone at another company:

1. They clone or fork
2. They run `/setup` — fills in their own identity, company, products
3. The shareable template only contains `_template/` placeholders, skills, and docs. Your company-specific content lives in folders that are gitignored by default.

See [SETUP.md](SETUP.md) for the full first-run walkthrough.
