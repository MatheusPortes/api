---
description: Safely publish non-AI application changes to a new branch based on origin/main.
agent: build
---

Read `AGENTS.md` and all applicable `.opencode/specs/` files first. Target branch: $1

If `$1` is empty, ask the user for the target branch and stop until it is provided. Use only the `origin` remote.

1. Inspect `git status`, `git diff`, recent commits, branches, and remotes. Identify AI artifacts as `AGENTS.md`, `opencode.json`, and `.opencode/**`.
2. Do not use `agent` as an application delivery target. First commit and push AI artifact changes to `origin/agent`; AI artifacts must never be copied to the requested branch.
3. Fetch `origin`, update local `main` from `origin/main` with fast-forward only, then create the requested new branch from the updated main branch. Stop if the target branch already exists or main cannot fast-forward.
4. Before moving changes, create a recoverable temporary backup of the current worktree. Preserve the backup until the push succeeds.
5. Transfer only non-AI changes to the requested branch. Keep AI artifacts in `agent`.
6. Inspect the target diff. Create small, coherent commits: commit a single file alone when independent, and group files only when they implement the same change.
7. Run focused validation appropriate to the changed files. Do not run `pnpm run lint` or `pnpm run build` as part of this command. Push with `git push -u origin <target-branch>` only after the focused validation succeeds.
8. After a successful push, delete the temporary backup and return to `agent`. If any command fails, do not push, do not delete the backup, and report the exact blocker and repository state.
