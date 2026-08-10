# Repository Guide

## AI specs

- Read all applicable `.opencode/specs/` files before investigating, planning, editing, or validating work.
- Treat these files as the AI-readable application schema. Update the affected spec in the same change whenever application state, behavior, architecture, data layout, generation, or delivery changes.
- Version AI artifacts only on the `agents` branch. Do not transfer `AGENTS.md`, `opencode.json`, or `.opencode/` files to application delivery branches.

## Tooling and checks

- Use Node.js `^20` and pnpm; `pnpm-lock.yaml` is the tracked lockfile.
- Build/type-check with `pnpm run build`; lint and check formatting with `pnpm run lint`; format with `pnpm run format`.
- There is no test script or test configuration. Run the focused checks above after changes.
- For local development, run `npm run watch` and `npm run dev` in separate terminals.

## Service and assets

- `src/index.ts` starts the Koa service and `src/routes/index.ts` maps the catch-all static-data and image routes. The compiled service reads root `assets/` through `src/config.ts`.
- Add data as `assets/data/<type>/<lowercase-hyphenated-id>/en.json`; use language-code JSON files for translations.
- Store entity images in `assets/images/<type>/<id>/` without filename extensions; the API converts them with Sharp when serving.
- `pnpm run gen` generates 128px character `icon` files from `assets/images/characters/*/icon-big`. Generated `icon` files are ignored; run it after changing character `icon-big` assets.

## Runtime and delivery

- The service listens on `PORT` (default `5000`); `SENTRY_DSN` optionally enables Sentry. The README's `API_PORT` and `.env.example` setup instructions do not match the implementation.
- The Docker image builds `src`, copies `assets`, and runs generation. GitHub Actions publishes the image only for pushes to `mistress`.
