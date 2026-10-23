# CRM › Database

owner: bohdantanasov (most confirmed records) · records: 4 (3 confirmed · 1 proposed) · rules guarded by a test: 3 of 3 · last change: 2026-10-21

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- The date quick filter's week presets, month cells and year arrows must stay role=button with their Slovak labels: automation-tests (TD-2113-qa crm-filters.spec.ts, crm-nabor-kanban.spec.ts) click them with getByRole('button', { name: 'Minulý týždeň' }) and the clear «×» by 'Vymazať výber rýchleho filtra'. Turning them into DropdownMenuItem (role menuitem) breaks those QA tests. [mem_f0739978efbb] (proposed)
  why: automation-tests (crm-filters.spec.ts, crm-nabor-kanban.spec.ts) click the presets with getByRole("button", { name: "Minulý týždeň" }) and the clear × by "Vymazať výber rýchleho filtra"; DropdownMenuItem (role menuitem) breaks them.
  do: Keep the week presets, month cells and year arrows role=button with their Slovak labels.
  guarded: test:automation-tests:tests/crm/crm-filters.spec.ts:253, test:automation-tests:tests/crm/crm-nabor-kanban.spec.ts:59
  where: monorepo-frontend:apps/crm/src/features/contact/ui/quick-case-filter-dropdown.tsx:196
  PRO-10063
- Pohlavie and Forma komunikácie are required only in the contact form (createContactFormSchema). CreateContactInput is also the Excel import's and the API's schema — adding the rule there would refuse import rows without pohlavie (pending Q39) and the wizard write-back. [mem_d77da8bb645d]
  why: CreateContactInput is also the Excel import and the API schema; a required rule there would refuse import rows without pohlavie (pending Q39) and the wizard write-back.
  do: Keep Pohlavie and Forma komunikácie required only in createContactFormSchema, never in CreateContactInput.
  guarded: test:monorepo-frontend:apps/crm/src/features/contact/contact.dto.test.ts:190
  where: monorepo-frontend:apps/crm/src/features/contact/contact.dto.ts
  TD-2146
- Every Databáza SELECT filter id needs its own branch in transformFiltersForApi; a filter offered in the UI (and supported by contact.dto and platform-list-query) without a branch here is silently dropped and the list stays unfiltered (typ spolupráce was). [mem_b796c5a08faf]
  why: A filter offered in the UI (and supported by contact.dto and platform-list-query) without a branch in transformFiltersForApi is silently dropped and the list stays unfiltered — typ spolupráce was (PRO-10055 QA).
  do: Every Databáza SELECT filter id gets its own branch in transformFiltersForApi and a case in filter-transforms.test.ts.
  guarded: test:monorepo-frontend:apps/crm/src/features/contact/utils/filter-transforms.test.ts:27
  where: monorepo-frontend:apps/crm/src/features/contact/utils/filter-transforms.ts:121
  PRO-10055

## Tried — did not work
- In Databáza the row's Nábor/FS status menu does not open on a mouse click, because the table's dnd-kit sensors take the pointer press. Focus the status button and press Enter, then focus the menuitem and press Enter. The 'Nový stav' picker inside the status modal opens on a normal click. [mem_d3c0fc8788df]
  why: The table dnd-kit sensors take the pointer press, so the row Nábor/FS status menu does not open on a mouse click.
  do: Focus the status button and press Enter, then focus the menuitem and press Enter; the "Nový stav" picker inside the status modal opens on a normal click.
  where: automation-tests:helpers/crm/email-wizard.ts:420
  TD-2146

## Disputed / open
- The date quick filter's week presets, month cells and year arrows must stay role=button with their Slovak labels: automation-tests (TD-2113-qa crm-filters.spec.ts, crm-nabor-kanban.spec.ts) click them with getByRole('button', { name: 'Minulý týždeň' }) and the clear «×» by 'Vymazať výber rýchleho filtra'. Turning them into DropdownMenuItem (role menuitem) breaks those QA tests. [mem_f0739978efbb] (proposed) — nobody confirmed it yet
