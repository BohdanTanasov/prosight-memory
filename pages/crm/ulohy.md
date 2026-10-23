# CRM › Ulohy

owner: bohdantanasov (most confirmed records) · records: 1 (1 confirmed) · rules guarded by a test: 0 of 0 · last change: 2026-10-12

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## How it works
- The Úlohy table pages at 25 rows sorted by deadline ascending, and its per-page select only renders once the list query has data (total > 10); skeleton rows have role=row too. A test that waits for any row and then counts the select can miss it, so future tasks land on page 2 in a full e2e run. Wait for a row with a contact link, then page through. [mem_845963fce33c]
  why: The table pages at 25 rows sorted by deadline ascending; the per-page select renders only once the list query has data (total > 10) and skeleton rows have role=row too, so a test that waits for any row can miss the select and a future task lands on page 2 in a full run.
  do: Wait for a row with a contact link, then page through (or switch to 100 per page).
  where: monorepo-frontend:apps/crm/src/features/task/containers/task-list.tsx:33
  PRO-10055
