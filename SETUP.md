# Setup

First-run instructions for the Product Operating System.

## Prerequisites

- [Claude Code](https://claude.com/claude-code) installed
- This repo cloned into a directory of your choice
- A few minutes for the interactive walkthrough

## Quick start

Open the repo in Claude Code and run:

```
/setup
```

That's it. The skill walks you through everything below. Each module is independently runnable — you can stop and resume anytime, and re-run individual modules later.

## What `/setup` does

`/setup` is **modular**. Run it without arguments to walk through all modules in order, or invoke a specific module:

### `/setup language`
Sets your preferred language for system messages and generated output. Default is English; configurable to any language. This is asked first because it affects the rest of the setup conversation.

### `/setup identity`
The most important module — this is what makes the OS sound like you.

- Fills in `identity/about-me.md` (role, focus, working style)
- Fills in `identity/values.md` (what you prioritize, what you won't compromise on)
- **Voice profile interview**: asks you to paste 3–5 real samples of your writing (messages, PRDs, tickets) and answers 6–8 calibrated questions about your communication style
- Generates `identity/voice-profile.md` from that conversation

After this module, every skill in the OS will produce output in your voice.

### `/setup company`
Fills in the `company/` layer:

- `about.md` — what the company does
- `okrs.md` — current OKRs (or informal priorities if OKRs aren't formal — the skill handles this gracefully)
- `kpis.md` — same maturity-tiered approach as OKRs
- `teams.md` — your team and key cross-functional partners
- `tools.md` — your stack (JIRA, Teams, etc.)
- `procedures.md` — internal procedures
- `glossary.md` — internal acronyms and codenames

### `/setup product <name>`
Creates your first product folder using `/new-product` under the hood. Coached — asks vision, audience, KPIs, and offers to link stakeholders.

### `/setup workflow`
Defines your end-to-end process. Three options:
- Paste a written process you already have → parsed into the structured format
- Walk through it conversationally → the skill asks "what happens first?", "who reviews?", etc.
- Start from a generic discovery-to-release template and customize

## Re-running setup

`/setup` is idempotent. Re-running:
- Detects existing values and asks before overwriting
- Lets you skip modules that are already done
- Handy for refreshing the voice profile with new examples, or updating OKRs at the start of a new quarter

## What happens after setup

Try one of:

```
/tour                                # learn how the OS thinks (5 min)
/write-prd                           # draft a PRD on something real
/new-release <product> v0.1.0        # scaffold your first release
```

## Getting help

- [README.md](README.md) — high-level overview
- [docs/glossary.md](docs/glossary.md) — what specific terms mean in this OS
- [docs/conventions.md](docs/conventions.md) — how skills behave and why

## Sharing the OS

When you want to share this OS with someone at a different company:

1. They clone the repo
2. The `_template/` folder ships with placeholder content — `/setup` copies from there into the live folders on first run
3. Your private content (`company/`, `products/`, `identity/examples/private/`, `workspace/`, `knowledge.md`) is gitignored and stays with you

See `.gitignore` for the full list of what's private vs. shareable.
