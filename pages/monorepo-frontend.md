# Monorepo frontend

owner: bohdantanasov (most confirmed records) · records: 1 (1 confirmed) · rules guarded by a test: 0 of 1 · last change: 2026-10-22

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- The 'Test components' CI step runs in the image mcr.microsoft.com/playwright:v<version>-jammy, which must match the playwright/@playwright/test version pinned in the package.json files. [mem_c35f3fb06e39]
  why: Playwright needs the browsers built for its exact version; a version bump without the image tag leaves the components browser tests without matching browsers.
  do: Move the image tag in the same commit as playwright (and all apps' @playwright/test pins).
  guarded: no test yet
  where: monorepo-frontend:bitbucket-pipelines.yml:428
