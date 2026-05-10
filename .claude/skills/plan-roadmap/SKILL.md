---
name: plan-roadmap
description: Coached roadmap planning for a product. Reads OKRs, KPIs, in-flight releases, and product strategy. Asks about time horizon, themes, capacity, and dependencies. Produces a structured roadmap that ties initiatives back to outcomes. Refines existing roadmaps additively without losing prior planning.
---

# /plan-roadmap

## When to use
- Putting together a roadmap for a product for the first time
- Refreshing a roadmap at the start of a new quarter or planning cycle
- Re-prioritizing after strategic shifts, OKR changes, or major customer signal
- Pressure-testing whether your current roadmap aligns with your OKRs

## What this skill does
Coached planning skill — more interactive than typical content-generation skills. Walks the user through outcome-first roadmap construction:
1. Set the time horizon
2. Surface the OKRs and KPIs the roadmap should serve
3. Group work into themes (outcomes, not features)
4. Place initiatives in the time horizon based on impact and dependencies
5. Flag tradeoffs and capacity constraints

Writes the result to `products/<product>/roadmap.md`. In refinement mode, preserves prior planning and works additively.

Roadmaps are planning documents — no `workspace/` draft step. The skill iterates conversationally and saves directly to canonical when the user approves each section.

## Required inputs
- **Product slug** — `<product>` argument

If `default_product` is set, can be omitted.

## Behavior

### 1. Validate context

- Read `identity/voice-profile.md`. Hint if empty.
- Read `config.md`.
- Resolve `products/<product>/`. If missing → offer `/new-product`.

### 2. Load context

