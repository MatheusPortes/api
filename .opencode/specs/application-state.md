# Application State

## Purpose

This repository is a static-data API for Genshin Impact entities, including characters, weapons, artifacts, materials, enemies, and their images.

## Runtime

- `src/index.ts` starts a Koa HTTP service on `PORT`, defaulting to `5000`.
- `SENTRY_DSN` enables optional Sentry error reporting.
- `src/routes/index.ts` serves type lists, entity lists and records, and entity images through catch-all routes.
- `src/modules/filesystem.ts` reads and caches static files. It adds an `id` field to every entity response and converts served images with Sharp.

## Data Model

- Entity data is stored at `assets/data/<type>/<lowercase-hyphenated-id>/en.json`.
- Translation files use language-code names such as `pt.json`. The API serves the requested file as a complete record; it does not merge it with `en.json` or fall back to English.
- Images are stored at `assets/images/<type>/<id>/<image-name>` without filename extensions.
- Character `icon` files are generated from `icon-big` by `pnpm run gen` and are ignored by Git.
- Read `data-conventions.md` before changing entity data or images, and `entity-research.md` before using external factual information.

## Build and Delivery

- `pnpm run build` compiles TypeScript from `src/` to `dist/`.
- The Docker build compiles source, copies `assets/`, and runs icon generation before starting `dist`.
- GitHub Actions publishes the Docker image for pushes to `mistress`.

## Maintenance Contract

Read this spec before planning, editing, or validating repository work. Update it in the same change whenever runtime behavior, routes, data layout, asset conventions, generation, environment variables, or delivery behavior changes.
