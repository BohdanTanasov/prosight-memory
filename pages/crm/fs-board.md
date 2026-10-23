# CRM › Fs board

owner: bohdantanasov (most confirmed records) · records: 2 (2 confirmed) · rules guarded by a test: 0 of 2 · last change: 2026-10-01

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- The products kanban and the FS board (/kanban/financial-mediation) share this service: PRO-9859 (commit 174937e3d) changed it for product boards and the FS board stopped loading products (PRO-9977). A change here must be checked on both boards. [mem_2e4423766965]
  why: PRO-9859 (commit 174937e3d) changed the shared service for the product boards and the FS board stopped loading products (PRO-9977).
  do: Any change in financial-mediation-kanban.service.ts or board/kanban-step-tasks.ts is checked on both the products kanban and /kanban/financial-mediation.
  guarded: not yet
  where: monorepo-frontend:apps/crm/src/features/financial-mediation-kanban/financial-mediation-kanban.service.ts, monorepo-frontend:apps/crm/src/features/financial-mediation-kanban/board/kanban-step-tasks.ts
  PRO-9977
- 'Remove from KANBAN' on the FS board goes through tRPC financialMediationKanban.removeFromKanban; PRO-9641 (commit 2f96e520d) reshaped its input DTO and the mutation failed with 500 (PRO-10005). Changing RemoveFromKanbanInput or the service it calls breaks card removal and blacklisting. [mem_64c5bd4b3569]
  why: PRO-9641 (2f96e520d) reshaped RemoveFromKanbanInput and the mutation answered 500 (PRO-10005).
  do: Changing RemoveFromKanbanInput or the service behind financialMediationKanban.removeFromKanban: re-test card removal and blacklisting on the FS board.
  guarded: not yet
  where: monorepo-frontend:apps/crm/src/features/financial-mediation-kanban/financial-mediation-kanban.router.ts:33
  PRO-10005
