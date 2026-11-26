# eVisual

owner: bohdantanasov (most confirmed records) · records: 3 (3 confirmed) · rules guarded by a test: 1 of 3 · last change: 2026-11-24

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- A production run whose `own` or `below` columns lack `netto_um` must be treated as outdated everywhere it is used: both the current-sync fetch and the statistics page must skip it as a cached result and trigger a fresh run instead of displaying stale data. [mem_efa8e7dba350]
  why: Review remark pointed out that `fresh` had no way to tell an old cached run from a new one, which led to adding `isOutdatedProduction` and applying it in both emakler-current-sync.tsx and emakler-stats.tsx.
  do: When touching production-run caching or display logic in emakler, keep the outdated check (missing netto_um) applied consistently in every place that consumes cached production results, not just one.
  guarded: no test yet
  where: monorepo-frontend:apps/evisual/src/features/emakler/containers/emakler-current-sync.tsx:113
  PRO-10729
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
