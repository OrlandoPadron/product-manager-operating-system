---
name: generate-release-review
description: Generate a polished PPTX release review presentation from a release's tickets and PRD. Asks coaching questions to shape the narrative, infers scope framing, and uses the style template PPTX as the visual base. Saves to workspace/ first, then commits to the release folder on approval.
---

# /generate-release-review

## When to use
After a release has shipped and before the release review meeting. Invoke with the product and version to generate the presentation deck.

Use it any time you need to present a release to your team — QA, dev, stakeholders, or the broader Speexx org.

## What this skill does
Reads the release's tickets and PRD, synthesizes the scope narrative, asks a focused set of coaching questions, then generates a `.pptx` file using your existing release review style template as the visual base. The deck is drafted to `workspace/` first; you confirm before it's saved to the release folder.

Team photos are placeholders in the generated file — you add them manually after.

## Required inputs
- **Product slug** — `<product>` argument (e.g., `mentoring`, `cms`)
- **Release version** — `<version>` argument (e.g., `43.10.0-mentoring-programs`)

If either is missing:
- If `default_product` is set in `config.md` and product is omitted, use it.
- If only product is given, list available releases in `products/<product>/releases/` and ask which one.

## Context to read
- `identity/voice-profile.md` (always)
- `config.md` (always)
- `identity/examples/release-reviews/` — style reference; the `.pptx` file here is the visual template
- `products/<product>/about.md` — product vision, KPIs, stakeholders
- `products/<product>/releases/<version>/` — tickets, PRD, and any existing `about.md`
- `company/glossary.md` — to correctly name Speexx-specific terms in the deck
- `company/teams.md` — for context on roles mentioned in the team slide

## Behavior

### 1. Validate context

Check the following before proceeding:

**Python + python-pptx:**
Run `python --version` (or `py --version` on Windows). If Python is not installed:
- Offer to install: *"Python isn't installed. I can install it via `winget install Python.Python.3.12` — want me to do that? Or install it manually and come back."*
- After Python is confirmed, check `pip show python-pptx`. If missing: run `pip install python-pptx`.

**Style template:**
Look for a `.pptx` file in `identity/examples/release-reviews/`. If more than one exists, ask which to use as the style base. If none exists:
- *"No style template found in `identity/examples/release-reviews/`. Drop your reference PPTX there and run this skill again."*

**Release folder:**
Confirm `products/<product>/releases/<version>/` exists and contains at least one of: a tickets file (`.doc` or `.md`) or a PRD (`.pdf` or `.md`). If neither exists: *"I can't find tickets or a PRD for this release. Add them to `products/<product>/releases/<version>/` and re-run."*

### 2. Read and synthesize content

Read the release content in this order of preference:
- **Tickets:** `.md` → read directly. `.doc` → extract text via Word COM (same pattern as the `/setup` identity module used for `cms-106-tickets.doc`).
- **PRD:** `.md` → read directly. `.pdf` → extract text via PowerShell PDF reader or, if extraction fails, ask: *"I can't extract the PDF directly. Can you paste the key scope points here, or convert the PRD to `.md`?"*

From the content, synthesize:
- The 3–5 main scope themes (what was built)
- Whether the release *replaces something existing* (signals Before/After framing) or *adds something new* (signals "What we built" framing)
- Key capabilities delivered — concrete, specific, not adjective-heavy
- Any KPI targets or success metrics mentioned in the PRD

### 3. Draft the title line

Generate a one-line description of the release for the title slide. Aim for the format:
> *[What it does] for [who] — [one concrete benefit]*

Example: *"Structured mentoring programs for targeted employee growth"*

Show it: *"Title line suggestion: '[draft]'. Keep it, or tell me what to change."*

Wait for confirmation or a replacement before continuing.

### 4. Coaching questions

Ask these in sequence, one at a time. Don't batch them.

**Q1 — Demo:**
*"Will there be a live demo during the review? (yes / no)"*
If yes → include a "Demo time" placeholder slide after the scope slide.

**Q2 — Scope framing:**
Based on the synthesis from step 2, propose the framing:
- If the release replaces something: *"This looks like a Before/After release — [old state] → [new state]. I'll frame the scope slide as 'What has changed?' — does that work, or do you want a different angle?"*
- If it's a new feature: *"This looks like a net-new feature. I'll frame the scope slide as 'What we built' — does that work?"*
- If unclear: *"How do you want to frame the main scope slide? Before/After, 'What we built', or something else?"*

