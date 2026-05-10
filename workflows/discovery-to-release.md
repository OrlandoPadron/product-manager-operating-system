---
name: Discovery to Release
trigger: A new feature request, problem statement, or initiative is identified
status: active
---

# Discovery to Release

## Description
A generic end-to-end workflow for taking a product idea from initial discovery through development, release, and review. **This is an example shipped with the OS** — you should customize it (or replace it via `/new-workflow`) to match how your company actually works.

## Trigger
A problem worth solving has been identified — from customer signal, internal request, OKR alignment, or strategic priority.

## Steps

### 1. Problem identification & framing
- **Owner:** PM
- **Artifact:** Problem statement (in `workspace/` or `products/<x>/discovery/`)
- **Skill:** `/add-knowledge` (for capturing context) — full discovery skill TBD
- **Notes:** Be specific. Who has the problem, when, and what gets worse if we don't solve it.

### 2. Discovery
- **Owner:** PM (with design, eng input)
- **Artifact:** Discovery notes
- **Skill:** *tbd — run `/new-action` to define one*
- **Notes:** Customer interviews, data analysis, competitive research, technical exploration.

### 3. PRD creation
- **Owner:** PM
- **Artifact:** `products/<x>/releases/<v>/prd.md`
- **Skill:** `/write-prd`

### 4. PRD review
- **Owner:** Linked stakeholders (sponsor, tech lead, design lead)
- **Artifact:** Updated PRD with comments incorporated
- **Skill:** `/write-prd` (re-invoked in evolve mode)
- **Notes:** Iterate until reviewers approve.

### 5. Ticket writing
- **Owner:** PM
- **Artifact:** `products/<x>/releases/<v>/tickets/*.md`
- **Skill:** `/write-jira-tickets`
- **Notes:** Each ticket maps to a concrete unit of dev work.

### 6. Ticket review
- **Owner:** Product, Dev, QA leads
- **Artifact:** Refined tickets
- **Skill:** `/write-jira-tickets` (evolve mode)

### 7. Development
- **Owner:** Engineering
- **Artifact:** Working code
- **Skill:** *(out of scope for the OS)*

### 8. Smoke test / review
- **Owner:** PM, QA
- **Artifact:** Sign-off on tickets
- **Skill:** *tbd*

### 9. Release announcement & Teams post
- **Owner:** PM
- **Artifact:** `products/<x>/releases/<v>/announcement.md`, `teams-post.md`
- **Skill:** `/draft-announcement`, `/draft-teams-post`

### 10. Changelog
- **Owner:** PM
- **Artifact:** `products/<x>/releases/<v>/changelog.md`
- **Skill:** `/generate-changelog`

### 11. Release review
- **Owner:** PM
- **Artifact:** `products/<x>/releases/<v>/release-review.md`
- **Skill:** `/draft-release-review`
- **Notes:** Slides recapping what shipped, metrics, what's next.

## Approval gates

- After step 4 (PRD review) — requires sponsor approval before tickets are written
- After step 6 (ticket review) — requires Dev lead approval before development starts
- Before step 9 (release announcement) — requires QA sign-off

## Optional steps
- **Discovery doc** (step 2) — sometimes problems are well-understood enough to skip directly to PRD
- **Hosting a retrospective** — not part of the default OS but can be added if your team runs them

## Exit criteria
The release is shipped, the changelog is published, and the release review has been delivered to the relevant audience.

## Related workflows
- **Hotfix** — abbreviated workflow for urgent fixes (skip discovery, lighter PRD)
- **Experiment** — lighter weight, includes hypothesis and success threshold

(These don't ship by default. Run `/new-workflow` to add them.)
