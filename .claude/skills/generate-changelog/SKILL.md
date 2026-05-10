---
name: generate-changelog
description: Generate a release changelog by synthesizing the PRD and shipped tickets. Asks for audience (customer-facing vs. internal vs. technical) and adapts tone, depth, and grouping accordingly. Drafts to workspace/, then commits to the release folder. Detects existing changelog and refines instead of overwriting.
---

# /generate-changelog

## When to use
- A release has been developed and is ready to be communicated as a changelog
- After tickets are committed and you want the user-facing or internal recap of what shipped
- Updating an existing changelog when scope changed late or items got cut

## What this skill does
Reads the release's PRD and tickets, synthesizes a changelog grouped by theme (or by user-impact, depending on audience), in the user's voice. The changelog template is **inlined** in this skill (not a file) — short artifacts don't need their own template file.

Asks the user up front who the audience is, since that radically changes structure:
- **Customer-facing** — outcomes and benefits, no JIRA keys, no internal jargon
- **Internal team** — what shipped, what was cut, what's known-broken
- **Technical / developer** — API changes, breaking changes, deprecations, migration notes

Drafts to `workspace/`. Commits to canonical only after approval.

## Required inputs
- **Product slug** — `<product>` argument
- **Release version** — `<version>` argument

If `default_product` is set, product can be omitted.

## Behavior

### 1. Validate context

- Read `identity/voice-profile.md`. Hint if empty.
- Read `config.md`.
- Resolve release folder. If missing → offer `/new-release`.
- Read `products/<product>/releases/<version>/prd.md`. If empty: *"PRD is empty. Changelog quality depends on the PRD — continue with just tickets, or run `/write-prd` first?"*
- Read `products/<product>/releases/<version>/tickets/`. If empty: *"No tickets in this release. Continue with just the PRD, or run `/write-jira-tickets` first?"*

### 2. Ask audience

*"Who's the audience for this changelog?"*

1. **Customer-facing** (default if release has a customer-facing announcement) — outcomes, benefits, no internal jargon
2. **Internal team** (default for company-internal changelogs) — what shipped, what was cut, known issues
3. **Technical / developer** — API changes, breaking changes, migration notes
4. **Multi-audience** — generate two or three variants in one go

The user's choice determines structure and voice depth.

### 3. Load context

- `identity/voice-profile.md`
- `identity/examples/announcements/` — closest tonal proxy for customer-facing
- `identity/examples/messages/` — for internal-team tone
- `config.md`
- `company/glossary.md` — to expand internal acronyms when audience is external; preserve them when internal
- `products/<product>/about.md` — product KPIs (for outcome framing)
- `products/<product>/releases/<version>/about.md` — release metadata, scope summary, status
- `products/<product>/releases/<version>/prd.md` — primary source for *why*
- All files in `products/<product>/releases/<version>/tickets/` — primary source for *what*
- For customer-facing audience: linked stakeholders' profiles for any user representatives or design partners (their priorities should shape what gets emphasized)

### 4. Detect refinement mode

If `products/<product>/releases/<version>/changelog.md` already exists and is non-empty:
- *"A changelog already exists. Refine, regenerate, or diff?"*

### 5. Synthesize

#### Resolve template (custom override → default fallback)

Before drafting, check for a user-provided template matching the chosen audience:

| Audience | Custom template path |
|---|---|
| Customer-facing | `templates/changelog-customer.md` |
| Internal team | `templates/changelog-internal.md` |
| Technical / developer | `templates/changelog-developer.md` |
| Multi-audience | Resolve each variant independently |

If the file exists → use its structure as the base; map content to its sections and frontmatter, even if it differs from the default below. If missing → use the default fallback for that audience below.

Templates control *structure*; voice always comes from `identity/voice-profile.md` and examples regardless.

#### Inlined template — Customer-facing

```markdown
# {{release_title}}

{{one_paragraph_summary — what changed for users, in plain language}}

## What's new
- **{{feature_name_1}}** — {{user_benefit}}.
- **{{feature_name_2}}** — {{user_benefit}}.

## What's improved
- {{improvement_1}}
- {{improvement_2}}

## What's fixed
- {{bug_fix_1}}
- {{bug_fix_2}}

{{optional: known issues, what's coming next}}
```

#### Inlined template — Internal team

```markdown
# {{release_title}} — Internal Changelog

**Shipped:** {{date}}
**Status:** {{status}}

## What shipped
{{grouped_by_theme — link to PRD section, list relevant tickets by key}}

## What was cut
{{anything_in_PRD_but_dropped_from_release — with reason if known}}

## Known issues
{{from_tickets_marked_known_broken}}

## Metrics watch
{{which_KPIs_to_watch_post_release}}

## Stakeholder notes
{{anything_specific_for_linked_stakeholders}}
```

#### Inlined template — Technical / developer

```markdown
# {{release_title}} — Developer Notes

**Version:** {{version}}
**Released:** {{date}}

## Breaking changes
{{or "None"}}

## API changes
{{additions, deprecations}}

## Migration notes
{{steps_developers_need_to_take}}

## Bug fixes
{{relevant_to_API_consumers}}

## Internal changes worth noting
{{anything_API_adjacent_that_changed}}
```

Apply voice from `identity/voice-profile.md`. **Don't invent items.** Every entry must trace to a ticket, the PRD, or the release `about.md`. If something feels missing, ask the user — don't fabricate.

### 6. Save draft to workspace

Save to `workspace/changelog-<product>-<version>.md` (or `workspace/changelog-<product>-<version>-<audience>.md` if multi-audience).

*"✓ Drafted changelog to `workspace/...`. Show / edit / commit?"*

### 7. Iterate

Standard feedback loop. Per-section edits, regroupings, additions, removals.

### 8. Commit to canonical

When approved, move to `products/<product>/releases/<version>/changelog.md`.

If multi-audience: write the *primary* audience's version to `changelog.md`, save additional audience variants to `products/<product>/releases/<version>/changelog-<audience>.md` (e.g., `changelog-developer.md`).

Append to `CHANGELOG.md` (the OS's own log, not to be confused with the release changelog):
```
- `products/<product>/releases/<version>/changelog.md` — {{Generated / Refined}} changelog ({{audience}}) *(via /generate-changelog)*
```

### 9. Surface flags

- Tickets that didn't make it into the changelog (and why — e.g., marked as Out of scope or Cut)
- Acronyms expanded for external audience (so user can verify the expansion is right)
- KPI deltas referenced from product `about.md` — flag if data wasn't available

### 10. Suggest next action

- *"Next: `/draft-announcement <product> <version>` to write the customer-facing communication"*, or
- *"Next: `/draft-release-review <product> <version>` to prepare the release review slides"*

Based on the active workflow.

## Save location
- Draft: `workspace/changelog-<product>-<version>[-<audience>].md`
- Canonical: `products/<product>/releases/<version>/changelog.md` (and optional variants)

## Missing context hints
- Empty PRD or tickets → strong warning
- No announcement examples → use messages examples as fallback for customer tone, flag the substitution
- Glossary terms used internally that aren't in `company/glossary.md` and audience is external → flag and ask for expansion

## Idempotency
- Existing changelog → refinement mode (preserves entries; updates only what changed)
- Workspace drafts can be regenerated freely

## Voice consistency
Changelogs are concentrated voice signal — every entry is short, so each phrase carries weight. Imitate the announcement and message examples closely. Default to brevity unless user's examples lean long.

## Examples
- `identity/examples/announcements/` (primary for customer audience)
- `identity/examples/messages/` (primary for internal audience)

## Next action hint
Typically: announcement → release review. Read active workflow to confirm.