**Q3 — What's next:**
*"What's coming after this release? List the upcoming initiatives with version numbers and rough dates if you have them — I'll build the 'What's next' timeline from that."*

**Q4 — The team:**
*"Who worked on this release? Give me names and roles (e.g., 'Joel González — Software Developer, Backend'). I'll leave photo placeholders for you to fill in after."*

**Q5 — Optional slides:**
*"Do you want to include any of these optional slides? (yes / no for each)"*
- Metrics: before/after on KPIs
- What worked / What didn't: process or product reflection

If metrics: *"What numbers do you want to show? Format: [metric name] — before: [X] / after: [Y]"*
If what worked/didn't: *"What's worth calling out — one or two things that worked well, and one honest thing that didn't?"*

### 5. Draft all slide content

Before generating the PPTX, show the full slide-by-slide content as structured text for review:

```
SLIDE 1 — Title
  Release name: [name]
  Description: [one-liner]
  Date: [today's date]
  Presenter: [from identity/about-me.md]

SLIDE 2 — [Scope frame title]
  [Content]

SLIDE 3 — Demo time  ← only if Q1 = yes

SLIDE [N] — Metrics  ← only if requested
SLIDE [N] — What worked / What didn't  ← only if requested

SLIDE [N] — What's next
  [Timeline items with versions + dates]

SLIDE [N] — The Team
  [Name — Role (photo placeholder)]
  ...

SLIDE [N] — Thank you / Questions
```

Say: *"Here's what I'll put in the deck. Review each slide and tell me what to change. When you're happy with the content, I'll generate the PPTX."*

Wait for confirmation or adjustments before proceeding.

### 6. Generate the PPTX

Write and execute a Python script that:

1. Opens the style template `.pptx` from `identity/examples/release-reviews/` using `python-pptx`
2. Reads the available slide layouts from the template's slide master
3. Creates a new presentation object and copies the theme/slide master from the template (using `prs.slide_master` and `prs.slide_layouts`)
4. Builds each slide in order using the confirmed content from step 5:
   - Maps each slide to the closest matching layout from the template
   - Populates title and body text placeholders
   - For the team slide: adds name and role as text, with a labeled placeholder box where the photo goes (e.g., "[ PHOTO: Joel González ]")
   - For the What's next slide: builds a timeline structure with version labels and dates
5. Saves to: `workspace/release-review-<product>-<version>.pptx`

If the script fails or produces unexpected output:
- Show the error and the relevant section of the script
- Offer to diagnose and retry before asking the user to intervene

### 7. Confirm and save canonical

Say:
*"✓ Generated: `workspace/release-review-<product>-<version>.pptx`*
*Note: team photos still need to be added manually to the team slide.*
*Move to `products/<product>/releases/<version>/release-review.pptx`? (yes / no)"*

If yes: move (or copy) the file to the canonical path.

Append to `CHANGELOG.md`:
```
- `products/<product>/releases/<version>/release-review.pptx` — Release review deck generated *(via /generate-release-review)*
```

### 8. Suggest next action

*"Deck saved. Open it, add team photos, and you're ready for the review meeting."*

If the release doesn't yet have an announcement drafted:
*"Next up: `/draft-announcement <product> <version>` to write the release communication."*

## Save location
- Draft: `workspace/release-review-<product>-<version>.pptx`
- Canonical: `products/<product>/releases/<version>/release-review.pptx`

## Missing context hints
- `identity/voice-profile.md` is empty → *"Run `/setup identity` to make output sound like you."*
- No `.pptx` in `identity/examples/release-reviews/` → *"Drop your reference release review PPTX in `identity/examples/release-reviews/` — the skill uses it as the visual base."*
- No tickets or PRD in the release folder → *"Add tickets (`.md` or `.doc`) and/or a PRD (`.md` or `.pdf`) to `products/<product>/releases/<version>/` before running this skill."*
- Python not installed → offer `winget install Python.Python.3.12`
- `python-pptx` not installed → offer `pip install python-pptx`
- KPIs are TBC in the PRD → ask the user for actual numbers before generating the metrics slide; don't fabricate baselines or targets

## Idempotency
- If a `release-review.pptx` already exists at the canonical path: *"A deck already exists at `products/<product>/releases/<version>/release-review.pptx`. Regenerate and overwrite, or save with a new name?"*
- The workspace draft is always overwritten on re-run without prompting.

## Examples
- `identity/examples/release-reviews/` — visual style reference; the `.pptx` here is also the style template used for generation
