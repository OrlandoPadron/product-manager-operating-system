---
name: new-action
description: Coached creation of a new custom skill. Interviews the user about purpose, examples, inputs, and behavior. Generates a SKILL.md following the OS's conventions (reads voice profile, drafts to workspace, confirms saves, suggests next action). Detects existing skills and switches to evolve mode.
---

# /new-action

## When to use
- A recurring PM task you do regularly that isn't already a skill
- You want to extend the OS to handle a new kind of work
- You want to refine an existing skill (evolve mode — same entry point)

## What this skill does
Interviews the user about a new custom skill, then generates the skill's `SKILL.md` file in `.claude/skills/<name>/SKILL.md` following all OS conventions. The generated skill always:
- Reads `identity/voice-profile.md` and `config.md` on every invocation
- Reads relevant context layers
- Saves drafts to `workspace/` first, asks before committing to canonical
- Appends to `CHANGELOG.md` when meaningful artifacts are saved
- Suggests next action when relevant
- Hints when context is missing

If a skill with the given name already exists, switches to evolve mode: shows current behavior, asks what to change, regenerates, diffs, saves on approval.

## Required inputs
- **Skill name** — `<name>` argument or asked. Will be slug-formatted.

## Behavior

### 1. Validate context

- Read `identity/voice-profile.md` and `config.md`.
- Compute path: `.claude/skills/<slug>/SKILL.md`.
- Check existence. If exists → **evolve mode** (jump to step 7).

### 2. Purpose

Ask:
- *"What does this action do? In one sentence."*
- *"When would you use it? What's the trigger condition?"*
- *"What does success look like? What output should the user get?"*

### 3. Examples

- *"Show me 2–3 outputs you've produced manually that this skill should imitate. Paste them, or point me at files in `identity/examples/<type>/` if you've already added some. If this is a brand-new artifact type, describe what the output should look like instead."*

If examples are pasted that don't yet have a home in `identity/examples/`, ask: *"Want to save these as examples for this skill (and future invocations)?"*

### 4. Naming category

Suggest a verb prefix based on purpose:
- Creating something coached → `/new-<noun>`
- Generating long-form → `/write-<noun>`
- Generating a short comm → `/draft-<noun>`
- Deriving from existing content → `/generate-<noun>`
- Forward-planning → `/plan-<noun>`
- Linking entities → `/link-<noun>`

If user chose a name that doesn't match the convention, gently suggest one that does — but don't force it.

### 5. Inputs and context

- *"What context does this skill need to read? Walk through:"*
  - Identity layer? (voice-profile always; which examples folder?)
  - Company layer? (which files specifically — OKRs, procedures, stakeholders, glossary?)
  - Product or release scope? (does the skill take `<product> <version>` arguments?)
  - A specific template? (does one exist in `templates/`, or do we need `/new-template`?)
  - A specific workflow? (does it consume `workflows/<x>.md`?)

- For each "needs a template that doesn't exist yet": offer to chain into `/new-template` (when available — note this is in slice 2, so for now leave a TODO in the generated skill).

- For each "needs a workflow that doesn't exist yet": offer to chain into `/new-workflow` (also slice 2).

### 6. Behavior steps

In plain English: *"When invoked, this action will: (1) read X, (2) ask Y, (3) draft Z, (4) save where, (5) suggest what next?"*

Walk through and refine. The user critiques each step.

### 7. Evolve mode (if skill exists)

- Read existing `.claude/skills/<slug>/SKILL.md`.
- Show current frontmatter description and section headers.
- *"What do you want to change? (purpose / inputs / behavior / save location / next action / examples / all)"*
- For each chosen section: show current, ask what to change, regenerate.
- Diff before saving.

### 8. Generate the SKILL.md

Use `templates/skill-scaffold.md` as the structural base. Fill in:
- `name` and `description` (frontmatter)
- "When to use" (from purpose interview)
- "What this skill does" (from purpose + behavior interview)
- "Context to read" (from inputs interview — always include voice profile + config)
- "Behavior" steps (from behavior interview, written as numbered prompts the skill executes)
- "Save location" (canonical path + draft path if applicable)
- "Missing context hints" (auto-add hint for empty voice profile; add others based on inputs)
- "Idempotency" notes
- "Next action hint" (if user identified a typical follow-up)

The generated skill must follow every convention in `docs/conventions.md`.

### 9. Show, refine, save

Show the full generated SKILL.md. *"Look right? Want to edit any section? (yes / let me edit / regenerate section X with changes: ...)"*

Iterate until approved.

Save to `.claude/skills/<slug>/SKILL.md`.

*"✓ Created skill `/{{slug}}` at `.claude/skills/<slug>/SKILL.md`."*

### 10. Test run (offer)

*"Want to do a dry run on a sample input so you can see it work before trusting it? (yes/no)"*

If yes: simulate the skill's behavior on a hypothetical input, walking through what it would do. Don't actually save anything during the dry run.

### 11. Append to CHANGELOG

```
- `.claude/skills/<slug>/SKILL.md` — {{Created / Updated}} skill *(via /new-action)*
```

### 12. Update README.md skill index

Find the relevant category in `README.md` ("Creation", "Generation", etc.) and add a one-liner for the new skill. Show the diff and confirm.

## Save location
- Canonical: `.claude/skills/<slug>/SKILL.md`
- Updates: `README.md` (skill index)
- No draft step — the skill is generated in conversation and saved directly to canonical when approved

## Missing context hints
- Voice profile empty → hint
- User wants a template that doesn't exist → flag and queue for `/new-template` (slice 2)
- User wants a workflow that doesn't exist → flag and queue for `/new-workflow` (slice 2)
- User trying to create a skill that duplicates an existing one → suggest evolve mode instead

## Idempotency
- Existing skill → evolve mode
- Re-running creates a clean, complete SKILL.md every time

## Naming convention
- Slug: lowercase, hyphenated. *"Draft an OKR review"* → `draft-okr-review`
- Verb prefix from the conventions in `docs/glossary.md`

## Next action hint
- After creation: *"Try `/<slug>` now to see it in action."*
- After dry run: *"Looks good — invoke `/<slug>` for real next time you do this work."*
