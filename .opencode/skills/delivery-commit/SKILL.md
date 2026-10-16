---
name: Delivery Commit
description: Prepare the authorized per-entity delivery commit or push while preserving the repository's branch and AI-artifact boundaries.
metadata:
  opencode/autoinvoke: false
---

Use this skill only when the owner explicitly requests an entity commit, delivery-branch mirror, or push.

1. Read `../../specs/git-workflow.md` and inspect Git status before changing branches or creating commits.
2. Confirm the selected entity is complete, validated, and recorded on `agent` before mirroring served JSON to a delivery branch.
3. Keep AI artifacts (`AGENTS.md`, `opencode.json`, and `.opencode/`) on `agent`; never transfer them to a delivery branch.
4. Make one coherent commit per entity using the exact local ID in an English message. Validate the delivery worktree independently.
5. Push only when explicitly authorized. Report branch, commits, validations, and any unresolved state.

Stop on conflicts, ambiguous files, non-fast-forward branches, or validation failures. Do not discard user changes.
