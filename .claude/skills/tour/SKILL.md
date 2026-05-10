---
name: tour
description: Interactive 5-minute walkthrough of how the Product Operating System thinks. Different from /setup (which configures) — /tour just teaches the mental model.
---

# /tour

## When to use
- First time encountering the OS and wanting to understand it before configuring
- After someone shared the OS with you and you're orienting
- Anytime you want a refresher on how the layers fit together

## What this skill does
Walks the user through the OS's mental model conceptually. Doesn't change any files. Doesn't configure anything. Just explains and shows examples.

Goal: leave the user with a clear mental model in ~5 minutes, ready to use the system intentionally.

## Behavior

### 1. Open with the elevator pitch

> *"This is a Product Operating System — a structured set of context files and Claude skills that turn your repeating PM work into one-command operations that sound like you. Let me show you how it thinks."*

Ask: *"Have you used Claude Code before, or is this your first time? I'll calibrate."*

### 2. The six layers

Walk through each layer in order, briefly. Show the actual folder by reading its README. Explain in plain English what each is for.

For each layer, give an example of *what would live there*:

- **`identity/`** → "Your voice profile, your values, your past PRDs."
- **`company/`** → "Your company's OKRs, who's who on your team."
- **`products/`** → "Each product you ship gets a folder. Each release gets a sub-folder with its PRD, tickets, announcements."
- **`workflows/`** → "Your end-to-end process. The OS doesn't assume how you work — you tell it."
- **`templates/`** → "Scaffolds for PRDs, tickets, slide decks. Skills consume these."
- **`knowledge.md`** → "Cross-cutting things you've learned that don't belong to a specific company file or product."

### 3. The two skill patterns

> *"Skills come in two flavors. Knowing which is which makes the OS easy to navigate."*

- **Coached creation** (`/new-*`, `/add-knowledge`): the skill asks questions, drafts something, lets you iterate, and only saves when you approve. *"You never get a blank file."*
- **Content generation** (`/write-prd`, `/draft-teams-post`, etc.): the skill reads context — your voice, your company, the relevant product — and produces a draft in your voice. *"It does the boilerplate so you can focus on the judgment."*

### 4. Walk through a release lifecycle (the payoff)

> *"Let's trace a typical release through the system."*

Use a hypothetical product called `checkout-v2` and version `v1.2.0`. For each step, name the skill, what it does, what file lands where:

1. *"Problem identified → captured in `workspace/`"*
2. *"`/new-release checkout-v2 v1.2.0` → scaffolds `products/checkout-v2/releases/v1.2.0/` with `about.md`"*
3. *"`/write-prd checkout-v2 v1.2.0` → reads voice + product + stakeholders, drafts PRD into `workspace/`, asks before committing"*
4. *"`/write-jira-tickets` → breaks the PRD into ticket files"*
5. *"Dev happens (outside the OS)"*
6. *"`/draft-announcement`, `/draft-teams-post`, `/generate-changelog` → ship-time artifacts"*
7. *"`/draft-release-review` → produces slide deck for the release review"*

> *"At every step, the OS reads your voice profile, your stakeholders, your past examples. The output sounds like you because it's literally drawing from your own writing."*

### 5. The extension story

> *"Anything you do regularly that isn't a skill yet? Run `/new-action`. It interviews you, takes a couple of examples, and generates a new skill that fits the OS's conventions. The system grows with your work."*

### 6. Privacy and sharing

Brief mention. *"Your real content (`company/`, `products/`, `knowledge.md`, private examples) is gitignored by default. Want to share the OS with a friend at another company? They clone, run `/setup`, and get their own private instance. The shareable template ships with placeholder content in `_template/`."*

### 7. Where to go next

End with a menu:

> *"Three good next steps:*
> - *`/setup` if you haven't configured the OS yet*
> - *`/write-prd` if you have something to draft right now*
> - *Open `README.md` for the full skill index, or `docs/glossary.md` if any of the terms I used were unclear*
> *"*

## Context to read
- All folder READMEs (briefly skim to ground the explanation in actual file paths)
- `README.md` and `docs/glossary.md` (to stay consistent with the canonical docs)

## Save behavior
- **None.** This skill is read-only. It does not modify any files.

## Idempotency
- Always safe to re-run. Adapts depth based on whether the OS is configured or not — if `config.md` is filled in, references the user's real products/identity by name; if not, uses generic examples.

## Next action hint
End with the menu above. Don't push any single action — let the user choose.
