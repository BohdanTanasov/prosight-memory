# CRM

owner: bohdantanasov (most confirmed records) · records: 4 (4 confirmed) · rules guarded by a test: 0 of 3 · last change: 2026-10-24

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- In the recruitment-kanban deal patch flow, buildPatchRecruitmentKanbanDealPayload overwrites a field with whatever value it is given except when that value is `undefined`; only `undefined` means "don't touch this field", while `null` is sent as an explicit clear. [mem_dbef37f4d043]
  why: Before commit e8b2ce516a the cooperationType key was only added when values.cooperationType !== null, so clearing an existing cooperation type on the deal silently did nothing; the fix makes toCommitRecruitmentStepInput set base.cooperationType = null explicitly.
  do: When adding or editing any field going through buildPatchRecruitmentKanbanDealPayload, send null explicitly to clear a previously-set value and never just omit the key.
  guarded: no test yet
  where: monorepo-frontend:apps/crm/src/shared/ui/cooperation-type-radio-field.tsx:57
  PRO-10062
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

## Decisions and why
- Task group counts (groupCounts) are intentionally not cached even though they are refetched on every search page; caching was skipped because counts would quickly go stale whenever tasks are edited. [mem_b55e0744401a]
  why: Autor cache zámerne vynechal, lebo by po úpravách úloh zastarávala.
  do: Before adding caching to groupCounts or search results in task.service.ts, confirm task edits still invalidate it correctly or avoid caching it at all.
  where: monorepo-frontend:apps/crm/src/features/task/task.service.ts:397
  PRO-10062
