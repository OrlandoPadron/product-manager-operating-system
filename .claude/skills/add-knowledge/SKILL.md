---
name: add-knowledge
description: Capture a learning, decision, domain concept, or insight into the right home in the OS. Coached — classifies the input, refines wording for clarity, suggests destination, dates the entry. Knowledge that can't be retrieved later is wasted.
---

# /add-knowledge

## When to use
- You learned something worth remembering — from a meeting, a postmortem, a customer call, a release retro
- You made a decision worth capturing the *why* of (ADR-style)
- You want to record a domain concept or internal acronym
- You want to update a stakeholder profile, glossary entry, or product `about.md` with a discrete fact

## What this skill does
Takes raw input from the user and routes it to the right file. Refines the wording for clarity (knowledge that can't be retrieved is wasted). Dates the entry. Confirms the destination. Appends rather than overwrites.

## Required inputs
- The thing to capture — pasted text, described in plain language, or a reference to a doc to summarize.

## Behavior

### 1. Validate context

- Read `identity/voice-profile.md` (so the rewrite stays in voice — even though knowledge entries are short, voice consistency matters when reading them back later).
- Read `config.md`.

### 2. Capture the input

*"What do you want to capture? You can:*
- *Paste text and I'll summarize and place it*
- *Describe in your own words*
- *Point me at a doc, meeting note, or thread to extract from*
*"*

If user pastes text: read it. If they describe: re-state it back to confirm understanding. If they reference a doc: read it.

### 3. Classify

Based on content, propose a destination:

| Content type | Destination |
|---|---|
| Domain concept, how something works in your industry | `knowledge.md` (Domain knowledge section) |
| "Why we chose X over Y" | `knowledge.md` (Decisions section) |
| "What we'd do differently" | `knowledge.md` (Learnings section) |
| Internal acronym, codename, term | `company/glossary.md` |
| Fact about a specific product | `products/<x>/about.md` |
| Fact about a specific stakeholder | `company/stakeholders/<name>.md` (proposes specific section) |
| New procedure or norm | `company/procedures.md` |
| Customer/user insight tied to a product | `products/<x>/about.md` (audience section) |

Show the proposed destination: *"This sounds like a [type]. I'd put it in `[file]`. Sound right? (yes / no, suggest another / let me pick)"*

If user disagrees, offer alternatives. If user picks a non-standard destination, accept and proceed.

### 4. Refine wording

Knowledge entries need to be retrievable later. Rewrite for:
- **Specificity** — replace "we" with concrete actors, "recently" with dates, vague nouns with concrete ones
- **Searchability** — front-load terms someone would grep for
- **Brevity** — strip filler. Knowledge entries should be 2–6 sentences max for most things; longer for decision rationale

Show the rewrite. *"Cleaned-up version below. Want to edit, or save as is?"*

```
### [YYYY-MM-DD] [category] {{Title}}

{{Body — what was learned, why it matters, how to apply it next time.}}
```

Iterate until user approves.

### 5. Append to destination

- Read the destination file.
- Find the appropriate section (e.g., `## Domain knowledge` in `knowledge.md`, `## Acronyms` in glossary).
- Append the new entry under that section, dated. **Don't overwrite** existing content.
- For destinations without obvious sections (e.g., `products/<x>/about.md`), ask which section to append to.

### 6. Confirm and CHANGELOG

*"✓ Saved to `<file>` (under section `<section>`)."*

Append to `CHANGELOG.md`:
```
- `<file>` — Added knowledge entry: "{{Title}}" *(via /add-knowledge)*
```

### 7. Offer split (if knowledge.md is getting big)

When `knowledge.md` exceeds ~100 entries (roughly 8000 lines), at the end of the save:

*"`knowledge.md` is getting large. Want to split it into folders? I can create `knowledge/domain.md`, `knowledge/decisions.md`, `knowledge/learnings.md` and migrate existing entries."*

If yes: create the split structure, migrate, update internal references.

## Save location
- Most common: appends to `knowledge.md`
- Other: `company/glossary.md`, `company/procedures.md`, `company/stakeholders/<name>.md`, `products/<x>/about.md`, etc.
- Never creates a new top-level file unless splitting `knowledge.md`

## Missing context hints
- If user references a stakeholder/product that doesn't exist: *"`<name>` doesn't have a profile yet. Want to create one first via `/new-stakeholder` or `/new-product`?"*

## Idempotency
- Always appends; never overwrites
- If user adds the same fact twice, detects similarity and asks: *"This looks like the entry from `<date>` — append anyway, replace it, or skip?"*

## Format conventions
- Entries are dated with `YYYY-MM-DD`
- Categories are bracketed: `[domain]`, `[decision]`, `[learning]`, `[insight]`, `[gotcha]`, etc.
- Title is concise — one line, searchable

## Next action hint
- After saving: *"You can review captured knowledge anytime in `<file>`. Skills will reference it when relevant context comes up."*
- If multiple related entries are being captured: *"Want to capture another?"*
