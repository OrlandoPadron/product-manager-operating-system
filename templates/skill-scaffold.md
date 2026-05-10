<!--
Skill scaffold — consumed by /new-action.
This is what gets generated when the user teaches the OS a new recurring action.
Lives in .claude/skills/<skill-name>/SKILL.md.
-->

---
name: {{skill_name}}
description: {{one_line_description}}
---

# /{{skill_name}}

## When to use
<!-- One paragraph. The trigger condition for invoking this skill. -->
{{when_to_use}}

## What this skill does
<!-- Plain-English summary of behavior. -->
{{what_it_does}}

## Context to read
<!-- Always include identity/voice-profile.md and config.md. List other context this skill needs. -->
- `identity/voice-profile.md` (always)
- `identity/examples/{{example_type}}/` (if applicable)
- `config.md` (always)
- {{additional_context}}

## Required inputs
<!-- What the user must provide as arguments or in conversation. -->
- {{input_1}}
- {{input_2}}

## Behavior

<!-- The actual instructions Claude follows. This is where the skill's logic lives. -->

### 1. Validate context
<!-- Check that required files exist. If voice-profile.md is empty, hint the user to run /setup identity. -->

### 2. Gather inputs
<!-- Ask the user the questions needed to produce a good output. Coached creation pattern if this is a /new-* skill. -->

### 3. {{skill_specific_step}}

### 4. Save and confirm
<!-- Write to canonical location. Show the path. Append to CHANGELOG.md if this is a meaningful artifact. -->

### 5. Suggest next action
<!-- If applicable, end with a one-liner pointing at what's typically next. -->

## Save location
<!-- Where output goes. Should be under workspace/ first if a draft, then prompted to canonical location. -->
- Draft: `workspace/{{draft_subpath}}`
- Canonical: `{{canonical_path}}`

## Missing context hints

<!-- What the skill should say when context is missing. -->
- If `identity/voice-profile.md` is empty: *"Run `/setup identity` to make output sound like you."*
- If {{specific_missing_context}}: *"{{specific_hint}}"*

## Examples
<!-- Reference examples this skill should imitate. -->
- `identity/examples/{{example_type}}/`
