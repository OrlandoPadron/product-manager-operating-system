# workflows/

## What lives here
End-to-end processes specific to your company — the sequence of steps your work actually flows through, from idea to delivery. One markdown file per named workflow.

## Why workflows are user-defined, not system-defined
Every company's process is different. Some have heavy PRD review cycles; some skip PRDs. Some run retros; some don't. The OS doesn't ship with a hardcoded flow because hardcoding would force everyone into someone else's process. Instead: you define your flow, and skills read it to know what step they're in and what comes next.

## Structure of a workflow file
Each workflow file follows the schema in `templates/workflow-definition.md`. It includes:
- Trigger (what kicks the workflow off)
- Steps (ordered, each with owner, artifact produced, and skill that runs it)
- Optional steps (clearly marked)
- Approval gates

## How skills use this
Skills like `/write-prd` and `/write-jira-tickets` can read the active workflow to suggest the next action ("PRD saved. Next in this workflow: `/write-jira-tickets`"). The workflow becomes the navigation.

## When to add a workflow
- During `/setup workflow` (first-time)
- Whenever your company adopts a different flow for a different kind of work (hotfix, experiment, infrastructure migration, etc.) → `/new-workflow`

## Default
The repo ships with `discovery-to-release.md` as a generic example. You can keep it, modify it, or delete and replace.
