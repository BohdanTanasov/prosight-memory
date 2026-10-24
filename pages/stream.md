# Stream

owner: bohdantanasov (most confirmed records) · records: 2 (2 confirmed) · rules guarded by a test: 1 of 2 · last change: 2026-10-08

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- The BO edit form omits s3_key when no new file is picked; updateAudioRecording keeps omitted = keep the stored file, null = clear, string = replace. [mem_26a3274e9b1a]
  why: Sending s3_key: null wiped the files of production recordings 203-207 (PRO-10104).
  guarded: test:monorepo-frontend:packages/stream-api/src/services/stream-write.service.test.ts:247
  where: monorepo-frontend:packages/stream-api/src/services/stream-write.service.ts:241
  PRO-10104
- Do not rely on cancellation to dedupe; one invalidation per burst; keep maxURLLength set in client.tsx. [mem_6d6dc6ab7aa6]
  why: tRPC 11.18 httpBatchLink dataLoader never drops an op React Query cancelled while it waits for the batch tick (groupItems checks item.aborted, nothing sets it; the observable teardown is a no-op), so each cancelQueries+invalidateQueries in one tick adds an identical query to one GET URL.
  guarded: no test yet
  where: monorepo-frontend:apps/stream-app/src/infra/trpc/client.tsx:71
  PRO-9993

## Incidents
- incident:PRO-10104 · The BO edit form omits s3_key when no new file is picked; updateAudioRecording keeps omitted = keep the stored file, null = clear, string = replace. [mem_26a3274e9b1a]
