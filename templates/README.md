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
Each generation skill checks `templates/` for a user-provided override before falling back to its inline default. The override pattern is documented in [../docs/conventions.md](../docs/conventions.md) (convention 15) — file presence is the configuration.

| Skill | Recognized template(s) |
|---|---|
| `/write-prd` | `templates/prd.md` |
| `/write-jira-tickets` | `templates/jira-ticket.md` |
| `/generate-changelog` | `templates/changelog-{customer,internal,developer}.md` |
| `/draft-announcement` | `templates/announcement-{email,in-app,blog,social}.md` |
| `/plan-roadmap` | `templates/roadmap.md` |
| `/new-stakeholder` | `templates/stakeholder.md` |
| `/new-action` | `templates/skill-scaffold.md` |
| `/new-workflow` *(slice 2)* | `templates/workflow-definition.md` |
| `/draft-release-review` *(slice 2)* | `templates/release-review-slides.md` |
| `/draft-teams-post` *(slice 2)* | `templates/teams-post.md` |

If you don't have a custom template for one of the multi-variant skills (e.g., no `changelog-customer.md`), the skill uses its inline default for that variant. Templates are always optional.

## Editing principles
- Templates should be **structural**, not opinionated about content. Voice and tone come from `identity/voice-profile.md`.
- Use `{{placeholder}}` syntax for fields skills will fill in.
- Comments inside `<!-- -->` are read by skills as guidance and stripped from final output.
