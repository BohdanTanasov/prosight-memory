# Devdeck › Prs

owner: bohdantanasov (most confirmed records) · records: 4 (4 confirmed) · rules guarded by a test: 0 of 3 · last change: 2026-10-02

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- The background review's queue step and its start step must use the same arithmetic: spent + what queued runs hold + the next run's OWN tier cap <= the day's cap (mayStart with tierCapUsd). Checking only spent + reserved >= cap before queueing let a head be queued every poll and refused at every start, writing a failed review row each time and a red 'Failed' on the board (#5499 on 2026-10-02, three refusals in one morning). [mem_5d1a4cae21dd]
  why: Checking only spent + reserved >= cap before queueing let a head be queued every poll and refused at every start, writing a failed review row each time and a red "Failed" on the board (#5499 on 2026-10-02, three refusals in one morning).
  do: Queue step and start step use the same arithmetic: spent + what queued runs hold + the next run own tier cap <= the day cap (mayStart with tierCapUsd).
  guarded: not yet
  where: prosight-devdeck:src/features/prs/daemon/auto-queue.ts:56
- A person's PR work is what they authored or reviewed, never PRs they are only asked on: default reviewers sit on about half of all PRs. [mem_0bb72654f32c]
  why: Default reviewers sit on about half of all PRs, so counting PRs a person is only asked on would make every person list meaningless.
  do: who.ts counts authored or reviewed PRs only; never add requested-reviewer PRs to a person work.
  guarded: not yet
  where: prosight-devdeck:web/src/features/prs/containers/who.ts:1
- Per-thread fixes (POST …/threads/:tid/fix) each start a worktree at the PR head, so several of them cannot all be pushed: after the first push-fix lands, the others go stale with head_moved. Fixing several remarks of one PR must stack the fixes (thread-fix `base` = the previous fix's sha), as the sweep does; one push of the last commit then sends the whole chain. [mem_8780f8003d91]
  why: Each per-thread fix (POST …/threads/:tid/fix) starts a worktree at the PR head, so after the first push-fix lands the others go stale with head_moved.
  do: Fixing several remarks of one PR stacks the fixes (thread-fix base = the previous fix sha), as the sweep does; one push of the last commit sends the chain.
  guarded: not yet
  where: prosight-devdeck:src/features/prs/daemon/thread-fix.ts:450

## Decisions and why
- BoardPr.review speaks about THIS head only: 'reviewed' means a done pass of the current commit; a done review of an older commit stays in `run` with pushedSince and the web says it as 'older commit: …' in grey, never as the current verdict, and never grounds a Publish. The old DevDeck had this as the 'stale' state (devdeck/lib/pr-board.ts stateOf) and the port had lost it, which is why four live rows showed a replaced commit's verdict as current. [mem_c920357282bf]
  why: The old DevDeck had the "stale" state (devdeck/lib/pr-board.ts stateOf) and the port lost it: four live rows (#5498, #4510, #4532, #5499) showed a replaced commit verdict as current.
  do: BoardPr.review speaks about this head only; a done review of an older commit stays in `run` with pushedSince, the web shows it grey as "older commit: …", never as the current verdict and never as grounds for a Publish.
  where: prosight-devdeck:src/features/prs/contract.ts:346
