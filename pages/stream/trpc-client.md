# Stream › Trpc client

owner: bohdantanasov (most confirmed records) · records: 1 (1 confirmed) · rules guarded by a test: 0 of 1 · last change: 2026-10-08

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- tRPC 11.18 httpBatchLink's dataLoader never drops an op that React Query cancelled while it waits for the batch tick (item.aborted is never set), so each cancelQueries+invalidateQueries in the same tick adds another identical query to one GET URL. Do not rely on cancellation to dedupe; keep invalidations to one per burst and keep maxURLLength set. [mem_6d6dc6ab7aa6]
  why: tRPC 11.18 httpBatchLink dataLoader never drops an op React Query cancelled while it waits for the batch tick (groupItems checks item.aborted, nothing sets it; the observable teardown is a no-op), so each cancelQueries+invalidateQueries in one tick adds an identical query to one GET URL.
  do: Do not rely on cancellation to dedupe; one invalidation per burst; keep maxURLLength set in client.tsx.
  guarded: not yet
  where: monorepo-frontend:apps/stream-app/src/infra/trpc/client.tsx:71
  PRO-9993
