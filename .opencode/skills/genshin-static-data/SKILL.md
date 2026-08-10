---
name: genshin-static-data
description: Use when adding or changing Genshin Impact entities, translations, JSON records, images, character icons, weapons, artifacts, materials, enemies, or asset folders.
---

# Genshin Static Data

1. Read `.opencode/specs/application-state.md`, `data-conventions.md`, and `entity-research.md`, then inspect a comparable existing entity before editing.
2. Research every external fact in the three required community sources. Compare entity-specific pages, resolve disagreements, and prepare explicit references before editing.
3. Use lowercase hyphenated IDs and the layout required by the data area; not every type uses `<type>/<id>/en.json`.
4. Add translations using the exact local language filename convention. They are complete served records, not overlays, so preserve the intended response shape.
5. Store images without filename extensions at the matching image path.
6. When changing a character `icon-big`, run `pnpm run gen`. Do not add the generated `icon` file because it is ignored.
7. Do not complete the task if required sources are unavailable or factual disagreements remain unresolved. Report the blocker and list all references.
8. Run the relevant checks and update the application-state spec if the data model, asset convention, or generation workflow changes.
