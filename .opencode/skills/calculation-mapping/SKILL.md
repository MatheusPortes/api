---
name: Calculation Mapping
description: Map one source-backed calculation mechanic to the API's approved effect, modifier, talent, or independent-skill schema.
---

Use this skill when a selected entity needs calculation fields, a new modifier mapping, or a review of an existing calculation representation.

1. Read `../../specs/entity-calculation-workflow.md`, `../../specs/data-conventions.md`, and `../../specs/entity-research.md`.
2. Identify the exact entity, source clause, affected JSON paths, units, result type, scope, activation, and consumer-only parts before editing.
3. Search `../../history/entity-work-history.md` for the entity ID and the relevant rule key. Read archived examples only when the active rulebook points to them.
4. Reuse an approved rule only when the mechanic's semantics match. Do not infer a schema from similar wording or equal coefficients.
5. For a new or revised general semantic rule, perform the rulebook's retrospective compatibility review before declaring it settled.
6. Preserve native container differences, EN/PT parity, source descriptions, and unsupported mechanics as consumer usage or explicit blockers.
7. Validate source identity, raw coefficients, ranks/levels, JSON, and the focused diff; record the decision and evidence in entity history.

Never use this skill to create a calculation engine, bulk-normalize records, or silently change unrelated entities.
