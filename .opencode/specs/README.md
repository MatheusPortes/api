# AI Spec Index

Read this file first. It is the always-loaded routing layer; read only the additional files needed for the task.

## Always Applicable

- `communication.md` — language, reporting, and collaboration constraints.
- `application-state.md` — runtime, data layout, active maintenance state, and pointers.

## Task-Based Loading

| Task | Read before acting |
| --- | --- |
| Any served data or image change | `data-conventions.md` |
| External factual research or release status | `entity-research.md` |
| Calculation fields, effects, modifiers, progression, or artifact mechanics | `entity-calculation-workflow.md` and the relevant history entries |
| Commit, delivery branch, worktree, or push | `git-workflow.md` |
| A prior entity decision, source conflict, or closed follow-up | the matching section of `.opencode/history/entity-work-history.md` |

## Project Skills

Skills are discovered from `.opencode/skills/`; their full instructions load only when used.

- `artifact-maintenance` and `calculation-mapping` may be selected automatically when their descriptions match the task.
- `delivery-commit` is manual-only; invoke it explicitly with `@delivery-commit`.
- Skills complement, rather than replace, the task-based spec loading above.

## History Policy

History is evidence, not default context. Search it by entity ID or rule key; do not load it wholesale. Archived material under `.opencode/history/` preserves prior detail and never overrides active specs.

## Spec Authoring Policy

Keep active specs normative and concise: current behavior, active rules, constraints, and links. Record completed entity narratives, source transcripts, retrospective notes, and superseded decisions in history. Define a rule once and link to it rather than duplicating it across specs.
