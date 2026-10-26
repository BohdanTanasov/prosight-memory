# Monorepo frontend

owner: bohdantanasov (most confirmed records) · records: 2 (2 confirmed) · rules guarded by a test: 0 of 2 · last change: 2026-10-24

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- apps/eplan-frontend and packages/salesman-profile must keep an explicit `next` devDependency declaration; without it pnpm can resolve the next-intl peer to a stray nested next version dragged in by an unrelated package instead of the app's real next version. [mem_08883c1116e9]
  why: Commit 6a99cf50c9 added explicit next devDependency entries to these two packages specifically to stop pnpm from picking next@16.4.1 nested under @react-email/ui as the peer for next-intl. @react-email/ui (packages/email-templates) brings its own nested next, and pnpm resolved next-intl's peer to it.
  do: Keep the explicit next devDependency in eplan-frontend and salesman-profile and check it stays present after future lockfile/dependency bumps.
  guarded: no test yet
  BATCH-2026
- The 'Test components' CI step runs in the image mcr.microsoft.com/playwright:v<version>-jammy, which must match the playwright/@playwright/test version pinned in the package.json files. [mem_c35f3fb06e39]
  why: Playwright needs the browsers built for its exact version; a version bump without the image tag leaves the components browser tests without matching browsers.
  do: Move the image tag in the same commit as playwright (and all apps' @playwright/test pins).
  guarded: no test yet
  where: monorepo-frontend:bitbucket-pipelines.yml:428
