---
name: new-product
description: Coached creation of a new product. Asks for vision, audience, KPIs, and links stakeholders. Scaffolds the product folder with about.md, roadmap.md, and an empty releases/ folder. Detects existing products and switches to evolve mode.
---

# /new-product

## When to use
- Adding a new product to your portfolio
- Refreshing an existing product's `about.md` or `roadmap.md`

## What this skill does
Creates a new `products/<name>/` folder with `about.md`, `roadmap.md`, and `releases/` (empty). The `about.md` is filled in via coached interview — vision, audience, KPIs, stakeholders. Detects existing products and switches to evolve mode (refines the existing `about.md` rather than overwriting).

Coached creation: propose → critique → refine → save. Never produces a blank scaffold.

## Required inputs
- **Product name** — `<name>` argument or asked interactively. Will be lowercased and hyphenated for the folder name.

## Behavior

### 1. Validate context
- Read `config.md`. If first-run state, hint: *"You haven't run `/setup` yet — `/new-product` will work but `about.md` will be richer if your company context is loaded first."*
- Read `identity/voice-profile.md`. If empty, hint: *"Run `/setup identity` first to make `about.md` sound like you."*
- Check if `products/<name>/` already exists. If yes → **evolve mode** (jump to step 6).

### 2. Gather product basics

Ask in order:
- *"What's the product called? (Use the official internal name — I'll handle slug formatting.)"*
- *"One-paragraph elevator pitch — what does it do, who is it for?"*
- *"What problem does it solve? Be concrete — what gets worse without it?"*
- *"Who are the users? Are they the same as the buyers, or different?"*
- *"What stage is the product in? (Discovery / MVP / growing / mature / sunset)"*
- *"What's the strategic context? Why now, why this product, why this team?"*

### 3. Gather KPIs

- *"What KPIs are you measured on for this product? (If none are formal, that's fine — just describe what success looks like.)"*
- For each KPI: *"Current value? Target? Where do you check it?"*
- If user has no KPIs at all: write *"KPIs not yet defined for this product."* and offer to revisit later.

### 4. Stakeholders

- Read `company/stakeholders/`. List existing profiles by name.
- *"Which of these stakeholders are involved with this product? You can multi-select, or add new ones."*
- For each selected: *"What role do they play? (sponsor / reviewer / decision-maker / tech-lead / design-partner / etc.)"* — free-form, no fixed list.
- For new stakeholders: chain into `/new-stakeholder` for each, then return.

### 5. Roadmap (lightweight)

- *"Want to outline the high-level roadmap now? You can do this later via `/plan-roadmap` (coming in slice 2). For now, I can scaffold an empty `roadmap.md` or capture a few bullets."*

### 6. Evolve mode (if product exists)

- Read existing `products/<name>/about.md`.
- *"This product already exists. What do you want to update? (vision / audience / KPIs / stakeholders / strategic context / all)"*
- Apply the relevant subset of steps 2–5 in evolve mode: show current values, ask what to change, regenerate, diff.

### 7. Draft and confirm

- Generate `about.md` from collected info, applying voice from `identity/voice-profile.md`.
- Frontmatter includes:
  ```yaml
  ---
  name: {{Product Name}}
  slug: {{product-slug}}
  stage: {{stage}}
  stakeholders:
    - name: {{stakeholder-slug}}
      role: {{role}}
  ---
  ```
- Show draft in full. *"Look right? (yes / let me edit / regenerate with these changes: ...)"*
- Iterate until user approves.

### 8. Save and confirm

- Create folder structure:
  ```
  products/<slug>/
    about.md         # the drafted content
    roadmap.md       # placeholder or user's outlined bullets
    releases/        # empty
  ```
- *"✓ Created product at `products/<slug>/`.
   - `about.md` — your product profile
   - `roadmap.md` — placeholder; run `/plan-roadmap` later to fill it in
   - `releases/` — empty; run `/new-release <slug> <version>` to start your first release"*
- Append to `CHANGELOG.md`:
  ```
  - `products/<slug>/` — Created product *(via /new-product)*
  ```

### 9. Suggest next action

- If no releases yet: *"Next: `/new-release <slug> v0.1.0` to start your first release."*
- If user mentioned upcoming work in step 2: suggest the relevant skill.

## Save location
- Canonical: `products/<slug>/about.md`, `products/<slug>/roadmap.md`, `products/<slug>/releases/` (folder)
- No draft step for the folder structure itself; the `about.md` content is iterated in conversation before saving directly to canonical.

## Missing context hints
- Empty voice profile → *"`about.md` will be more accurate after `/setup identity`."*
- No company context → *"`about.md` won't reflect your company's strategic situation. Consider `/setup company` first."*
- No stakeholders defined yet → *"You can skip linking stakeholders for now and add them later via `/link-stakeholder <name>`."*

## Idempotency
- Existing product → evolve mode (refine, don't overwrite)
- Re-running on a partially configured product picks up where it left off

## Naming convention
- Folder name: lowercase, hyphenated. `Checkout V2` → `checkout-v2`.
- The user's preferred display name is preserved in frontmatter `name:`.

## Examples to imitate
None on the scaffolding side — `about.md` voice comes from `identity/voice-profile.md` and `identity/examples/messages/` (closest tonal proxy for product-internal docs).
