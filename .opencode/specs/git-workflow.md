# Git Workflow

## AI Artifact Branch

- The `agent` branch is the exclusive branch for AI artifacts: `AGENTS.md`, `opencode.json`, and every file under `.opencode/`, including specs, skills, agents, and commands.
- Commit and push AI artifacts only to `origin/agent`.
- Do not transfer AI artifacts to application delivery branches.

## Application Delivery

- Entity maintenance uses coordinated per-entity commits. First, commit the complete entity work on `agent`: its served JSON changes plus directly affected AI specs/history. Then mirror only the served entity JSON changes to the named application-delivery branch, as one commit for that same entity. Each branch therefore has exactly one commit per entity; the `agent` commit is the complete research record and the delivery commit is publishable data only. Use the same exact local ID in both English commit messages. Do not cherry-pick the complete `agent` commit into a delivery branch, because it contains AI artifacts.
- The current weapon delivery branch is `weapons-specifications`, created from `main`. Its tracked changes and entity commits may contain only the selected entity's publishable JSON files; do not add `AGENTS.md`, `opencode.json`, `.opencode/`, or unrelated files. Keep the primary worktree on `agent` so it retains the full entity JSON and governance record.
- On 2026-10-14 the owner authorized source-validated alphabetical artifact rounds of at most five entities, removed separate approval for each commit/advance, and explicitly restored commit-before-advance. Research/map/apply/validate/record/commit one entity before selecting the next. Use `<id> - <what was done>` (exact local ID, English description), preserving per-entity scope/directly affected specs/history and approved governance changes. Stop earlier for source conflicts or unapproved schema/semantic decisions; standing commit authorization does not approve an invented schema or unresolved mapping. Report round outcomes/known check failures; no push, bulk migration, automatic additional round or edits beyond scope are authorized. A commit does not certify unresolved coverage.

- Use `/push <target-branch>` to perform an application delivery push. The command applies the checks and recovery steps below.
- Start each application delivery branch from an updated `origin/main`.
- If the requested branch exists locally, fast-forward it from the updated `main`; otherwise, create it from `main`. Stop if it exists on `origin` or either branch cannot fast-forward, then transfer only non-AI changes.
- Create small, coherent commits. A single-file change may have its own commit; files that implement one change may share a commit.
- For `/push`, run focused validation appropriate to the changed files; do not run `pnpm run lint` or `pnpm run build`.
- Create a temporary backup before moving changes. Delete it only after a successful push.
- On success, return to `agent`. On conflicts, validation failures, ambiguous files, or push failures, stop and report the state without deleting the backup.
