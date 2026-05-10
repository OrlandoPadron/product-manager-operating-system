---
name: write-jira-tickets
description: Break a PRD into JIRA tickets in your voice. Reads the release's PRD, identity, ticket examples, and templates/jira-ticket.md. Generates one file per ticket. Drafts to workspace/ first, then commits to the release's tickets/ folder. Detects existing tickets and switches to refinement mode (add, update, remove without overwriting work-in-progress).
---

# /write-jira-tickets

## When to use
- After a PRD has been drafted and is ready to be broken into concrete dev work
- Adding new tickets to an in-flight release
- Refining existing tickets after a Dev/QA lead review

## What this skill does
Reads the release's PRD and decomposes it into discrete JIRA tickets. Generates one markdown file per ticket in your voice, using `templates/jira-ticket.md` as the structural base. Drafts the full set in `workspace/` first; commits to canonical only after you approve.

In refinement mode (when tickets already exist), works additively: adds new tickets you describe, updates specific existing ones, optionally removes obsolete ones — never blanket-overwrites the folder.

## Required inputs
- **Product slug** — `<product>` argument
- **Release version** — `<version>` argument

If `default_product` is set in `config.md` and product is omitted, use it.

## Behavior

### 1. Validate context

- Read `identity/voice-profile.md`. Hint if empty.
- Read `config.md` for output language and project conventions.
- Resolve the release folder. If missing → offer `/new-release`.
- Read `products/<product>/releases/<version>/prd.md`. If empty or placeholder: *"PRD is empty for this release. Run `/write-prd <product> <version>` first — tickets without a PRD will miss context."* Continue if user insists.

### 2. Load context

- `identity/voice-profile.md` — voice
- `identity/examples/tickets/` — every example file (imitate structure, level of detail, definition-of-done style)
- `config.md`
- `company/tools.md` — JIRA project key, ticket types in use (Story, Bug, Task, Spike, etc.), workflow status names
- `company/procedures.md` — definition of done if defined, ticket review process
- `products/<product>/about.md` — product KPIs and stakeholders
- `products/<product>/releases/<version>/about.md` — release scope, status
- `products/<product>/releases/<version>/prd.md` — the PRD (primary source)
- For each linked stakeholder (especially tech-lead role): `company/stakeholders/<name>.md` — to weight technical depth and clarity expectations
- `templates/jira-ticket.md` — structural base
- Existing tickets in `products/<product>/releases/<version>/tickets/` if any (for refinement mode)

### 3. Detect refinement mode

Check `products/<product>/releases/<version>/tickets/`:

- Empty or only `.gitkeep` → **first-draft mode** (proceed to step 4).
- Contains tickets already → **refinement mode**:
  - *"This release has {{N}} existing tickets. What do you want to do?"*
    - Add new tickets (describe what to add)
    - Update specific ticket(s) — name them
    - Re-derive from PRD (warns: existing in-progress estimates / assignees may be lost; offers to preserve frontmatter)
    - Diff against current set without changing anything

### 4. Decompose the PRD into tickets

Walk through the PRD's scope and user-flow sections. For each concrete unit of work, propose a ticket with:
- A short, descriptive title
- Type (Story / Bug / Task / Spike — picked from `company/tools.md` if specified, else default to Story)
- Summary, context, acceptance criteria, technical notes, out-of-scope, dependencies, definition of done

Group cross-cutting work (telemetry, copy, design QA) into separate tickets — don't bury it inside feature tickets.

### 5. Show ticket list and confirm scope

Before drafting full ticket content, show the *list*:

> *"Proposed tickets ({{N}}):*
> *1. [Story] Implement checkout summary header — captures L1 of new flow*
> *2. [Story] Add address validation step — covers L2*
> *3. [Task] Update telemetry events for new flow*
> *4. [Spike] Investigate payment SDK upgrade compatibility*
> *...*
> *Look right? Want to add, remove, or merge any before I draft them?"*

Iterate on the *list* before drafting full content — cheaper than rewriting drafted tickets.

### 6. Draft each ticket

