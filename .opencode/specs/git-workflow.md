# Git Workflow

## AI Artifact Branch

- The `agents` branch is the exclusive branch for AI artifacts: `AGENTS.md`, `opencode.json`, and every file under `.opencode/`, including specs, skills, agents, and commands.
- Commit and push AI artifacts only to `origin/agents`.
- Do not transfer AI artifacts to application delivery branches.

## Application Delivery

- Start each application delivery branch from an updated `origin/main`.
- Create the requested branch from `main`, then transfer only non-AI changes.
- Create small, coherent commits. A single-file change may have its own commit; files that implement one change may share a commit.
- Create a temporary backup before moving changes. Delete it only after a successful push.
- On success, return to `agents`. On conflicts, validation failures, ambiguous files, or push failures, stop and report the state without deleting the backup.
