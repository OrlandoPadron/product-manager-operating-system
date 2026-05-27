# Conventions

Patterns every skill in this OS follows. If you understand these, you understand any skill.

## 1. Read the voice layer first

Every skill — without exception — loads:
- `identity/voice-profile.md`
- `config.md` (for language, formality, defaults)
- The relevant `identity/examples/<type>/` folder

Skills produce voice-faithful output by default. If `voice-profile.md` is empty, they hint: *"Run `/setup identity` to make output sound more like you."*

## 2. Coached creation, not blank scaffolds

`/new-*` skills don't produce empty stubs. They:

1. **Propose** — ask the user enough questions to draft something meaningful
2. **Critique** — show the draft and explicitly invite changes
3. **Refine** — incorporate feedback, regenerate
4. **Save** — write to canonical location with explicit confirmation

The user never sees a blank `about.md` waiting to be filled. They see a populated draft they can edit, accept, or replace.

## 3. Drafts go to `workspace/` first

Content-generating skills (`/write-prd`, `/draft-teams-post`, etc.) save drafts to `workspace/` (or `workspace/drafts/`) and ask the user to confirm before committing to canonical locations like `products/<x>/releases/<v>/prd.md`.

This means:
- Nothing in `products/` is ever "in progress" — it's always finalized
- The user can iterate freely without polluting the canonical structure
- Discarded drafts can be deleted from `workspace/` without consequence

## 4. Forward links only

When a stakeholder is connected to a product or release, the link lives in the **product's or release's** frontmatter:

```yaml
# products/<x>/about.md
---
stakeholders:
  - name: jane-smith
    role: reviewer
---
```

The stakeholder file itself stays a clean profile. This avoids drift (one source of truth per relationship) and keeps profiles reusable across multiple products.

## 5. Ask, don't fabricate

When a skill needs context it can't find — no KPI defined, no stakeholder profile for a named person, scope unclear — it **asks the user** rather than making something up.

Bad: *"Increased conversion by 15%."* (where did that number come from?)
Good: *"I don't see a conversion metric in the product's KPIs. What's the relevant number?"*

## 6. End with the canonical path

When a skill saves something, it tells the user exactly where:

> ✓ Saved PRD to `products/checkout-v2/releases/v1.2.0/prd.md`

No silent writes. No ambiguity about where things live.

## 7. Suggest the next action

When a skill finishes and there's an obvious next step in the workflow, the response includes a one-liner:

> Next in this workflow: `/write-jira-tickets checkout-v2 v1.2.0`

The OS reads `workflows/<active>.md` to know what's typical. The user can follow the suggestion or do something different.

## 8. Append to CHANGELOG

When a skill saves a meaningful artifact (PRD, release scaffold, stakeholder profile, knowledge entry, new skill), it appends a one-line entry to `CHANGELOG.md`:

```
## 2026-05-10
- `products/checkout-v2/releases/v1.2.0/prd.md` — Initial PRD draft *(via /write-prd)*
```

Manual edits to context files (e.g., updating `okrs.md` by hand) don't auto-log. Git is the source of truth for those.

## 9. Detect first-run state and warn

If `config.md` still has `<run /setup to fill in>` placeholders, skills should prompt:

> *"Looks like you haven't run `/setup` yet. Skills will work but the output won't sound like you and won't have your company context. Want to run `/setup` now?"*

If the user declines, proceed but flag what's missing.

## 10. Idempotent skills

Skills that modify state (like `/setup`) should be safe to re-run:
- Detect existing values
- Ask before overwriting
- Allow skipping modules that are already done

This means the user never has to remember "have I done this yet?" — just re-run.

## 11. Read workflows for navigation, not enforcement

`workflows/<name>.md` describes the user's typical sequence. Skills *consult* it to suggest next actions, but never *force* a sequence. The user might run skills out of order, skip steps, or invent new ones — the OS adapts.

## 12. Frontmatter is for machine-readable fields only

If a field is for skills to parse (stakeholder lists, status tags, dates), put it in YAML frontmatter. If it's for humans to read (level of formality, narrative context), put it in the first line of prose. Don't put narrative in frontmatter or status flags in prose.

