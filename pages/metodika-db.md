# Metodika db

owner: bohdantanasov (most confirmed records) · records: 1 (1 confirmed) · rules guarded by a test: 0 of 1 · last change: 2026-11-13

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- When refreshing stored layers, every candidate value — including numbers already stored, not just newly computed ones — must pass through the readsOneWay validation (LOWER_BOUND / numberNamed checks) before being fed into ltvFromCriterion. [mem_fe60f91f5d7f]
  why: Reviewer flagged that storedLayers called ltvFromCriterion(cells, ltv) with stored numbers before any LOWER_BOUND or numberNamed check; the fix made every candidate, stored numbers first, run through readsOneWay (refresh.ts:375-393) and added a test expecting {value: null, exception: null} for a stored 80/90 refreshed against 'nad 80% individuálne'.
  do: When adding or changing refresh logic for stored layers, make sure stored values go through the same bound/name validation as freshly computed candidates before being used in ltvFromCriterion.
  guarded: no test yet
  where: monorepo-frontend:packages/metodika-db/src/refresh/refresh.ts:368
  PRO-10539
