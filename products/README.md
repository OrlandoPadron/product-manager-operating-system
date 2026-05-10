# products/

## What lives here
One folder per product you work on. Inside each product: an `about.md`, a `roadmap.md`, and a `releases/` folder containing one subfolder per shipped or in-flight release.

## Structure

```
products/
└── <product-name>/
    ├── about.md                       # Vision, audience, KPIs, stakeholders
    ├── roadmap.md                     # Forward-looking plan
    └── releases/
        └── <version>/
            ├── about.md               # Release metadata: scope, stakeholders, dates
            ├── prd.md                 # The PRD for this release
            ├── tickets/               # One file per JIRA ticket
            │   ├── PROJ-123.md
            │   └── ...
            ├── announcement.md        # External / customer-facing announcement
            ├── teams-post.md          # Internal Teams/Slack announcement
            ├── changelog.md           # Release changelog
            └── release-review.md      # Slides for the release review (markdown with --- separators)
```

## When to add to it
- New product to your portfolio → `/new-product`
- New release for an existing product → `/new-release <product> <version>`

## Which skills read from it
- `/write-prd` writes to `products/<x>/releases/<v>/prd.md` after reading product context, stakeholders, and release scope
- `/write-jira-tickets`, `/generate-changelog`, `/draft-teams-post`, `/draft-announcement`, `/draft-release-review` all write to release subfolders
- `/plan-roadmap` reads and writes `products/<x>/roadmap.md`

## Privacy
This entire folder is gitignored by default. The structure is documented in `_template/products/` for the shareable OS template.
