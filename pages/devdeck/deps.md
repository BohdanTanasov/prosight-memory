# Devdeck › Deps

owner: none recorded · records: 1 (0 confirmed · 1 proposed) · rules guarded by a test: 0 of 0 · last change: 2026-10-22

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Decisions and why
- The deps conflict count (the card title, the 'What to do' step) is conflictCount(): rows deduped per package+range, with a family's peers merged (react+react-dom). It is not links.conflicts.length, so it reads 11 where the daemon lists 12. [mem_cd373b523f4a] (proposed)
  why: Rows are deduped per package+range and a family peers merged (react+react-dom), so the count reads 11 where the daemon lists 12 conflicts.
  do: Use conflictCount() for the card title and the "What to do" step, never links.conflicts.length.
  where: monorepo-frontend:tools/devdeck/web/src/features/deps/ui/links.ts:1

## Disputed / open
- The deps conflict count (the card title, the 'What to do' step) is conflictCount(): rows deduped per package+range, with a family's peers merged (react+react-dom). It is not links.conflicts.length, so it reads 11 where the daemon lists 12. [mem_cd373b523f4a] (proposed) — nobody confirmed it yet
