<!--
Workflow definition template — consumed by /new-workflow.
Defines an end-to-end process the user follows for a class of work.
Lives in workflows/<name>.md.
-->

---
name: {{workflow_name}}
trigger: {{what_kicks_this_off}}
status: active                       # active, draft, deprecated
---

# {{workflow_name}}

## Description
<!-- One paragraph. What kind of work this workflow applies to. -->
{{description}}

## Trigger
<!-- The event that starts this workflow. -->
{{trigger_detail}}

## Steps

<!--
Each step should have: number, name, owner, artifact produced, applicable skill.
Mark optional steps with [optional].
If a step doesn't have a skill yet, leave skill: tbd — the user can run /new-action later.
-->

### 1. {{step_1_name}}
- **Owner:** {{owner_1}}
- **Artifact:** {{artifact_1}}
- **Skill:** `{{skill_1}}`
- **Notes:** {{notes_1}}

### 2. {{step_2_name}}
- **Owner:** {{owner_2}}
- **Artifact:** {{artifact_2}}
- **Skill:** `{{skill_2}}`

<!-- Add more steps as needed. -->

## Approval gates
<!-- Points where formal sign-off is required before proceeding. -->
- {{gate_1}}

## Optional steps
<!-- Steps that are part of the workflow but not always run. -->
- {{optional_step}}

## Exit criteria
<!-- How you know the workflow is "done." -->
{{exit_criteria}}

## Related workflows
<!-- Other workflows this might chain to or branch from. -->
- {{related}}
