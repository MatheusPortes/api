# Application State

## Purpose

This repository is a static-data API for Genshin Impact entities, including characters, weapons, artifacts, materials, enemies, and their images.

## Runtime

- `src/index.ts` starts a Koa HTTP service on `PORT`, defaulting to `5000`.
- `SENTRY_DSN` enables optional Sentry error reporting.
- `src/routes/index.ts` serves type lists, entity lists and records, and entity images through catch-all routes.
- `src/modules/filesystem.ts` reads and caches static files. It adds an `id` field to every entity response and converts served images with Sharp.

## Data Model

- The owner retired the uncommitted bulk calculation migration and requested an individual-entity restart. On 2026-10-14 all pending entity changes, including the prior removal of committed calculation fields, were restored to Git HEAD `0b2f887d6d2d2953d9a6896002c06044454434ff`. `assets/data` had no tracked changes immediately after that reset; subsequent individual changes are recorded below and in history. Future maintenance is English/Portuguese only.
- Previously committed character/weapon `calculation` data, shared `assets/data/calculation/curves/en.json` (served at `/calculation/curves`), and earlier Aquila Favonia effect/proc examples remain present. The uncommitted bulk talent/effect extensions were not restored. Existing fields do not certify complete coverage under the new workflow; inspect the selected record before reusing any historical example.
- `.opencode/specs/entity-calculation-workflow.md` is the single active calculation plan/rulebook, including general rules, examples, deferred schemas and the staged per-entity workflow. `.opencode/history/entity-work-history.md` retains reusable examples/references and future individual work entries; the discarded migration's inventory/counts are no longer retained there. The preserved backup/frozen report is optional historical evidence, not a current completion list.
- The three experimental Python bulk migration/audit scripts were retired. Do not invoke archived bulk commands or regenerate the old blocker report. No next entity is selected automatically.
- Individual artifact work started with `artifacts/adventurer` on 2026-10-14, ordered alphabetically by local directory ID. Its English/Portuguese set-bonus coefficients are represented in top-level `effects`: 2-piece flat HP and 4-piece `healing` with `conditional` referencing character HP. Completion is limited to these two set bonuses; main/substat progression is outside scope.
- `artifacts/archaic-petra` has English/Portuguese `effects` for the 2-piece Geo bonus and four mutually exclusive ordinary-Crystallize elemental alternatives for the 4-piece bonus. Coverage is partial: newer KQM/Yatta text includes Lunar-Crystallize, unlike the local/Game8 set descriptions, and that branch is not yet mapped. Other entities/locales remain at baseline unless recorded above or in history.
- `artifacts/berserker` has reviewed English/Portuguese `effects` for both set bonuses: separate `crit_rate` coefficients `0.12` and additional `0.24`, both `bonus: true`. The below-70%-HP activation remains consumer usage, not an attribute-scaling `conditional`. Selected set-bonus coverage is complete; commit requires explicit owner approval.
- `artifacts/blizzard-strayer` has English/Portuguese `effects` for the 2-piece Cryo damage bonus (`0.15`) and two separate 4-piece CRIT Rate bonuses (`0.2` each). Labels preserve the Cryo-affected/Frozen enemy conditions; the consumer evaluates eligibility per target/hit under rule `target-state-crit-rate`, not as unconditional global CRIT. Selected coefficient coverage is complete; the owner approved its commit on 2026-10-14.
- `artifacts/bloodstained-chivalry` has reviewed English/Portuguese `effects`: 2-piece `physical_dmg_percent` (`0.25`) and 4-piece `dmg_percent` (`0.5`) with `target: "charged_attack"`. The 4-piece label names the defeat trigger; activation, 10s duration and zero Stamina cost remain consumer usage in the preserved description. Selected damage-coefficient coverage is complete; commit requires owner approval.
- `artifacts/brave-heart` has reviewed English/Portuguese `effects`: 2-piece `atk_percent` (`0.18`) and 4-piece generic `dmg_percent` (`0.3`), both `bonus: true`. The 4-piece label specifies enemy HP above 50%; the consumer evaluates this strict target-state condition, not an attribute-scaling `conditional` or unconditional damage grant. Selected coefficient coverage is complete; commit requires owner approval.
- `artifacts/crimson-witch-of-flames` has English/Portuguese `effects` with base Pyro bonus `0.15`, two reaction-scoped `dmg_percent` modifiers (`0.4` and `0.15`, each with a standardized `reaction` array), and separate additional Pyro bonus `0.075` per stack with `accumulations: 3` as its maximum stack count. Active stacks remain consumer state. The owner authorized correcting the English description to include Burgeon and the starting-value reference; Portuguese and other locales are preserved. Selected coefficient coverage is complete; commit requires owner approval.
- `artifacts/deepwood-memories` has English/Portuguese `effects` for 2-piece Dendro damage (`elemental_dmg_percent`, `0.15`, `bonus: true`) and 4-piece enemy Dendro resistance reduction (`elemental_res_reduction`, `0.3`, no `bonus`). Both specify `element: "DENDRO"`; the enemy-debuff label names Skill/Burst-hit activation. Existing descriptions/locales are preserved and activation/timing remain consumer usage. Selected coefficient coverage is complete; commit requires owner approval.

- Entity data is stored at `assets/data/<type>/<lowercase-hyphenated-id>/en.json`.
- Translation files use language-code names such as `pt.json`. The API serves the requested file as a complete record; it does not merge it with `en.json` or fall back to English.
- This service exposes static records; it does not resolve progression, compose stats or calculate damage. Future calculation inputs must be added and validated one selected entity at a time under the consolidated workflow.
- Images are stored at `assets/images/<type>/<id>/<image-name>` without filename extensions.
- Character `icon` files are generated from `icon-big` by `pnpm run gen` and are ignored by Git.
- Read `data-conventions.md` before changing entity data or images, and `entity-research.md` before using external factual information.

## Build and Delivery

- `pnpm run build` compiles TypeScript from `src/` to `dist/`.
- The Docker build compiles source, copies `assets/`, and runs icon generation before starting `dist`.
- GitHub Actions publishes the Docker image for pushes to `mistress`.

## Maintenance Contract

Read this spec before planning, editing, or validating repository work. Update it in the same change whenever runtime behavior, routes, data layout, calculation-data conventions, asset conventions, generation, environment variables, or delivery behavior changes.
