---
name: link-stakeholder
description: Fast path to link an existing stakeholder profile to one or more products and/or releases. Doesn't edit the profile — only adds forward references in the target's frontmatter. Use this when you don't need to change the profile itself.
---

# /link-stakeholder

## When to use
- Starting a new release and pulling in stakeholders that already have profiles
- Adding a stakeholder to a product they weren't previously linked to
- Quickly building out a release's stakeholder list without going through the full `/new-stakeholder` flow

## What this skill does
Adds forward references to existing stakeholder profiles in the frontmatter of products and/or releases. Doesn't touch the stakeholder file itself. Multi-target by design — link one stakeholder to several products/releases in one invocation.

For creating or editing the stakeholder profile itself, use `/new-stakeholder`.

## Required inputs
- **Stakeholder name** — `<name>` argument or asked. Must match an existing profile slug.

## Behavior

### 1. Validate context

- Read `company/stakeholders/`. List available profiles.
- If user passed a name: resolve to a slug. If no exact match, fuzzy-match and confirm: *"Did you mean `jane-smith`?"*
- If no profile exists: *"`<name>` doesn't have a profile yet. Want to create one via `/new-stakeholder`?"*

### 2. Choose targets

- *"Where do you want to link `{{name}}`?"*
- Show two options:
  - **Product** — read `products/`, multi-select
  - **Release** — read `products/<x>/releases/`, multi-select. Show release status (e.g., "v1.2.0 [In dev]") so user can pick the right one.

User can pick any combination.

### 3. Choose role

For each target, ask:
- *"What role on `<target>`? (sponsor / reviewer / decision-maker / tech-lead / design-partner / approver / FYI / other)"*

If the same role applies to all targets in this batch: *"Use this role for all? (yes/no)"*

### 4. Detect duplicates

- For each target, read existing frontmatter.
- If `{{name}}` is already in the `stakeholders:` list with the same role: *"`{{name}}` is already linked to `<target>` as `{{role}}`. Skip."*
- If linked with a *different* role: *"`{{name}}` is currently `<old_role>` on `<target>`. Update to `<new_role>`? (yes/no/skip)"*

### 5. Write links

For each target:
- Read existing frontmatter.
- Add (or update) entry in `stakeholders:`:
  ```yaml
  stakeholders:
    - name: {{slug}}
      role: {{role}}
  ```
- Preserve all other frontmatter and content unchanged.

### 6. Confirm

*"✓ Linked `{{name}}` to:
- `products/<product>/about.md` as `<role>`
- `products/<product>/releases/<version>/about.md` as `<role>`
"*

Append to `CHANGELOG.md`:
```
- `products/<product>/about.md` — Linked {{name}} as {{role}} *(via /link-stakeholder)*
```

### 7. Suggest next action

- If user linked to a release: *"Skills like `/draft-teams-post <product> <version>` will now tailor output for `{{name}}`."*
- *"Want to link another stakeholder, or someone else? Just run `/link-stakeholder <name>` again."*

## Save location
- Modifies frontmatter of `products/<product>/about.md` and/or `products/<product>/releases/<version>/about.md`
- Never modifies `company/stakeholders/<name>.md` (that's `/new-stakeholder`'s job)

## Missing context hints
- Stakeholder doesn't exist → offer `/new-stakeholder`
- No products yet → *"Create a product first via `/new-product`."*
- No releases for the selected product → *"This product has no releases yet. Linking to product only — you can link to a release later."*

## Idempotency
- Detects duplicate links and asks before overwriting
- Always safe to re-run

## Conventions
- Forward links only — the stakeholder file is never modified by this skill (see `docs/conventions.md`)
- One source of truth per relationship — if a stakeholder needs to be unlinked, edit the product/release frontmatter directly or build `/unlink-stakeholder` later

## Next action hint
After linking, point at the skill that will use the new context (e.g., `/write-prd`, `/draft-teams-post`).