For each ticket on the approved list:
- Use `templates/jira-ticket.md` as structural base
- Apply voice from `identity/voice-profile.md` and tonal patterns from `identity/examples/tickets/`
- Keep technical depth proportional to the user's example tickets — don't over-engineer if their style is terse, don't under-explain if their style is detailed
- Frontmatter:
  ```yaml
  ---
  key: TBD                          # placeholder — assigned in JIRA after creation
  title: {{ticket_title}}
  type: {{type}}
  parent: {{epic_or_release_key}}
  release: {{version}}
  status: To Do
  estimate: TBD
  ---
  ```
- Filename: `<key>.md` if a JIRA key is known; otherwise `<NN>-<slug>.md` where NN is the order number (e.g., `01-checkout-summary-header.md`)

### 7. Save drafts to workspace

Save to `workspace/tickets-<product>-<version>/`:

```
workspace/tickets-<product>-<version>/
├── 01-checkout-summary-header.md
├── 02-address-validation.md
└── ...
```

*"✓ Drafted {{N}} tickets to `workspace/tickets-<product>-<version>/`. Show me a specific one / show all summaries / commit all to the release folder?"*

### 8. Iterate

Per-ticket feedback options:
- *"Ticket 3 needs more technical detail — add a section on the data migration"*
- *"Merge tickets 5 and 6"*
- *"Split ticket 2 — UI work and validation logic should be separate"*
- *"Regenerate ticket 4 with these acceptance criteria: ..."*

Apply changes to workspace drafts. Show diffs when requested.

### 9. Commit to canonical

When the user approves the set:

- Move each file from `workspace/tickets-<product>-<version>/` to `products/<product>/releases/<version>/tickets/`
- In refinement mode, only touch files explicitly being added/updated/removed; preserve unrelated existing tickets

*"✓ Committed {{N}} tickets to `products/<product>/releases/<version>/tickets/`."*

Append to `CHANGELOG.md`:
```
- `products/<product>/releases/<version>/tickets/` — {{Drafted / Updated}} {{N}} tickets *(via /write-jira-tickets)*
```

### 10. Surface flags

At the end, surface:
- Tickets with `TBD` placeholders the user should fill in (estimates, JIRA keys once assigned)
- Dependencies between tickets that should be reflected in JIRA links once created
- Anything in the PRD that didn't map cleanly to a ticket (open questions, unscoped sections)

### 11. Suggest next action

Based on `workflows/<active>.md`. Typically: ticket review by Dev/QA leads, then development.

*"Next: share with Dev/QA leads for review. After their feedback, re-run `/write-jira-tickets <product> <version>` in refinement mode to update."*

## Save location
- Draft: `workspace/tickets-<product>-<version>/`
- Canonical: `products/<product>/releases/<version>/tickets/`
- CHANGELOG: appends one-liner on commit

## Missing context hints
- Empty PRD → strong warning, offer to back out
- No ticket examples in `identity/examples/tickets/` → flag: *"No ticket examples loaded. Tickets will follow the standard template structure. Add examples to make future tickets sound more like you."*
- `company/tools.md` doesn't specify JIRA project key or ticket types → use generic defaults and flag: *"Project key and ticket types aren't defined in `company/tools.md` — using defaults. Update `tools.md` for better fidelity."*
- Tech-lead stakeholder profile missing → flag if release has a tech-lead linked but no profile

## Idempotency
- Existing tickets → refinement mode (additive, surgical)
- Workspace can be regenerated without affecting canonical
- Re-running on a partial commit picks up where it left off

## Output language
- Honors `config.md` `language.output`
- Override via `--lang=<code>`

## Voice consistency
Tickets are short artifacts where voice signal is concentrated in: how acceptance criteria are phrased, level of technical detail in notes, what's marked out-of-scope, definition-of-done style. Imitate `identity/examples/tickets/` closely.

## Examples
- `identity/examples/tickets/` — read every file on each invocation

## Next action hint
Typically: ticket review → development. The skill reads the active workflow to suggest the right next step.
