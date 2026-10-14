# Git Workflow

## AI Artifact Branch

- The `agent` branch is the exclusive branch for AI artifacts: `AGENTS.md`, `opencode.json`, and every file under `.opencode/`, including specs, skills, agents, and commands.
- Commit and push AI artifacts only to `origin/agent`.
- Do not transfer AI artifacts to application delivery branches.

## Application Delivery

- Before advancing to another entity, present the current entity's changes, validation results and remaining blockers, then wait for the owner's explicit approval to commit. Only after that approval, commit with the message format `<id> - <what was done>`. Use the exact local entity ID and an English description. A request to work on the next entity, approval of a schema/mechanic or the standing requirement to commit each entity is not itself commit approval. Keep the commit scoped to that entity and its directly affected specs/history; AI artifacts may be included only on `agent`. A commit is not a completion certificate and does not authorize a push or automatic selection of the next entity.

- Use `/push <target-branch>` to perform an application delivery push. The command applies the checks and recovery steps below.
- Start each application delivery branch from an updated `origin/main`.
- If the requested branch exists locally, fast-forward it from the updated `main`; otherwise, create it from `main`. Stop if it exists on `origin` or either branch cannot fast-forward, then transfer only non-AI changes.
- Create small, coherent commits. A single-file change may have its own commit; files that implement one change may share a commit.
- For `/push`, run focused validation appropriate to the changed files; do not run `pnpm run lint` or `pnpm run build`.
- Create a temporary backup before moving changes. Delete it only after a successful push.
- On success, return to `agent`. On conflicts, validation failures, ambiguous files, or push failures, stop and report the state without deleting the backup.
