# Devdeck › Tests

owner: none recorded · records: 1 (0 confirmed · 1 proposed) · rules guarded by a test: 0 of 0 · last change: 2026-10-22

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Testing notes (what a spec must know)
- The custom pipeline `devdeck-coverage` exists only on branches whose bitbucket-pipelines.yml has it (feature/devdeck until it merges to develop). Starting it on a branch without it fails, so tests/run defaults to the newest run's branch, not the mainline. [mem_ff3a99f20004] (proposed)
  why: The custom pipeline devdeck-coverage exists only on branches whose bitbucket-pipelines.yml has it (feature/devdeck until it merges to develop); starting it on another branch fails.
  do: tests/run defaults to the newest run branch, not the mainline; revisit the default after the merge to develop.
  where: monorepo-frontend:tools/devdeck/src/features/tests/daemon/read.ts:54

## Disputed / open
- The custom pipeline `devdeck-coverage` exists only on branches whose bitbucket-pipelines.yml has it (feature/devdeck until it merges to develop). Starting it on a branch without it fails, so tests/run defaults to the newest run's branch, not the mainline. [mem_ff3a99f20004] (proposed) — nobody confirmed it yet
