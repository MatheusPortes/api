# Application State

## Purpose

This repository is a static-data API for Genshin Impact entities and images.

## Runtime

- `src/index.ts` starts Koa on `PORT` (default `5000`); `SENTRY_DSN` enables optional Sentry.
- `src/routes/index.ts` maps catch-all static-data and image routes.
- `src/modules/filesystem.ts` reads/caches static files, adds response `id`, and converts images with Sharp.
- The service exposes static records only; it does not calculate progression, stats, damage, reactions, timers, or consumer state.

## Data and Assets

- Entity data: `assets/data/<type>/<lowercase-hyphenated-id>/<language>.json`.
- A requested locale is a complete response: the API does not merge it with English or provide English fallback.
- Images: `assets/images/<type>/<id>/<image-name>` without filename extensions.
- Character `icon` files are generated from `icon-big` and ignored; run `pnpm run gen` after changing `icon-big`.
- Committed character/weapon `calculation` data and `assets/data/calculation/curves/en.json` remain available, but do not certify complete coverage.

## Current Maintenance State

- Entity calculation work is individual and source-validated; never run a bulk migration or restore archived bulk results.
- Normal maintenance changes only EN/PT and preserves other locales. Entity-specific deletions require explicit owner authorization.
- For an authorized bounded artifact round, work sequentially by local ID, fully complete and commit one entity before advancing, stop at the requested limit or on a source/schema conflict, and do not push unless explicitly requested.
- EN/PT display text may be aligned with validated sources; new mechanical schemas still require a deliberate decision.
- The prior artifact review has no active follow-up. Dismissed topics (full damage engines, artifact main/substat progression, consumer state/timers, and unrelated baseline check repairs) must not be reopened automatically.

## Where to Find Detail

- Active data formats and compatibility: `data-conventions.md`.
- Active calculation rules and workflow: `entity-calculation-workflow.md`.
- External research requirements: `entity-research.md`.
- Delivery/branch requirements: `git-workflow.md`.
- Completed entity summaries and closed decisions: `.opencode/history/entity-work-history.md`.
- Former verbose application-state summaries: `.opencode/history/application-state-archive.md`.

## Build and Delivery

- `pnpm run build` compiles `src/` to `dist/`; `pnpm run lint` lints and checks formatting.
- Docker compiles source, copies `assets`, runs icon generation, and starts `dist`.
- GitHub Actions publishes the Docker image for pushes to `mistress`.

## Maintenance Contract

Update this file when runtime behavior, routes, data layout, generation, environment variables, or delivery behavior changes. Put completed entity narratives and research evidence in history, not here.
