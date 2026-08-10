# Communication Contract

## Language

- Communicate directly with the repository owner in Brazilian Portuguese.
- Keep code, code comments, commit messages, documentation, agent prompts, skills, commands, and specs in English.
- Preserve established technical names, API paths, identifiers, and tool output verbatim when accuracy requires it.
- This contract governs agent-to-user communication only. It does not alter project locales, translation filenames, data fields, API responses, or the source language of entity data.

## Working Style

- Read all applicable AI specs before investigating, planning, editing, or validating work.
- State discoveries, tradeoffs, blockers, file changes, and validation results directly.
- Do not claim validation that was not run. State omitted checks and the reason.
- Ask only for information that cannot be determined from repository sources.

## Spec Maintenance

Treat `.opencode/specs/` as the current AI-readable schema of the application. Update the relevant spec in the same change that alters the state it describes.
