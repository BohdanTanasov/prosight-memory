# Components

owner: bohdantanasov (most confirmed records) · records: 1 (1 confirmed) · rules guarded by a test: 0 of 1 · last change: 2026-11-13

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- `Tabs.Content` of @prosight/components keeps every tab mounted by default (`keepMounted = true`): all tabs' content renders at once, hidden ones included. [mem_a8ea036aab63]
  why: eVisual /provizie rendered five tabs of tables over ~1800 people plus five person pickers at once; switching a tab froze the page for 20+ s on test (PRO-10506).
  do: Pass `keepMounted={false}` on tabs whose content is heavy or fetches; keep the default only for light tabs that must keep their state.
  guarded: no test yet
  where: monorepo-frontend:packages/components/src/tabs/tabs-content/tabs-content.tsx:9
  PRO-10506
