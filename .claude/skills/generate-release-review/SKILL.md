---
name: generate-release-review
description: Generate a Markdown release review presentation from a release's tickets and PRD. Asks coaching questions to shape the narrative, infers scope framing, and outputs a slide-separated .md file. Saves to workspace/ first, then commits to the release folder on approval.
---

# /generate-release-review

## When to use
After a release has shipped and before the release review meeting. Invoke with the product and version to generate the presentation deck.

Use it any time you need to present a release to your team — QA, dev, stakeholders, or the broader Speexx org.

## What this skill does
Reads the release's tickets and PRD, synthesizes the scope narrative, asks a focused set of coaching questions, then generates a `.md` file with slides separated by `---`. Each slide is a clearly structured section with title and content. The file is drafted to `workspace/` first; you confirm before it's saved to the release folder.

## Required inputs
- **Product slug** — `<product>` argument (e.g., `mentoring`, `cms`)
- **Release version** — `<version>` argument (e.g., `43.10.0-mentoring-programs`)

If either is missing:
- If `default_product` is set in `config.md` and product is omitted, use it.
- If only product is given, list available releases in `products/<product>/releases/` and ask which one.

## Context to read
- `identity/voice-profile.md` (always)
- `config.md` (always)
- `identity/examples/release-reviews/` — style reference; look for `.md` examples here
- `products/<product>/about.md` — product vision, KPIs, stakeholders
- `products/<product>/releases/<version>/` — tickets, PRD, and any existing `about.md`
- `company/glossary.md` — to correctly name Speexx-specific terms in the deck
- `company/teams.md` — for context on roles mentioned in the team slide

## Behavior

### 1. Validate context

**Release folder:**
Confirm `products/<product>/releases/<version>/` exists and contains at least one of: a tickets file (`.doc` or `.md`) or a PRD (`.pdf` or `.md`). If neither exists: *"I can't find tickets or a PRD for this release. Add them to `products/<product>/releases/<version>/` and re-run."*

### 2. Read and synthesize content

Read the release content in this order of preference:
- **Tickets:** `.md` → read directly. `.doc` → extract text via Word COM.
- **PRD:** `.md` → read directly. `.pdf` → extract text via PowerShell PDF reader or ask the user to paste key scope points.

From the content, synthesize:
- The 3–5 main scope themes (what was built)
- Whether the release *replaces something existing* (signals Before/After framing) or *adds something new* (signals "What we built" framing)
- Key capabilities delivered — concrete, specific, not adjective-heavy
- Any KPI targets or success metrics mentioned in the PRD

### 3. Draft the title line

Generate a one-line description of the release for the title slide. Aim for the format:
> *[What it does] for [who] — [one concrete benefit]*

Show it: *"Title line suggestion: '[draft]'. Keep it, or tell me what to change."*

Wait for confirmation or a replacement before continuing.

### 4. Coaching questions

Ask these in sequence, one at a time. Don't batch them.

**Q1 — Demo:**
*"Will there be a live demo during the review? (yes / no)"*
If yes → include a "Demo time" slide after the scope slide.

**Q2 — Scope framing:**
Based on the synthesis from step 2, propose the framing:
- If the release replaces something: *"This looks like a Before/After release. I'll frame the scope slide as 'What has changed?' — does that work?"*
- If it's a new feature: *"This looks like a net-new feature. I'll frame the scope slide as 'What we built' — does that work?"*
- If unclear: *"How do you want to frame the main scope slide? Before/After, 'What we built', or something else?"*

**Q3 — What's next:**
*"What's coming after this release? List the upcoming initiatives with version numbers and rough dates if you have them — I'll build the 'What's next' slide from that."*

**Q4 — The team:**
*"Who worked on this release? Give me names and roles (e.g., 'Joel González — Software Developer, Backend')."*

**Q5 — Optional slides:**
*"Do you want to include any of these optional slides? (yes / no for each)"*
- Metrics: before/after on KPIs
- What worked / What didn't: process or product reflection

If metrics: *"What numbers do you want to show? Format: [metric name] — before: [X] / after: [Y]"*
If what worked/didn't: *"What's worth calling out?"*

### 5. Draft all slide content

Before generating the file, show the full slide-by-slide content as structured text for review:

```
SLIDE 1 — Title
  Release name / one-liner / date / presenter

SLIDE 2 — [Scope frame title]
  [Content]

SLIDE 3 — Demo time  ← only if Q1 = yes

SLIDE [N] — Metrics  ← only if requested
SLIDE [N] — What worked / What didn't  ← only if requested

SLIDE [N] — What's next  ← only if Q3 has content
  [Timeline items]

SLIDE [N] — The Team
  [Name — Role]
  ...

SLIDE [N] — Thank you / Questions
```

Say: *"Here's what I'll put in the deck. Review each slide and tell me what to change. When you're happy with the content, I'll generate the file."*

Wait for confirmation or adjustments before proceeding.

### 6. Generate the Markdown

Write the `.md` file directly (no script needed). Format:

- Slides are separated by `---` on its own line
- Each slide starts with the slide title as `#` heading
- Subtitle or framing line as `##`
- Body content uses standard Markdown (bullets, bold, etc.)
- Team slide lists each person as `**Name** — Role`
- Photo placeholder omitted (names and roles are sufficient in MD format)

Save to: `workspace/release-review-<product>-<version>.md`

### 7. Confirm and save canonical

Say:
*"✓ Generated: `workspace/release-review-<product>-<version>.md`*
*Move to `products/<product>/releases/<version>/release-review.md`? (yes / no)"*

If yes: copy the file to the canonical path.

Append to `CHANGELOG.md`:
```
- `products/<product>/releases/<version>/release-review.md` — Release review deck generated *(via /generate-release-review)*
```

### 8. Suggest next action

*"Deck saved. You're ready for the review meeting."*

If the release doesn't yet have an announcement drafted:
*"Next up: `/draft-announcement <product> <version>` to write the release communication."*

## Save location
- Draft: `workspace/release-review-<product>-<version>.md`
- Canonical: `products/<product>/releases/<version>/release-review.md`

## Markdown slide format

```markdown
# Release Name
## One-liner subtitle

*Date · Presenter*

---

# What we built
## Scope of the release

- **Feature 1** — Description
- **Feature 2** — Description

---

# Demo time

*Live demo*

---

# What worked. What didn't.
## A release of two halves

**What worked**
- Item
- Item

**What didn't**
- Item
- Item

---

# What's next

| Version | Initiative | Date |
|---|---|---|
| v1.x | ... | Q3 2026 |

---

# The Team

**Name** — Role
**Name** — Role

---

# Thank you

*Questions?*
```

## Missing context hints
- `identity/voice-profile.md` is empty → *"Run `/setup identity` to make output sound like you."*
- No tickets or PRD in the release folder → *"Add tickets (`.md` or `.doc`) and/or a PRD (`.md` or `.pdf`) to `products/<product>/releases/<version>/` before running this skill."*
- KPIs are TBC in the PRD → ask the user for actual numbers before generating the metrics slide; don't fabricate baselines or targets

## Idempotency
- If a `release-review.md` already exists at the canonical path: *"A deck already exists at `products/<product>/releases/<version>/release-review.md`. Regenerate and overwrite, or save with a new name?"*
- The workspace draft is always overwritten on re-run without prompting.

## Examples
- `identity/examples/release-reviews/` — style reference for tone and structure
