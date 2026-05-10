# identity/

## What lives here
Everything that defines **who you are** as a product manager: your role, your values, the way you communicate, and concrete samples of your past writing. This is the layer that makes the OS sound like *you* instead of generic AI.

- `about-me.md` — your role, responsibilities, working style, what you focus on
- `voice-profile.md` — how you write and communicate (formality, structure, words you reach for, things you avoid). Generated during `/setup identity`.
- `values.md` — personal and professional values that guide your decisions
- `examples/` — real samples of your work, organized by artifact type. Skills read these to imitate your structure and tone.
  - `messages/`, `prds/`, `tickets/`, `announcements/`, `release-reviews/`
  - `private/` (gitignored — for unredacted originals)

## When to add to it
- After joining a new company or changing roles → update `about-me.md`
- When you notice the system doesn't sound like you → add more examples and re-run `/setup identity`
- When your communication style evolves → refresh `voice-profile.md`

## Which skills read from it
**Every** skill reads `voice-profile.md` and at least one folder under `examples/`. This layer is the most important context the OS has — keep it current.

## How to populate it
Run `/setup identity` for the first-time interview, or drop new files directly into `examples/<type>/` anytime. Skills pick them up dynamically — no registration step.
