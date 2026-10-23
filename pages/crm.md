# CRM

owner: bohdantanasov (most confirmed records) · records: 4 (4 confirmed) · rules guarded by a test: 0 of 2 · last change: 2026-10-12

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- Any change in financial-mediation-kanban.service.ts or board/kanban-step-tasks.ts is checked on both the products kanban and /kanban/financial-mediation. [mem_2e4423766965]
  why: PRO-9859 (commit 174937e3d) changed the shared service for the product boards and the FS board stopped loading products (PRO-9977).
  guarded: no test yet
  where: monorepo-frontend:apps/crm/src/features/financial-mediation-kanban/financial-mediation-kanban.service.ts, monorepo-frontend:apps/crm/src/features/financial-mediation-kanban/board/kanban-step-tasks.ts
  PRO-9977
- Changing RemoveFromKanbanInput or the service behind financialMediationKanban.removeFromKanban: re-test card removal and blacklisting on the FS board. [mem_64c5bd4b3569]
  why: PRO-9641 (2f96e520d) reshaped RemoveFromKanbanInput and the mutation answered 500 (PRO-10005).
  guarded: no test yet
  where: monorepo-frontend:apps/crm/src/features/financial-mediation-kanban/financial-mediation-kanban.router.ts:33
  PRO-10005

## Testing notes (what a spec must know)
- The contact-history step header renders title and date as sibling flex spans; the explicit {' '} between them keeps the text node 'Obchodný rozhovor 17.10.2026' (screen readers, Playwright getByText). Do not drop them in a refactor. [mem_ede955cbe739]
  where: monorepo-frontend:packages/crm-timeline/src/features/contact/profile/ui/contact-profile-history-step.tsx:505
  PRO-10055
- The Úlohy table pages at 25 rows sorted by deadline ascending, and its per-page select only renders once the list query has data (total > 10); skeleton rows have role=row too. A test that waits for any row and then counts the select can miss it, so future tasks land on page 2 in a full e2e run. Wait for a row with a contact link, then page through. [mem_845963fce33c]
  where: monorepo-frontend:apps/crm/src/features/task/containers/task-list.tsx:33
  PRO-10055
