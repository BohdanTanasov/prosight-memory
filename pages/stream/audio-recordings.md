# Stream › Audio recordings

owner: bohdantanasov (most confirmed records) · records: 1 (1 confirmed) · rules guarded by a test: 1 of 1 · last change: 2026-10-08

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- updateAudioRecording treats s3_key three ways: omitted keeps the stored file, null clears it, a string replaces it. The BO edit form must omit s3_key when no new file is picked. Sending null wiped the files of production recordings 203-207. [mem_26a3274e9b1a]
  why: Sending s3_key: null wiped the files of production recordings 203-207 (PRO-10104).
  do: The BO edit form omits s3_key when no new file is picked; updateAudioRecording keeps omitted = keep the stored file, null = clear, string = replace.
  guarded: test:monorepo-frontend:packages/stream-api/src/services/stream-write.service.test.ts:247
  where: monorepo-frontend:packages/stream-api/src/services/stream-write.service.ts:241
  PRO-10104

## Incidents
- incident:PRO-10104 · updateAudioRecording treats s3_key three ways: omitted keeps the stored file, null clears it, a string replaces it. The BO edit form must omit s3_key when no new file is picked. Sending null wiped the files of production recordings 203-207. [mem_26a3274e9b1a]
