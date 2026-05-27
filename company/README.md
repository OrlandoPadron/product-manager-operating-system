# company/

## What lives here
Everything that defines **where you work**: company facts, goals, people, tools, internal procedures, and acronyms. This is the context skills load to make output relevant to *your* organization.

- `about.md` — what the company does, who it serves, current strategic context
- `okrs.md` — current OKRs (or informal priorities if OKRs aren't formal)
- `kpis.md` — company-level KPIs and how they're tracked
- `teams.md` — org chart, your reporting line, key cross-functional partners
- `tools.md` — the stack you work in (JIRA, Teams, Confluence, etc.) and how you use each
- `procedures.md` — internal procedures and conventions specific to your company
- `glossary.md` — internal acronyms, codenames, and domain terms
- `stakeholders/` — one file per key stakeholder, with their communication style and priorities
- `brand/` — brand guidelines: colors, typography, voice tokens, and a design-system cheat sheet

### `brand/` sub-folder

| File | Contents |
|---|---|
| `brand/colors.md` | Full color palette with hex, RGB, usage rules |
| `brand/typography.md` | Type scale (H1–H6, body, button), font family and weights |
| `brand/voice-and-messaging.md` | Taglines, mission, values, product copy, CTA vocabulary, tone guide |
| `brand/design-system-reference.md` | **The cheat sheet** — single-page token reference loaded automatically by visual skills |

**Auto-loading rule:** If `brand/design-system-reference.md` exists, any skill producing a visual artifact (mockup, Figma frame, slide deck, visual announcement) loads it automatically. No prompt needed — brand is on by default unless the user explicitly overrides it for a specific artifact.

## When to add to it
- After joining a new company → run `/setup company`
- When OKRs change → update `okrs.md`
- When you meet a new key stakeholder → run `/new-stakeholder`
- When a procedure changes → update `procedures.md`
- When the brand refreshes or you switch companies → run `/setup brand`

## Which skills read from it
- `/write-prd`, `/draft-teams-post`, `/draft-announcement`, `/draft-release-review` → all of it
- `/plan-roadmap` → `okrs.md`, `kpis.md`, `about.md`
- Any skill with a named audience → loads matching `stakeholders/*.md`
- **Any visual artifact skill** → loads `brand/design-system-reference.md` if it exists

## How to populate it
Run `/setup company` for the first-time walkthrough (includes a brand step at the end). To configure or refresh brand alone, run `/setup brand`.

After that, edit files directly or use `/new-stakeholder` and `/add-knowledge` to add granular updates.

## Privacy
This entire folder is gitignored by default. Sanitized placeholders live in `_template/company/` for sharing the OS without leaking your company's internals.
