# templates/

## What lives here
Reusable scaffolds that skills consume when generating artifacts. Each template defines the structure of a specific kind of document.

- `prd.md` — PRD scaffold (for `/write-prd`)
- `jira-ticket.md` — JIRA ticket scaffold (for `/write-jira-tickets`)
- `release-review-slides.md` — release review slide deck (for `/draft-release-review`)
- `workflow-definition.md` — structure for `workflows/<name>.md` files (for `/new-workflow`)
- `skill-scaffold.md` — structure for new skills (for `/new-action`)
- `stakeholder.md` — structure for stakeholder profiles (for `/new-stakeholder`)

## When to edit
- When the structure of an artifact you produce regularly should change → edit the template
- When you want a brand-new artifact type → run `/new-template`

## Which skills read from it
Each generation skill loads its corresponding template. Lighter artifacts (changelog, Teams post, announcement) have their templates inlined inside their skill prompts — fewer files to maintain, less indirection.

## Editing principles
- Templates should be **structural**, not opinionated about content. Voice and tone come from `identity/voice-profile.md`.
- Use `{{placeholder}}` syntax for fields skills will fill in.
- Comments inside `<!-- -->` are read by skills as guidance and stripped from final output.