- `identity/voice-profile.md` and `identity/values.md`
- `config.md`
- `company/about.md` — strategic context
- `company/okrs.md` — current OKRs (note maturity tier from first prose line)
- `company/kpis.md` — KPIs that may need to move
- `products/<product>/about.md` — product positioning, KPIs, stakeholders
- `products/<product>/roadmap.md` if exists — current roadmap (refinement mode)
- `products/<product>/releases/` — list every release, status, target date — what's already in flight
- For each linked product stakeholder: profile (especially sponsors and tech-leads — their priorities shape what's prioritizable)
- `knowledge.md` — for prior decisions or learnings that might apply

### 3. Detect mode

- No existing roadmap, or it's a placeholder → **first-pass mode**
- Existing roadmap → *"Refresh entire roadmap, refine specific time horizon, or add new initiatives only?"*
  - Refresh — full re-plan
  - Refine horizon — pick Q2/H2/etc and re-do just that section
  - Add only — append new initiatives without touching existing ones

### 4. Set time horizon

*"What time horizon are we planning? (Q / H / year / multi-year)"*

Default to whatever the company's OKR cadence is (from `company/okrs.md` `review_cycle:` if present).

Sub-question if needed: *"How granular within the horizon? (one bucket per quarter / per month / per release)"*

### 5. Surface anchoring outcomes

Read OKRs and product KPIs. Show them.

*"Which of these is this roadmap most accountable for moving? (Multi-select. The roadmap should ladder up to these.)"*

If OKRs are marked informal or absent: *"OKRs are informal — what outcomes do *you* think this roadmap should drive? List them in plain language."* Use those as the anchor.

### 6. Theme generation

*"What themes do you think will dominate this horizon? Themes are outcomes (e.g., 'reduce time-to-value for new users') not features ('redesign onboarding'). Aim for 3–5."*

If user struggles: propose themes derived from:
- The selected OKRs and KPIs
- Recurring concerns in linked stakeholder profiles
- Customer evidence captured in `products/<product>/about.md` or `knowledge.md`
- Status of in-flight releases (what they suggest is already prioritized)

User accepts, edits, or replaces.

### 7. Initiative placement

For each theme, ask:
- *"What concrete initiatives belong under this theme? (Initiatives ≈ release-sized chunks of work.)"*
- *"Rough impact on the anchor outcomes? (high / medium / low — and which outcome specifically)"*
- *"Rough effort? (S / M / L / XL — be honest, lean conservative)"*
- *"Dependencies — anything that has to happen first? (Other initiatives, infra, hiring, decisions)"*

Surface conflicts:
- Two initiatives competing for the same critical path
- Initiatives without clear outcome ties
- Outcomes with no initiatives addressing them

### 8. Capacity reality check

*"What's your team's realistic capacity over this horizon? (How many M / L initiatives can you actually finish?)"*

If the proposed roadmap exceeds capacity: *"Looks like ~{{N}} initiatives but we said ~{{capacity}}. What gets cut, deferred, or de-scoped?"*

Walk through tradeoffs explicitly. Roadmap value comes from honest capacity, not aspirational lists.

### 9. Risks and unknowns

- *"What are the biggest risks to this roadmap? (Capacity, dependencies, market shifts, stakeholder alignment, technical unknowns.)"*
- *"What do we *not* know yet that could change this plan? (Spike candidates, validation needed, decisions pending.)"*

### 10. Resolve template, then draft

#### Resolve template (custom override → default fallback)

If `templates/roadmap.md` exists → use its structure as the base; map planning content to its sections and frontmatter. If missing → use the default fallback structure below.

Templates control *structure*; voice always comes from `identity/voice-profile.md`.

#### Default fallback structure

```markdown
---
product: {{product-slug}}
horizon: {{horizon}}
last_updated: {{date}}
anchor_outcomes:
  - {{okr_or_kpi_1}}
  - {{okr_or_kpi_2}}
review_cycle: {{cycle}}
---

# Roadmap — {{Product Display Name}}

**Horizon:** {{horizon}}
**Last updated:** {{date}}

## Anchor outcomes
*What this roadmap is accountable for moving.*
- **{{outcome_1}}** — {{current → target}}
- **{{outcome_2}}** — {{current → target}}

## Themes

### {{Theme 1: Outcome statement}}
*Why this matters: {{tie to anchor outcome}}*

| Initiative | Impact | Effort | Target | Status | Dependencies |
|---|---|---|---|---|---|
| {{init_1}} | High → outcome 1 | M | {{period}} | {{status}} | {{deps}} |
| {{init_2}} | Med → outcome 2 | L | {{period}} | Planned | {{deps}} |

### {{Theme 2}}
...

## In flight
*Releases already in progress, with status — read from `products/<product>/releases/`.*
- **{{release_v}} ({{status}})** — target {{date}}, scope: {{summary}}

## Cut from this horizon
*Things considered but consciously deferred. Capture the *why*.*
- **{{cut_initiative}}** — {{reason for deferral}}

## Risks
- {{risk_1}}
- {{risk_2}}

## Open questions
*What we don't know yet that could change this plan.*
- {{question_1}}
- {{question_2}}

## Review cadence
{{when_we_revisit_this}}
```

Apply voice from `identity/voice-profile.md`. Roadmaps tend to be more terse and decisive than PRDs — match the user's planning examples if any exist.

### 11. Show and refine

Show the full draft. *"Look right? Common edits: reorder themes, swap effort estimates, move initiatives between periods, expand a theme, simplify."*

Iterate.

### 12. Save

Save to `products/<product>/roadmap.md`.

In refinement mode (refresh existing horizon or add only):
- Refresh: replace specified horizon section, preserve others
- Add only: append new initiatives to existing themes (or create new themes), preserve placements

*"✓ Saved roadmap to `products/<product>/roadmap.md`."*

Append to `CHANGELOG.md`:
```
- `products/<product>/roadmap.md` — {{Created / Refreshed / Refined}} roadmap for {{horizon}} *(via /plan-roadmap)*
```

### 13. Surface flags

- Outcomes that have no initiatives addressing them — *"Outcome `<X>` has no initiatives. Intentional, or did we miss it?"*
- Initiatives with no clear outcome tie — *"Initiative `<Y>` doesn't ladder up to any anchor outcome. Drop it, or add a missing outcome?"*
- Effort total > capacity — *"You committed to ~{{X}} of work but said capacity was ~{{Y}}. Worth flagging in the next review."*
- OKRs marked informal/absent — *"This roadmap anchors on informal priorities. When OKRs become formal, revisit."*
- Stakeholder priorities not reflected — *"Sponsor `<name>` cares about `<thing>` (per their profile) but no theme addresses it. Intentional?"*

### 14. Suggest next action

- *"Want to start the first initiative? `/new-release <product> <version>` to scaffold the release."*
- *"Share with stakeholders for alignment, then re-run `/plan-roadmap <product> --refine` after feedback."*
- *"Capture decisions or tradeoffs from this planning session via `/add-knowledge` so future roadmaps can reference them."*

## Save location
- Canonical: `products/<product>/roadmap.md`
- No workspace draft — planning skill iterates in conversation

## Missing context hints
- Empty OKRs/KPIs → adapt by anchoring on user-provided plain-language outcomes; flag clearly that the roadmap isn't tied to formal goals
- No in-flight releases → fine; just note "no work currently in flight" in the In-flight section
- No stakeholders linked → flag: *"No stakeholders linked. The roadmap may miss priorities that aren't in the OKRs. Consider linking sponsors first."*
- No prior `knowledge.md` decisions → fine, but mention that capturing tradeoffs from this session would help future planning

## Idempotency
- Existing roadmap → refinement mode (refresh / refine horizon / add only)
- Re-running on a partial draft picks up where it left off; sections completed are preserved unless user asks to redo them

## Output language
- Honors `config.md` `language.output`
- Override via `--lang=<code>`

## Voice consistency
Roadmaps lean terse and decisive — fewer adjectives, more numbers. Imitate user's examples if they exist; otherwise default to that style.

## Examples
- No dedicated example folder for roadmaps; uses voice profile + values + decision-making style from interview

## Next action hint
Most common: capture session decisions via `/add-knowledge`, then start the first initiative via `/new-release`.
