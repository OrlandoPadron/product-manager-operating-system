---
name: write-prd
description: Draft a PRD for a release in your voice. Reads identity, company, product, release, and stakeholder context. Uses templates/prd.md as the structural base. Drafts to workspace/ first, then asks before committing to the release folder. Detects existing PRD and switches to refinement (evolve) mode.
---

# /write-prd

## When to use
- Starting the PRD for a release that's been scoped
- Refining an existing PRD with stakeholder feedback (evolve mode)

## What this skill does
Generates a release PRD in your voice, grounded in the context the OS has loaded: your voice profile, your company's strategic situation, the product's state, the release's scope, and the linked stakeholders' priorities.

Saves the draft to `workspace/` first. Only commits to `products/<product>/releases/<version>/prd.md` after explicit user approval.

If a PRD already exists at the canonical path, switches to refinement mode: incorporates new feedback or scope changes without losing prior content.

## Required inputs
- **Product slug** — `<product>` argument
- **Release version** — `<version>` argument

If either is missing, ask:
- If `default_product` is set in `config.md` and product is missing, use it.
- If only product is given, list its releases and ask which one.

## Behavior

### 1. Validate context

- Read `identity/voice-profile.md`. If empty → strong hint: *"Voice profile is empty. The PRD will be much better after `/setup identity`. Continue anyway?"*
- Read `config.md` for output language and formality.
- Resolve `products/<product>/`. If doesn't exist → *"Product `<product>` doesn't exist. Want to create it first via `/new-product`?"*
- Resolve `products/<product>/releases/<version>/`. If doesn't exist → *"Release doesn't exist. Want to scaffold it first via `/new-release <product> <version>`?"*

### 2. Load context

Read in this order:
- `identity/voice-profile.md` — voice
- `identity/values.md` — what user prioritizes (helps with framing)
- `identity/examples/prds/` — every example file in this folder, to imitate structure and tone
- `config.md` — language, formality, time zone
- `company/about.md` — strategic context
- `company/okrs.md` — current OKRs (note maturity tier from first line of prose)
- `company/kpis.md` — KPIs to potentially reference
- `company/procedures.md` — review process to mention if relevant
- `products/<product>/about.md` — product profile, KPIs, stakeholders
- `products/<product>/releases/<version>/about.md` — release scope, target date, status, stakeholders
- For each stakeholder linked in product or release frontmatter: `company/stakeholders/<name>.md`
- `templates/prd.md` — structural scaffold

### 3. Detect refinement mode

If `products/<product>/releases/<version>/prd.md` already exists and is non-empty (not just the placeholder):
- *"A PRD already exists for this release. What do you want to do?"*
  - **Refine** — incorporate new feedback or context, preserving structure
  - **Regenerate** — start fresh from the current context (warns: existing content will be replaced)
  - **Diff against current draft** — show what would change without saving

### 4. Gather missing inputs

For first draft, ask:
- *"What's the core problem this release solves? Be specific — what gets worse without it?"*
- *"What's the high-level solution? (You can describe in plain English; I'll structure it in the PRD.)"*
- *"What's the evidence — customer quotes, support tickets, metrics, research findings? Cite sources."*
- *"Any open questions or things you don't know yet?"*
- *"Risks worth flagging?"*

For refinement, ask:
- *"What changed? Paste the feedback or describe what needs updating."*
- Apply changes section by section, preserving everything else.

### 5. Draft the PRD

Use `templates/prd.md` as structural base. Apply:
- Voice from `identity/voice-profile.md` and patterns from `identity/examples/prds/`
- Stakeholder priorities — for each linked stakeholder, weight what they care about into the framing (e.g., if a stakeholder cares about technical risk, expand the Risks section; if another cares about customer impact, expand the Evidence section)
- Numbers, KPIs, OKRs only as cited from the actual loaded files. **Never invent metrics.** If a number is needed and not available, leave a `{{TBD: <description>}}` placeholder and flag at the end.
- Output language from `config.md`

### 6. Save draft to workspace

Save to `workspace/prd-<product>-<version>.md`.

*"✓ Drafted PRD to `workspace/prd-<product>-<version>.md`. Want me to open it / show you a summary / commit it to the release folder?"*

### 7. Iterate

Show the draft (or summary). User feedback options:
- *"Looks good — commit to release folder"*
- *"Edit section X to do Y"*
- *"Regenerate this section with these changes"*
- *"Add a section on Z"*

Apply changes to the workspace draft. Iterate.

### 8. Commit to canonical

When user approves:
- Move (or copy and delete) from `workspace/prd-<product>-<version>.md` to `products/<product>/releases/<version>/prd.md`.
- *"✓ Committed PRD to `products/<product>/releases/<version>/prd.md`."*

Append to `CHANGELOG.md`:
```
- `products/<product>/releases/<version>/prd.md` — {{Drafted / Refined}} PRD *(via /write-prd)*
```

### 9. Surface flags

If any of these came up during generation, flag them at the end:
- `{{TBD}}` placeholders left in the doc
- Open questions the PRD raised
- Stakeholder profiles that were referenced but missing key info (e.g., "Jane's profile doesn't list her decision-making lens — consider running `/new-stakeholder jane-smith` to fill it in")
- KPIs referenced but marked as "informal" or "absent" in source files

### 10. Suggest next action

Read `workflows/<active>.md` (from release frontmatter or default `discovery-to-release.md`). The next step is typically PRD review by stakeholders, then `/write-jira-tickets`.

*"Next in this workflow: share for stakeholder review, then `/write-jira-tickets <product> <version>` once feedback is incorporated."*

## Save location
- Draft: `workspace/prd-<product>-<version>.md`
- Canonical: `products/<product>/releases/<version>/prd.md`
- CHANGELOG: appends one-liner on commit

## Missing context hints
- Empty voice profile → hint and offer to continue
- No product KPIs → flag at end of draft, propose adding via `/add-knowledge` or product `about.md` edit
- No stakeholders linked → flag: *"This release has no linked stakeholders. The PRD will be generic without that context. Want to link stakeholders first via `/link-stakeholder`?"*
- No prior PRD examples → flag: *"You have no PRDs in `identity/examples/prds/` yet. The output will follow the standard template structure. Add examples to make future PRDs sound more like you."*

## Idempotency
- Existing PRD → refinement mode (preserves content not explicitly being changed)
- Workspace drafts can be regenerated freely without affecting canonical

## Output language
- Honors `config.md` `language.output`
- Override via `--lang=<code>` argument

## Voice consistency
This skill's job is to make the PRD sound like *the user* wrote it. The template is structural scaffolding. The voice profile and examples are the actual style source. When in doubt, lean on the examples — concrete imitation beats abstract instruction.

## Examples
- `identity/examples/prds/` — read every file here on each invocation. The skill should imitate structure, section ordering, level of detail, and tonal patterns.

## Next action hint
After commit: typically `/write-jira-tickets <product> <version>` once reviewers have approved. The skill reads the active workflow to suggest the right next step.
