# Evisual › Network

owner: bohdantanasov (most confirmed records) · records: 1 (1 confirmed) · rules guarded by a test: 1 of 1 · last change: 2026-10-15

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- eVision reads are not read-only: network.summary and network.resolve call ensureNetwork, which inserts a 'current' network row if missing — pointing a local app at a production DB writes to it just by opening the page. [mem_fe824c97ca96]
  why: network.summary and network.resolve call ensureNetwork, which inserts a "current" network row when it is missing.
  do: Never point a local eVision at a production DB to look — a read writes; diagnose through a signed-in browser on the prod app (PRO-10143).
  guarded: test:monorepo-frontend:apps/evisual/src/features/network/network.service.test.ts:84 (documents the insert)
  where: monorepo-frontend:apps/evisual/src/features/network/network.service.ts:178
  PRO-10143
