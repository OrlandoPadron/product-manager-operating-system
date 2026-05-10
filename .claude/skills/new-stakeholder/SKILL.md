---
name: new-stakeholder
description: Coached creation of a stakeholder profile. Asks about role, communication style, what the person cares about, what to avoid. Optionally links the stakeholder to products and/or releases. Detects existing profiles and switches to evolve mode.
---

# /new-stakeholder

## When to use
- Adding a key stakeholder you communicate with regularly
- Updating an existing stakeholder's profile after new context (a recent meeting, a shift in their role, learnings about their style)

## What this skill does
Creates `company/stakeholders/<first-last>.md` with a structured profile. Coached interview captures role, what they care about, communication style, what to avoid. Final step: optionally links the stakeholder to one or more products and/or releases by writing into the *target's* frontmatter (forward links only — see `docs/conventions.md`).

If the named stakeholder already exists, switches to evolve mode: shows current profile, asks what to update, regenerates the relevant sections, diffs.

## Required inputs
- **Stakeholder name** — `<name>` argument or asked. Will be slug-formatted as `<first-last>.md`.

## Behavior

### 1. Validate context

- Read `identity/voice-profile.md`. If empty, hint.
- Compute filename: `company/stakeholders/<first-last>.md`.
- Check existence. If exists → **evolve mode** (jump to step 6).

### 2. Profile interview

Ask in order. Skip questions the user signals "don't know yet" — leave the section as a placeholder for later.

**Role and context** (3 questions):
- *"What's their full name and title?"*
- *"What team are they on, and who do they report to?"*
- *"What's your relationship — peer, manager, partner, customer, executive sponsor?"*

**What they care about** (the most important section):
- *"What are the 3–5 things this person genuinely cares about? Not their job description — the things that actually move them. Outcomes they're measured on, principles they hold, recurring concerns they raise."*

**What they don't care about**:
- *"What do they tend to skip past or de-prioritize? Useful so we don't pad communications with stuff they'll ignore."*

**Communication style** (4 questions):
- *"What's their preferred channel? (Async DM, short syncs, formal docs, email...)"*
- *"Format preference — short bullets vs. narrative, data-heavy vs. story-driven?"*
- *"Tone preference — direct, diplomatic, formal, casual?"*
- *"What's something specific that lands well with them? A way of framing things, a level of detail, a structure?"*

**What to avoid**:
- *"Anything that's caused friction or that you've noticed doesn't land?"*

**Decision-making lens**:
- *"When they review work, what are they actually evaluating? What's their first instinct?"*

**Past interactions**:
- *"Any past conversations or feedback worth remembering? Specific incidents that shaped how to work with them?"*

**Involvement style**:
- *"How do they like to be involved? Sponsor / reviewer / FYI-only / approver — and at what stage of work?"*

### 3. Draft profile

Use the template in `templates/stakeholder.md`. Fill in collected info. Preserve placeholders for fields the user didn't answer (so they can be filled in later).

Show the draft. *"Look right? (yes / let me edit / regenerate with these changes: ...)"*

Iterate until approved.

### 4. Save profile

Save to `company/stakeholders/<first-last>.md`.

*"✓ Saved profile to `company/stakeholders/<first-last>.md`."*

### 5. Linking step (optional but offered)

*"Want to link this stakeholder to any products or releases?"*

If yes:
- Read `products/`. List products. *"Which products are they involved with? (Multi-select.)"*
- For each selected product:
  - *"What role on this product? (sponsor / reviewer / decision-maker / tech-lead / design-partner / etc.)"*
  - Update `products/<product>/about.md` frontmatter to add `{ name: <slug>, role: <role> }` to the `stakeholders:` list.
- *"Any specific releases? (List in-flight releases for the selected products.)"*
- For each release:
  - Same role question.
  - Update `products/<product>/releases/<version>/about.md` frontmatter.

For each link written, confirm: *"✓ Linked to `products/<product>/about.md`."*

If user says no to linking: *"OK — you can link later via `/link-stakeholder <name>` whenever you're ready."*

### 6. Evolve mode

- Show current profile in `company/stakeholders/<first-last>.md`.
- *"What do you want to update? (role / communication style / what they care about / past interactions / links to products or releases / all)"*
- Apply only the requested sections. Diff before saving.
- For link updates, show current links and ask which to add/remove.

### 7. Confirm and append to CHANGELOG

```
- `company/stakeholders/<first-last>.md` — {{Created profile / Updated profile}} *(via /new-stakeholder)*
```

If links were added:
```
- `products/<product>/about.md` — Linked stakeholder {{name}} as {{role}} *(via /new-stakeholder)*
```

## Save location
- Canonical: `company/stakeholders/<first-last>.md`
- Link writes: `products/<product>/about.md` and/or `products/<product>/releases/<version>/about.md` frontmatter

## Missing context hints
- Voice profile empty → hint that profile *prose* will be more accurate after `/setup identity`
- No products defined yet → linking step will be skipped automatically; mention this so the user knows it's available later

## Idempotency
- Existing profile → evolve mode
- Existing links → if linking step adds a duplicate link, detect and ask before overwriting role

## Naming convention
- Filename: `<first-last>.md`, lowercase, hyphenated. *"María García-López"* → `maria-garcia-lopez.md`.
- Frontmatter `name:` preserves the actual display name.

## Next action hint
- If user just created a stakeholder and didn't link: *"Link them to a product or release later via `/link-stakeholder <slug>`."*
- If linked to a release: *"Skills like `/draft-teams-post <product> <version>` will now read this profile when generating release content."*
