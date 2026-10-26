# CRM › Database

owner: bohdantanasov (most confirmed records) · records: 3 (3 confirmed) · rules guarded by a test: 3 of 3 · last change: 2026-10-21

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- Keep the week presets, month cells and year arrows role=button with their Slovak labels. [mem_f0739978efbb]
  why: automation-tests (crm-filters.spec.ts, crm-nabor-kanban.spec.ts) click the presets with getByRole("button", { name: "Minulý týždeň" }) and the clear × by "Vymazať výber rýchleho filtra"; DropdownMenuItem (role menuitem) breaks them.
  guarded: test:automation-tests:tests/crm/crm-filters.spec.ts:253, test:automation-tests:tests/crm/crm-nabor-kanban.spec.ts:59
  where: monorepo-frontend:apps/crm/src/features/contact/ui/quick-case-filter-dropdown.tsx:196
  PRO-10063
- Keep Pohlavie and Forma komunikácie required only in createContactFormSchema, never in CreateContactInput. [mem_d77da8bb645d]
  why: CreateContactInput is also the Excel import and the API schema; a required rule there would refuse import rows without pohlavie (pending Q39) and the wizard write-back.
  guarded: test:monorepo-frontend:apps/crm/src/features/contact/contact.dto.test.ts:190
  where: monorepo-frontend:apps/crm/src/features/contact/contact.dto.ts
  TD-2146
- Every Databáza SELECT filter id gets its own branch in transformFiltersForApi and a case in filter-transforms.test.ts. [mem_b796c5a08faf]
  why: A filter offered in the UI (and supported by contact.dto and platform-list-query) without a branch in transformFiltersForApi is silently dropped and the list stays unfiltered — typ spolupráce was (PRO-10055 QA).
  guarded: test:monorepo-frontend:apps/crm/src/features/contact/utils/filter-transforms.test.ts:27
  where: monorepo-frontend:apps/crm/src/features/contact/utils/filter-transforms.ts:121
  PRO-10055
