---
name: draft-announcement
description: Draft the customer / user-facing release announcement in your voice. Reads identity, examples/announcements, the release's PRD, changelog, and stakeholders. Asks about audience and channel (email, in-app, blog, social) and adapts format. Drafts to workspace/, then commits to the release folder.
---

# /draft-announcement

## When to use
- A release is shipped or about to ship and needs an external announcement
- Updating an existing announcement (different channel, different audience segment, refinement after legal/marketing review)

## What this skill does
Generates the **external-facing** announcement of a release — the message that goes to customers, users, partners, or the public. Different from `/draft-teams-post`, which is internal team communication.

Asks audience and channel up front (these reshape format dramatically). Reads the release's PRD, changelog (if generated), and linked stakeholders. Imitates the user's announcement-writing patterns from `identity/examples/announcements/`.

The announcement template is **inlined** in this skill — short artifacts don't need a separate file.

Drafts to `workspace/`. Commits to canonical only after approval.

## Required inputs
- **Product slug** — `<product>` argument
- **Release version** — `<version>` argument

If `default_product` is set, product can be omitted.

## Behavior

### 1. Validate context

- Read `identity/voice-profile.md`. Hint if empty.
- Read `config.md` for output language.
- Resolve release folder. If missing → offer `/new-release`.
- Read `products/<product>/releases/<version>/about.md` for release metadata.
- Read PRD if it exists; warn if missing.
- Read changelog if it exists — useful for "what's new" framing. If missing: *"No changelog yet for this release. Want to run `/generate-changelog` first? (You can skip — the announcement will derive from the PRD instead.)"*

### 2. Ask audience and channel

*"Who's this announcement for?"*
1. **All customers / users** — broad audience, plain language
2. **A specific customer segment** — name it; the skill will read relevant context
3. **Partners / integrators** — technical adjacent
4. **Public / press / blog** — broadest, most polished

*"What channel will this go out on?"*
1. **Email** — subject line + body, longer-form OK
2. **In-app message** — much shorter, optional CTA
3. **Blog post** — long-form, standalone
4. **Social media** — very short, attention-getting (one-line + link)
5. **Multiple** — generate variants in one go

The choice affects length, structure, formality, and CTA style.

### 3. Load context

- `identity/voice-profile.md`
- `identity/examples/announcements/` — every example file (primary tonal source)
- `config.md`
- `company/about.md` — brand voice clues
- `company/glossary.md` — to expand internal terms for external audiences
- `products/<product>/about.md` — product positioning, audience definition, KPIs
- `products/<product>/releases/<version>/about.md` — release scope
- `products/<product>/releases/<version>/prd.md` — *why* the release exists, customer evidence
- `products/<product>/releases/<version>/changelog.md` — *what* shipped (if generated)
- For releases with linked stakeholders in customer-facing roles (design partners, customer reps): their profiles for what they care about

### 4. Detect refinement mode

If `products/<product>/releases/<version>/announcement.md` exists and is non-empty:
- *"An announcement already exists. Refine, regenerate for a different channel/audience, or diff?"*

### 5. Ask the framing question

The hardest part of an announcement is **what's the one thing the user should remember**. Ask explicitly:

*"If readers only remember one thing from this announcement, what should it be? (Phrase as a benefit, not a feature.)"*

This becomes the lead. Don't draft without it.

### 6. Resolve template, then draft

#### Resolve template (custom override → default fallback)

Check for a user-provided template matching the chosen channel:

| Channel | Custom template path |
|---|---|
| Email | `templates/announcement-email.md` |
| In-app | `templates/announcement-in-app.md` |
| Blog | `templates/announcement-blog.md` |
| Social | `templates/announcement-social.md` |
| Multi-channel | Resolve each variant independently |

If the file exists → use its structure as the base; map content to its sections and frontmatter, even if it differs from the default below. If missing → use the default fallback for that channel below.

Templates control *structure*; voice always comes from `identity/voice-profile.md` and `identity/examples/announcements/`.

#### Email

```markdown
---
channel: email
subject: {{subject_line — punchy, benefit-led, ≤60 chars}}
preheader: {{preheader — extends subject, ≤90 chars}}
audience: {{audience}}
---

{{Greeting}}

{{Lead — the one thing, stated simply}}

{{Why it matters — one short paragraph tying to user pain or goal}}

{{What's new}}
- {{Feature 1}} — {{user benefit}}
- {{Feature 2}} — {{user benefit}}

{{CTA — one clear action}}

{{Sign-off}}
```

