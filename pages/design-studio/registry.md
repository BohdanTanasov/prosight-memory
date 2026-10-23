# Design studio › Registry

owner: bohdantanasov (most confirmed records) · records: 1 (1 confirmed) · rules guarded by a test: 0 of 1 · last change: 2026-10-15

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- A prototype decision id in meta.json that is not lowercase-with-dashes (e.g. camelCase `beforeAfter`) is silently dropped by the registry: no error, the decision just never appears in Rozhodnutia. Use `before-after`. [mem_8a3797d5fca6]
  why: The registry silently drops a prototype decision id that is not lowercase-with-dashes: no error, the decision never appears in Rozhodnutia.
  do: Name decision ids like before-after, never camelCase.
  guarded: not yet
  where: monorepo-frontend:apps/design-studio/src/studio/registry.ts:237
