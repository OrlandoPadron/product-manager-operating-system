# company/stakeholders/

## What lives here
One markdown file per key stakeholder. Each profile captures how to communicate effectively with that person — their role, what they care about, what they don't, communication preferences, past interactions worth remembering.

## Filename convention
`<first-last>.md` (lowercase, hyphenated). Example: `jane-smith.md`, `john-doe.md`.

## Profile structure
Each file is created by `/new-stakeholder` and follows this shape:

```markdown
---
name: Jane Smith
role: Engineering Lead
team: Platform
---

# Jane Smith

## What she cares about
...

## Communication style
...

## What to avoid
...

## Past interactions worth noting
...
```

## How profiles get linked to products and releases
Linking is forward-only — the product or release lists the stakeholder, not the other way around. Stakeholder files stay clean profiles. See `products/<product>/about.md` and `products/<product>/releases/<v>/about.md` for the frontmatter shape.

## How profiles get used
When a skill produces content for a release or product (`/draft-teams-post`, `/write-prd`, `/draft-release-review`), it reads the linked stakeholders' profiles and tailors tone, emphasis, and what gets included or omitted.

## Skills that work with this folder
- `/new-stakeholder` — create or edit a profile (coached)
- `/link-stakeholder` — link an existing stakeholder to a product or release without editing the profile
