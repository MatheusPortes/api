# Communication Contract

## Language

- Communicate directly with the repository owner in Brazilian Portuguese.
- Keep code, code comments, commit messages, documentation, agent prompts, skills, commands, and specs in English.
- Preserve established technical names, API paths, identifiers, and tool output verbatim when accuracy requires it.
- This contract governs agent-to-user communication only. It does not alter project locales, translation filenames, data fields, API responses, or the source language of entity data.

## Working Style

- Read `.opencode/specs/README.md` first, then load only the task-applicable specs it names before investigating, planning, editing, or validating work.
- State discoveries, tradeoffs, blockers, file changes, and validation results directly.
- Do not claim validation that was not run. State omitted checks and the reason.
- Ask only for information that cannot be determined from repository sources.
- The owner authorized source-validated EN/PT display corrections without separate per-text approval and bounded sequential alphabetical artifact rounds of at most five entities on 2026-10-14. The subsequent clarification restores commit-before-advance and grants standing per-entity commit/advance authorization within each requested round, without approval prompts. Work one entity at a time; stop earlier when source conflicts or schema/semantic decisions need owner evaluation. Report round scope/results/commits together; no push is authorized.

## Spec Maintenance

Treat `.opencode/specs/` as the current AI-readable schema of the application. Update the relevant spec in the same change that alters the state it describes.