## 13. Small files, focused purpose

Each markdown file in the OS has a single, clear purpose. When a file grows too large or covers multiple unrelated topics, split it. `/add-knowledge` will offer to split `knowledge.md` if it grows past ~100 entries.

## 14. The `_template/` folder is for sharing, not editing

`_template/` contains placeholder versions of every gitignored file. It's used by `/setup` on first run to populate the live folders. **Don't edit `_template/` to change the OS** — edit live folders. `_template/` only changes when the OS itself is updated.

## 15. Custom templates override defaults — file presence is the configuration

Skills with structural output check `templates/` for a user-provided file before using their inline default. The override pattern:

- **Single-variant skills** check `templates/<artifact>.md` (e.g., `templates/teams-post.md`, `templates/roadmap.md`)
- **Multi-variant skills** check `templates/<artifact>-<variant>.md` (e.g., `templates/changelog-customer.md`, `templates/announcement-email.md`)

If the file exists, the skill maps generated content to its structure. If missing, the skill uses its inline default. Drop the file → override active. Delete it → defaults restored. **Zero settings, no configuration syntax — file presence *is* the configuration.**

What templates control vs. don't:
- Templates control **structure only** — section ordering, frontmatter shape, headings, what fields exist
- **Voice and tone always come from `identity/voice-profile.md` and `identity/examples/<type>/`**, never from the template
- Skills must respect the template's structure even if it differs from their inline default

New skills built via `/new-action` should follow this pattern automatically: check `templates/<name>[-<variant>].md`, fall back to inline default.

### Skills currently supporting custom templates

| Skill | Recognized template(s) |
|---|---|
| `/write-prd` | `templates/prd.md` |
| `/write-jira-tickets` | `templates/jira-ticket.md` |
| `/generate-changelog` | `templates/changelog-{customer,internal,developer}.md` |
| `/draft-announcement` | `templates/announcement-{email,in-app,blog,social}.md` |
| `/plan-roadmap` | `templates/roadmap.md` |
| `/new-stakeholder` | `templates/stakeholder.md` |
| `/new-action` | `templates/skill-scaffold.md` |
| `/new-workflow` *(slice 2)* | `templates/workflow-definition.md` |
| `/draft-release-review` *(slice 2)* | `templates/release-review-slides.md` |
| `/draft-teams-post` *(slice 2)* | `templates/teams-post.md` |

## 16. Auto-load brand for visual artifacts

If `company/brand/design-system-reference.md` exists, any skill that produces a **visual artifact** must load it automatically before generating output. The user is not asked — brand is on by default.

**What counts as a visual artifact:**
- Slide decks and release reviews (`/generate-release-review`)
- Figma frames, mockups, or any output routed through the Figma MCP
- Announcements that include layout or design direction (`/draft-announcement`)
- Any ad-hoc request for a prototype, wireframe, mockup, or visual spec

**What to do with the brand files:**
1. Load `company/brand/design-system-reference.md` as the quick-reference token set
2. When generating copy, check `company/brand/voice-and-messaging.md` for brand-specific language
3. When specifying colors or type, use the exact values from `company/brand/colors.md` and `company/brand/typography.md`
4. Surface the applied brand tokens to the user: *"Using Speexx brand: #FF7900 orange, Roboto Light, near-black headings."*

**Override:**
If the user says *"ignore brand"*, *"use my own colors"*, or specifies explicit values, defer to them for that artifact only. Do not persist the override.

**Detection:**
```
exists: company/brand/design-system-reference.md → brand is configured → auto-load
missing → no brand layer → proceed without it, no warning needed
```

**Skills that follow this rule:**

| Skill | What it uses from brand |
|---|---|
| `/generate-release-review` | Colors for section titles/highlights; typography for slide headings |
| `/draft-announcement` | Brand voice from `voice-and-messaging.md`; color/font tokens for visual versions |
| Figma MCP skills | Full palette + type scale + logo/layout rules |
| Future `/create-mockup`, `/prototype` | Full design system reference |
