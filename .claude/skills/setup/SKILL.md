---
name: setup
description: First-run interactive configuration for the Product Operating System. Modular — sub-modules for identity (with voice interview), company, product, language, workflow. Idempotent and safe to re-run.
---

# /setup

## When to use
- First time opening this OS — run `/setup` with no arguments to walk through every module
- Returning to refresh a specific layer — `/setup identity`, `/setup company`, etc.
- Adding a new product, workflow, or language preference

## What this skill does
Walks the user through configuring the OS interactively. Modular: each sub-module is independently runnable. Idempotent: detects existing values and asks before overwriting.

## Modules

```
/setup                       # Run all modules in order
/setup language              # Set system + output language, formality, time zone
/setup identity              # Voice interview, fill in about-me / values / voice-profile
/setup company               # Fill in company/* files
/setup product [name]        # Delegate to /new-product to create the first/next product
/setup workflow [name]       # Delegate to /new-workflow to define a workflow
```

## Behavior

### When invoked with no arguments

1. **Detect first-run state.** Read `config.md`. If placeholders (`<run /setup to fill in>`) are still present, this is a first run. Otherwise, ask: *"Looks like you've already run setup. Want to refresh everything, or pick a specific module?"*

2. **Run modules in order**, asking before each: *"Run `<module>` now? (y/n/skip)"*
   - language → identity → company → product → workflow

3. After each module, write a one-line entry to `CHANGELOG.md`.

4. End with a summary of what was configured and a suggestion: *"You're set up. Try `/tour` next to see how the OS thinks, or jump into `/write-prd` if you have something to draft."*

### `language` module

Read `config.md`. Then:

1. *"What language do you want the system to use for skill prompts and internal artifacts? (Default: en)"*
2. *"What language do you want generated content (PRDs, posts, etc.) to be in? Default is the same as system language."*
3. *"What's your overall voice formality? formal / professional-casual / casual"*
4. *"What's your time zone? (IANA format, e.g., Europe/Madrid)"*

Update the frontmatter in `config.md`. Confirm: *"✓ Saved language preferences to `config.md`."*

### `identity` module — the voice interview

This is the highest-value module. Don't rush it.

1. **About-me questions** (10–12):
   - Role and title
   - What you actually do day-to-day
   - What you're accountable for
   - What you focus on (products, themes)
   - How you operate (solo/collaborative, async/sync, doc-heavy/conversation-heavy)
   - Background that shapes your perspective
   - What you're trying to optimize for

2. **Values questions** (5–7):
   - What you prioritize as a PM (user impact, technical rigor, speed, quality, etc.)
   - Personal principles that show up in your work
   - What you won't compromise on
   - What you're willing to flex on

3. **Voice interview** (the critical part):
   - *"To capture your voice, paste 3–5 samples of your real writing. They can be: a Slack/Teams message, a PRD section, a JIRA ticket description, an email to a stakeholder. Don't sanitize — I won't save them as examples unless you tell me to. I'll just analyze them."*
   - After samples are pasted, ask 6–8 calibrated follow-up questions:
     - *"In sample 2 you wrote '...'. Is that a phrase you use often, or was it specific to that context?"*
     - *"What's a word or phrase you'd never use, even if it's technically correct?"*
     - *"When writing to executives vs. engineers, what changes about your tone?"*
     - *"How long is a typical message from you? Do you over-explain or under-explain?"*
     - *"Are you more likely to state your conclusion up front or walk through reasoning first?"*
     - *"What's a structure you almost always use in long-form docs? (TL;DR? bullet-first? headings?)"*
     - *"What would make a coworker say 'that doesn't sound like Orlando'?"*
   - **Generate `identity/voice-profile.md`** from the conversation. Be specific — quote actual phrases the user uses, name actual anti-patterns. Show the draft.
   - *"Want me to save the samples you pasted as examples (sanitized for sharing, or kept private in `identity/examples/private/`)?"*

4. Write `identity/about-me.md`, `identity/values.md`, `identity/voice-profile.md`. Optionally write samples to `identity/examples/`.

5. *"✓ Saved identity. Voice profile is in `identity/voice-profile.md` — review it and edit if anything's off."*

### `company` module

For each file in `company/` (about, okrs, kpis, teams, tools, procedures, glossary):

1. Show the existing placeholder structure
2. Ask the questions the structure implies
3. **For OKRs and KPIs specifically**, ask the maturity question:
   - *"How are OKRs tracked at your company? (a) formal & documented (b) informal / verbal priorities (c) not tracked"*
   - If (b): capture in plain language, set status as informal in the first line of `okrs.md`
   - If (c): write *"Not currently tracked."* and offer to capture *priorities* instead in `company/priorities.md`
4. For `stakeholders/`: *"Want to add a stakeholder profile now? You can also do this later via `/new-stakeholder`."* If yes, delegate to `/new-stakeholder`.

Write each file. Confirm each save.

### `product` module

Delegate to `/new-product`. *"Let's create your first product. I'll hand off to `/new-product` — you can return to setup after."*

After `/new-product` finishes, ask: *"Add another product, or move on?"*

### `workflow` module

Delegate to `/new-workflow` if it exists. If not yet built (this is the first slice), say: *"Workflow setup will be available in the next slice. For now, the OS ships with `workflows/discovery-to-release.md` as a generic example — you can edit it directly to match your process. Or run `/new-action` later to define a custom workflow definition skill."*

## Context to read
- `config.md` (always)
- Existing files in `identity/`, `company/`, `products/` (to detect filled vs. placeholder state)
- `_template/` if files don't exist yet (to copy placeholders into live folders)

## Save behavior
- Updates `config.md` frontmatter (language module)
- Writes `identity/about-me.md`, `identity/values.md`, `identity/voice-profile.md` (identity module)
- Writes files in `company/` (company module)
- Calls `/new-product` to write `products/<name>/...` (product module)
- Calls `/new-workflow` for workflow module (when available)
- Appends to `CHANGELOG.md` after each module completes

Always confirm save paths to the user.

## Missing context hints
- If `_template/` is missing or incomplete, surface a warning — the OS scaffolding is broken
- If identity examples folder is empty after voice interview, hint: *"Add examples to `identity/examples/<type>/` over time — the more I have to learn from, the better I sound like you."*

## Idempotency
- Re-running detects existing non-placeholder content and asks before overwriting any file
- Each module is independently runnable to refresh just one layer
- Skipping a module preserves existing state

## Next action hint
After full setup: *"Try `/tour` for a 5-minute walkthrough of how the OS works, or `/write-prd` to draft something real."*

After a single module: *"Module complete. Run `/setup` (no args) to do the rest, or jump to a specific skill."*
