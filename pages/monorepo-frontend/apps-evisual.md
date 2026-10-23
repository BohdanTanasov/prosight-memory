# Monorepo frontend › Apps evisual

owner: bohdantanasov (most confirmed records) · records: 1 (1 confirmed) · rules guarded by a test: 0 of 1 · last change: 2026-10-22

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- In evisual and mpk-admin, `pnpm analyze` (ANALYZE=true → withBundleAnalyzer) writes no bundle report: `next build` uses Turbopack and @next/bundle-analyzer only works with webpack, so it just prints a warning. The docs still present it as the bundle analyzer. [mem_171266e0d875]
  why: Found while raising next to 16.4.2: the plugin returns the config unchanged when process.env.TURBOPACK is set (it already did on 16.3.8).
  do: For a bundle report use `next analyze` (Turbopack analyzer, inside the app folder), or `next build --webpack` with ANALYZE=true.
  guarded: not yet
  where: monorepo-frontend:apps/evisual/next.config.ts:9
