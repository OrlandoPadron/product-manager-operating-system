---
language:
  system: en
  output: en
voice_formality: professional-casual
time_zone: Europe/Madrid
default_product: TBD
---

# Configuration

System-wide preferences. Skills read this on every invocation.

## Fields

- **`language.system`** — language for skill prompts and internal artifacts (default `en`)
- **`language.output`** — language for generated user-facing content (default `en`). Override per skill via `--lang=<code>`.
- **`voice_formality`** — overall tone setting: `formal` / `professional-casual` / `casual`. Most skills will adapt further based on audience and context.
- **`time_zone`** — your time zone in IANA format (e.g., `Europe/Madrid`, `America/New_York`). Used for date formatting.
- **`default_product`** — the product folder name to use when a skill is invoked without an explicit product argument. Useful if you mostly work on one product.

## Updating

Edit this file directly, or run `/setup language` (or any specific module) to update interactively.

## First-run state

If this file still contains `<run /setup to fill in>` placeholders, skills should detect this and prompt the user to run `/setup` before producing serious output.
