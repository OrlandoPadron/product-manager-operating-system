# Glossary

The Product Operating System overloads some common words. This page locks down what each term means inside this OS so the system stays unambiguous.

## Core terms

### Skill
An invokable Claude Code action — a markdown file in `.claude/skills/<name>/SKILL.md` with instructions Claude follows when you type `/<name>`. Skills are the *executable* part of the OS.

### Action
Synonym for skill. The user-facing word the OS uses (since "skill" is a Claude Code term and "action" is more natural in product-speak). When you read "action" in this OS, it means "skill."

### Workflow
A *sequence* of skills representing an end-to-end process (e.g., discovery → release). Lives in `workflows/<name>.md`. Workflows are *documentation*, not orchestration — they describe the order, the user invokes each skill in turn.

### Template
A reusable scaffold for a specific kind of artifact — a PRD shape, a ticket shape, a slide-deck shape. Lives in `templates/<name>.md`. Skills consume templates when generating content.

### Stakeholder profile
A markdown file in `company/stakeholders/<name>.md` capturing how to communicate effectively with a specific person. Skills load these when the audience is named.

### Coached creation
The pattern every `/new-*` skill follows: **propose → critique → refine → save**. Instead of producing a blank file, the skill asks questions, drafts content, lets the user iterate, and only then writes. Always interactive.

### Content generation
The pattern skills like `/write-prd` and `/draft-teams-post` follow: read context (identity, company, product), apply a template, generate a draft, save to `workspace/`, ask before committing to canonical location.

### Quick-link
The pattern `/link-stakeholder` follows: take a name and one or more product/release targets, write the link into the targets' frontmatter, no profile editing. Fast path for the common case.

### Evolve mode
What `/new-action` (and similar) does when invoked on something that already exists: detects the existing artifact, shows current state, asks what to change, regenerates, diffs, and only saves on approval.

## Layers

### Identity
Files about *who you are*. `identity/`. Voice profile, examples, values, about-me. Most important context — every skill reads from it.

### Company
Files about *where you work*. `company/`. OKRs, KPIs, teams, tools, procedures, glossary, stakeholders. Gitignored by default.

### Products
Files about *what you ship*. `products/<x>/`. About, roadmap, releases. Each release is a folder with PRD, tickets, announcements, etc.

### Knowledge
Cross-cutting things you've learned. Single file `knowledge.md` (until it grows enough to split). Captured via `/add-knowledge`.

### Templates
Scaffolds skills consume. `templates/`.

### Workflows
Your end-to-end processes. `workflows/`. User-defined, never assumed.

### Workspace
Drafts and works-in-progress. `workspace/`. Nothing is "real" until it's saved to a canonical location.

## Special locations

### `_template/`
Parallel placeholder versions of every gitignored folder. Used so the OS can be shared as a template without leaking your private content. `/setup` copies from `_template/` into the live folders on first run.

### `config.md`
System-wide preferences (language, time zone, default product, voice formality). Skills read this on every invocation.

### `CLAUDE.md`
Auto-loaded into every Claude Code session. The orientation layer — tells Claude how to behave inside this OS.

## Frontmatter conventions

### Stakeholder links
```yaml
stakeholders:
  - name: jane-smith
    role: reviewer
```
Lives in product `about.md` and release `about.md`. Forward-only references; the stakeholder file itself stays a clean profile.

### Maturity tier (informal)
OKR/KPI files express their formality in the **first line of prose**, not in frontmatter. Examples:
- "These are formal company OKRs reviewed quarterly."
- "These are informal priorities — we don't track formal OKRs."
- "Not currently tracked." (file otherwise empty)

Skills detect tier from this prose and adapt.

## Status concepts

### First-run
The state where `config.md` still has `<run /setup to fill in>` placeholders. Skills detect this and prompt the user to run `/setup` before producing serious output.

### Canonical location
The "real" home for an artifact: `products/<x>/releases/<v>/prd.md`, not `workspace/drafts/prd.md`. Coached skills always confirm the canonical path before saving.

### Sanitized vs. private
- **Sanitized** content (anonymized names, fake numbers) → committed to git, shareable as part of the template
- **Private** content (real customer names, real numbers, internal codenames) → in gitignored folders, stays local

## Naming conventions

### Skill verbs
- **`/new-*`** — create something (coached, with evolve mode for existing items)
- **`/write-*`** — generate a long-form document
- **`/draft-*`** — generate a shorter communication artifact
- **`/generate-*`** — generate something derived from existing content (e.g., `/generate-changelog` from PRD + tickets)
- **`/plan-*`** — produce a forward-looking plan
- **`/link-*`** — connect existing entities without editing them
- **`/setup`**, **`/tour`** — system-level meta-skills
- **`/add-knowledge`** — explicit knowledge capture (special case)

### File names
- Stakeholders: `<first-last>.md` lowercase hyphenated
- Releases: `<version>/` (e.g., `v1.2.0/`, `2026-q2/` — your choice)
- Products: `<product-name>/` lowercase hyphenated