#### In-app message

```markdown
---
channel: in-app
audience: {{audience}}
character_limit: {{limit_per_design_system}}
---

**{{Headline — 5–8 words, benefit-led}}**

{{Body — 1–2 short sentences max}}

[{{CTA button text}}]({{cta_link}})
```

#### Blog post

```markdown
---
channel: blog
audience: {{audience}}
title: {{title}}
slug: {{slug}}
---

# {{title}}

{{Lead paragraph — sets up the release in context, hooks the reader}}

## Why we built this
{{Customer pain or strategic context — pull from PRD}}

## What's new
{{Feature-by-feature, with benefits and screenshots/links if available}}

## How to use it
{{Concrete first steps}}

## What's next
{{Forward look — tease v+1 if appropriate}}

{{CTA}}
```

#### Social media

```markdown
---
channel: social
audience: public
character_limit: 280  # adjust per platform
---

{{One-line hook}}

{{Optional: emoji_or_specific_detail}}

{{Link / CTA}}
```

For all formats:
- Apply voice from `identity/voice-profile.md` and patterns from `identity/examples/announcements/`
- Honor `config.md` `language.output`
- Expand any acronym from `company/glossary.md` that isn't broadly understood
- **Don't invent metrics or quotes.** If the user wants social proof and there's none in the loaded context, ask.

### 7. Save draft to workspace

Save to `workspace/announcement-<product>-<version>[-<channel>].md`.

*"✓ Drafted announcement to `workspace/...`. Show / edit / commit?"*

### 8. Iterate

Per-section feedback. Common refinement requests:
- *"Make the subject line shorter / punchier"*
- *"Lead with the customer pain instead of the feature"*
- *"Reframe for [specific stakeholder]'s lens"*
- *"Adapt this for the in-app channel too"*

If user requests an additional channel variant, draft alongside the original.

### 9. Commit to canonical

Move primary version from workspace to `products/<product>/releases/<version>/announcement.md`.

If multiple channel variants:
- Primary version → `announcement.md`
- Variants → `products/<product>/releases/<version>/announcement-<channel>.md` (e.g., `announcement-email.md`, `announcement-blog.md`)

Append to `CHANGELOG.md`:
```
- `products/<product>/releases/<version>/announcement.md` — {{Drafted / Refined}} announcement ({{channel}}, {{audience}}) *(via /draft-announcement)*
```

### 10. Surface flags

- Acronyms expanded — list them so user can verify
- Quotes or numbers referenced — show source
- Anything in the PRD framed as "internal" that the user might not want in an external announcement
- Tone calibration mismatch — if examples lean casual but the channel was set to "press", flag it

### 11. Suggest next action

- *"Next: `/draft-teams-post <product> <version>` for the internal team comms (slice 2)"*, or
- *"Next: `/draft-release-review <product> <version>` for the release review slides (slice 2)"*

Based on the active workflow.

## Save location
- Draft: `workspace/announcement-<product>-<version>[-<channel>].md`
- Canonical: `products/<product>/releases/<version>/announcement.md` (and optional variants)

## Missing context hints
- No announcement examples → flag, fall back to messages examples for tone, warn the output may not be locked-in voice yet
- No PRD → can still proceed but flag that "why we built this" sections will be thin
- No changelog → can derive features from tickets directly, but recommend running `/generate-changelog` first for cleaner feature framing
- Customer-facing audience but no design-partner / customer-rep stakeholders linked → flag: *"You don't have a customer voice in the linked stakeholders. The announcement may sound generic — consider linking a design partner or customer rep first."*

## Idempotency
- Existing announcement → refinement mode
- Different channel/audience → adds variant alongside existing primary
- Workspace can be regenerated freely

## Voice consistency
Announcements are short, voice-dense artifacts. Copy patterns from `identity/examples/announcements/` exactly: subject-line style, opening lines, sign-offs, CTA language, emoji policy. The voice profile is general; the examples carry the specifics.

## Examples
- `identity/examples/announcements/` — primary, on every invocation
- `identity/examples/messages/` — fallback if announcements folder is empty

## Output language
- Honors `config.md` `language.output`
- Override via `--lang=<code>`

## Next action hint
Read active workflow. Typically: internal Teams post and/or release review next.
