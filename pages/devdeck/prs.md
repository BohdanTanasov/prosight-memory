# DevDeck › PRs

owner: bohdantanasov (most confirmed records) · records: 4 (4 confirmed) · rules guarded by a test: 0 of 3 · last change: 2026-10-02

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- Queue step and start step use the same arithmetic: spent + what queued runs hold + the next run own tier cap <= the day cap (mayStart with tierCapUsd). [mem_5d1a4cae21dd]
  why: Checking only spent + reserved >= cap before queueing let a head be queued every poll and refused at every start, writing a failed review row each time and a red "Failed" on the board (#5499 on 2026-10-02, three refusals in one morning).
  guarded: no test yet
  where: monorepo-frontend:tools/devdeck/src/features/prs/daemon/auto-queue.ts:56
- who.ts counts authored or reviewed PRs only; never add requested-reviewer PRs to a person work. [mem_0bb72654f32c]
  why: Default reviewers sit on about half of all PRs, so counting PRs a person is only asked on would make every person list meaningless.
  guarded: no test yet
  where: monorepo-frontend:tools/devdeck/web/src/features/prs/containers/who.ts:1
- Fixing several remarks of one PR stacks the fixes (thread-fix base = the previous fix sha), as the sweep does; one push of the last commit sends the chain. [mem_8780f8003d91]
  why: Each per-thread fix (POST …/threads/:tid/fix) starts a worktree at the PR head, so after the first push-fix lands the others go stale with head_moved.
  guarded: no test yet
  where: monorepo-frontend:tools/devdeck/src/features/prs/daemon/thread-fix.ts:450

## Decisions and why
- BoardPr.review speaks about this head only; a done review of an older commit stays in `run` with pushedSince, the web shows it grey as "older commit: …", never as the current verdict and never as grounds for a Publish. [mem_c920357282bf]
  why: The old DevDeck had the "stale" state (devdeck/lib/pr-board.ts stateOf) and the port lost it: four live rows (#5498, #4510, #4532, #5499) showed a replaced commit verdict as current.
  where: monorepo-frontend:tools/devdeck/src/features/prs/contract.ts:346
