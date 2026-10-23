# eVisual

owner: bohdantanasov (most confirmed records) · records: 2 (2 confirmed) · rules guarded by a test: 1 of 2 · last change: 2026-10-22

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- In evisual and mpk-admin, `pnpm analyze` (ANALYZE=true → withBundleAnalyzer) writes no bundle report: `next build` uses Turbopack and @next/bundle-analyzer only works with webpack, so it just prints a warning. The docs still present it as the bundle analyzer. [mem_171266e0d875]
  why: Found while raising next to 16.4.2: the plugin returns the config unchanged when process.env.TURBOPACK is set (it already did on 16.3.8).
  do: For a bundle report use `next analyze` (Turbopack analyzer, inside the app folder), or `next build --webpack` with ANALYZE=true.
  guarded: no test yet
  where: monorepo-frontend:apps/evisual/next.config.ts:9
- Never point a local eVision at a production DB to look — a read writes; diagnose through a signed-in browser on the prod app (PRO-10143). [mem_fe824c97ca96]
  why: network.summary and network.resolve call ensureNetwork, which inserts a "current" network row when it is missing.
  guarded: test:monorepo-frontend:apps/evisual/src/features/network/network.service.test.ts:84 (documents the insert)
  where: monorepo-frontend:apps/evisual/src/features/network/network.service.ts:178
  PRO-10143
