# Application State

## Purpose

This repository is a static-data API for Genshin Impact entities and images.

## Runtime

- `src/index.ts` starts Koa on `PORT` (default `5000`); `SENTRY_DSN` enables optional Sentry.
- `src/routes/index.ts` maps catch-all static-data and image routes.
- `src/modules/filesystem.ts` reads/caches static files, adds response `id`, and converts images with Sharp.
- The service exposes static records only; it does not calculate progression, stats, damage, reactions, timers, or consumer state.
- Raw source contributions remain separate; API consumers combine applicable bonuses under `consumer-composed-source-contributions` in the calculation workflow, without automatic bonus inheritance or precomputed post-bonus damage.

## Data and Assets

- Entity data: `assets/data/<type>/<lowercase-hyphenated-id>/<language>.json`.
- A requested locale is a complete response: the API does not merge it with English or provide English fallback.
- Images: `assets/images/<type>/<id>/<image-name>` without filename extensions.
- Character `icon` files are generated from `icon-big` and ignored; run `pnpm run gen` after changing `icon-big`.
- Committed character/weapon `calculation` data and `assets/data/calculation/curves/en.json` remain available, but do not certify complete coverage.

## Current Maintenance State

- Entity calculation work is individual and source-validated; never run a bulk migration or restore archived bulk results.
- Normal maintenance changes only EN/PT and preserves other locales. Entity-specific deletions require explicit owner authorization.
- For an authorized bounded artifact round (up to five entities) or weapon round (up to 20 entities), work sequentially by local ID, fully complete and commit one entity before advancing, stop at the requested limit, an unresolved source conflict under `entity-research.md`, or a schema conflict, and do not push unless explicitly requested. Mirror each completed weapon's publishable JSON in its separate `weapons-specifications` delivery-branch commit before selecting the next weapon.
- EN/PT display text may be aligned with validated sources; new mechanical schemas still require a deliberate decision.
- Character maintenance continues alphabetically after the completed `albedo` pilot in owner-authorized bounded rounds under `git-workflow.md`, using PT level-indexed talent tables as the structural reference and source-local modifiers/independent skills in `skillTalents`, `passiveTalents`, and `constellations`. Reviewed talent damage coefficients uniformly use `power[].components`, including single-attribute rows and the owner-authorized Albedo alignment; see `character-talent-attribute-components` and related character rules in the calculation workflow. Complete each character's coordinated `agent` and JSON-only `characters-specifications` commits before advancing; unresolved source/schema decisions stop the round, and no push is authorized by round approval.
- A new owner-authorized ten-character round is active after Collei, with exact selection/progress recorded in character history. Round 3 (`cyno` through `faruzan`, ten alphabetical IDs) is complete at 10/10 with `dori` as the final item. Round 4 (`fischl` through `jean`, ten alphabetical IDs) is active; `fischl` complete (1/10), `freminet` complete (2/10), `furina` complete (3/10), `gaming` complete (4/10), `ganyu` complete (5/10), `gorou` complete (6/10), `hu-tao` selected as item 7/10. Stop at the authorized limit or an unresolved source/schema gate. No push or automatic additional round is authorized.
- Reviewed references may use a single object or an explicit multiplicative array; grouped talent healing uses `type: healing` with level-local components under the calculation workflow. The sole new current-Bond reference is normalized `character.bond_of_life`; existing HP-unit `cleared_bond_of_life` and all prior single-reference data remain unchanged. Arlecchino is the first individually reviewed application; no runtime calculation or bulk migration is added.
- Individually reviewed constellation `changes` explicitly reference a localized talent name and select talent `level` or a labeled modifier's `accumulations` increment under `character-constellation-changes`. Base data remains unchanged; consumers apply unlocked changes. The required retrospective is complete and recorded in character history; preserve it without automatically repeating it. No unreviewed-character migration or runtime evaluator is added.
- Individually reviewed shield talent rows support `shield_absorption` with separate attribute-referenced and direct fixed components; reviewed healing modifiers can reference a uniquely resolving localized talent's raw healing at its effective level. Baizhu is the first selected application, with `SPREAD` identifying Propagação separately from Aggravate/Intensificação. Authoritative contracts and compatibility limits are in the calculation workflow; no runtime evaluator or unrelated migration is added.
- Individually reviewed incoming damage reduction uses source-local `dmg_reduction`; single-attribute constellation shield capacity can use a source-local `shield_absorption` modifier. Beidou is the first selected application under the calculation workflow's incoming-reduction and source-local-shield contracts, without changing previous text-only exceptions or adding a runtime evaluator.
- Reviewed source Base ATK references use `character.base_atk` (innate character plus weapon Base ATK, excluding artifacts/buffs), distinct from total `character.atk`. Bennett is the first selected application for flat ATK grants under `character-base-atk-reference`; no unrelated references or runtime calculations are changed.
- Individually reviewed independent EM-based damage follows `independent-em-referenced-damage`: native skill scaling `dmg_percent` explicitly references source `character.elemental_mastery`. Citlali C4 is the first approved application; this does not grant EM or change prior independent scaling types.
- Individually reviewed generic ATK SPD follows `generic-attack-speed-bonus`: `atk_speed_percent` without `target`, positive `values`, `bonus: true`; Dahlia A4 references Dahlia MaxHP with result cap, C6 adds a separate current-recipient increment; no resurrection result or healing engine.
- Individually reviewed direct reaction procs can carry `IndependentSkill.reaction` under `direct-reaction-independent-damage`. Cyno's enhanced Starsame identifies Stellar-Conduct damage, with its raw ATK proc, A4 attached EM contribution and C2 dimensionless reaction bonus kept separate; ordinary proc classifications remain unchanged.
- Every existing independent skill now requires a localized nonempty `label`; approved alternative proc totals remain labeled, mutually exclusive native skills. The owner-authorized `dragonspine-spear` adjustment includes label-only backfill across existing skill records/locales, without changing their mechanics or importing branch-specific calculations.
- The prior artifact review has no active follow-up. Dismissed topics (full damage engines, artifact main/substat progression, consumer state/timers, and unrelated baseline check repairs) must not be reopened automatically.

## Where to Find Detail

- Active data formats and compatibility: `data-conventions.md`.
- Active calculation rules and workflow: `entity-calculation-workflow.md`.
- External research requirements: `entity-research.md`.
- Delivery/branch requirements: `git-workflow.md`.
- Completed entity summaries, rule decisions, and closed decisions: `.opencode/history/entity-work-history.md` (the compact history index and links).

## Build and Delivery

- `pnpm run build` compiles `src/` to `dist/`; `pnpm run lint` lints and checks formatting.
- Docker compiles source, copies `assets`, runs icon generation, and starts `dist`.
- GitHub Actions publishes the Docker image for pushes to `mistress`.

## Maintenance Contract

Update this file when runtime behavior, routes, data layout, generation, environment variables, or delivery behavior changes. Put completed entity narratives and research evidence in history, not here.
