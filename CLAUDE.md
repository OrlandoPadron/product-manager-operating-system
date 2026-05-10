# Product Operating System — Session Guide

> This file is loaded automatically into every Claude Code session in this repo. Read it once at the start of each session to orient.

## What this repo is
A **Product Operating System** for a single product manager. Skills + structured context that produce PM artifacts (PRDs, tickets, changelogs, announcements, retros, slides) in the user's voice, faster than from scratch.

Full description in [README.md](README.md).

## Quick orientation

The OS has six layers. When the user invokes a skill, you should consider which of these the skill needs to read:

| Layer | What it holds | Read it when |
|---|---|---|
| `identity/` | who the user is, voice profile, examples | always — every skill needs voice |
| `company/` | where they work, OKRs, stakeholders, procedures | when output needs company context |
| `products/<x>/` | a specific product, its roadmap, its releases | when work is product-scoped |
| `workflows/` | end-to-end processes the user follows | when navigating between steps |
| `templates/` | scaffolds for generated artifacts | when producing structured output |
| `knowledge.md` | cross-cutting learnings | when prior context might apply |

## First-run check

If the user invokes a skill but `config.md` still has placeholder values (look for `<placeholder>` strings or `# Run /setup to fill this in`), prompt them gently:

> "Looks like you haven't run `/setup` yet — want to do that first? It only takes a few minutes and skills will produce much better output once your identity and company are loaded."

If they want to proceed without setup, that's fine — skills should still work but flag what's missing.

## Common starting points

| User wants to... | Run |
|---|---|
| Configure the OS for the first time | `/setup` |
| Understand how the OS works | `/tour` |
| Add a new product | `/new-product` |
| Start a new release | `/new-release <product> <version>` |
| Add a key stakeholder | `/new-stakeholder` |
| Capture a learning | `/add-knowledge` |
| Draft a PRD | `/write-prd` |
| Teach the OS a new recurring action | `/new-action` |

## Conventions every skill follows

1. **Read the voice layer first.** Always load `identity/voice-profile.md` and any relevant `identity/examples/<type>/` before generating content.
2. **Read `config.md`** for output language and other preferences.
3. **Coached creation, not blank scaffolds.** `/new-*` skills propose → critique → refine → save. They don't produce empty stubs.
4. **Drafts to `workspace/` first.** Skills write drafts to `workspace/drafts/` (or `workspace/`) and only commit to canonical locations after the user explicitly approves.
5. **Ask, don't fabricate.** If context is missing (no KPI defined, scope unclear, no voice profile), the skill asks the user instead of guessing.
6. **End with the canonical path.** When a skill saves, it tells the user where: `Saved to <path>`.
7. **Suggest the next action.** When relevant, end with a one-liner pointing at what's typically next.
8. **Append to CHANGELOG.** When a skill saves a meaningful artifact, it appends a one-line entry to `CHANGELOG.md` with date, file path, and a brief summary.

Full conventions in [docs/conventions.md](docs/conventions.md). Glossary in [docs/glossary.md](docs/glossary.md).

## What NOT to do

- **Do not invent stakeholders, OKRs, KPIs, or company context.** If they're not in the files, ask.
- **Do not save to `products/<x>/` directly.** Always go through `workspace/` and confirm.
- **Do not edit `_template/` files when the user asks for something — those are the shareable placeholders.** Edit live folders instead.
- **Do not assume a fixed workflow.** Read `workflows/<name>.md` to know the actual sequence the user follows.
