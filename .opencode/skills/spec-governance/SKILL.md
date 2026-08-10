---
name: spec-governance
description: Use before planning, editing, or validating application behavior, architecture, schemas, workflows, or AI instructions; maintains the AI specs in .opencode/specs.
---

# Spec Governance

## Required Workflow

1. Read every applicable file in `.opencode/specs/` before taking action.
2. Identify whether the requested change alters application state, behavior, architecture, data schemas, operational workflows, or communication rules.
3. Update the affected spec in the same change whenever such an alteration occurs.
4. If a required spec is absent, create it before treating the work as complete.

## Spec Set

Use `.opencode/specs/` as the project-local source of truth for AI-readable application state.

- `application-state.md` records runtime entrypoints, data and asset models, generation, environment variables, and delivery.
- `communication.md` records language and collaboration requirements.
- `data-conventions.md` records project-specific entity, translation, and asset patterns.
- `entity-research.md` records required external-data validation and citation rules.

## Portable Bootstrap

When this skill is copied to another project, create `.opencode/specs/` and establish at least these files:

- `application-state.md`: purpose, entrypoints, architecture, data model, runtime configuration, validation, and delivery.
- `communication.md`: user language, artifact language, reporting expectations, and validation disclosure.
- `data-conventions.md`: data layouts, record shapes, translation behavior, assets, and compatibility constraints.
- `entity-research.md`: authoritative community sources, required cross-validation, release-status rules, and reference reporting.

Add all applicable specs to the project's OpenCode `instructions` configuration. Keep each spec concise, factual, and updated with the application state it represents.

Follow the repository's Git workflow for AI artifacts. Keep project-specific branch rules in a local AI spec rather than assuming this skill's rules apply to every project.
