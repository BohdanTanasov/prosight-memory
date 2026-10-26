# CRM › Nabor board

owner: bohdantanasov (most confirmed records) · records: 3 (3 confirmed) · rules guarded by a test: 0 of 1 · last change: 2026-10-24

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- The recruitment-step-modal shows contact name/phone/email read from the cached card data (`view.card.contact`) held by the `trpc.recruitmentKanban.listByColumn` query, not from a fresh contact fetch. [mem_1ea68098d5c4]
  why: Reviewer noted the modal reads contact fields from `view.card.contact` sourced from `cardsById` over `recruitmentKanban.listByColumn`, so without invalidating that query a contact edit stays invisible there.
  do: Any place that edits a contact used by a recruitment kanban card must invalidate/refetch `trpc.recruitmentKanban.listByColumn` (e.g. via `pathFilter()` in its `onSettled`), not just the contact's own query.
  guarded: no test yet
  where: monorepo-frontend:apps/crm/src/features/recruitment-kanban/containers/recruitment-step-modal.tsx:184
  PRO-10062

## Decisions and why
- Render with ContactProcessStatus.label(status, "recruitment"), never shortLabel; pass processType="recruitment" to ContactStatusPill. [mem_67a0b959727e]
  why: Patrik F3 remark (15 Oct 2026): a Nábor status reads "<status> spolupracovník" for all five, Dnes nie / Nie / Blacklist included; FS keeps the bare forms.
  where: monorepo-frontend:packages/crm-timeline/src/shared/enums/crm/contact-process-status.ts:34
  PRO-10055
- A nábor step whose outcome books nothing (Ďalší ÚS, Ďalšie KS) sends the corrected held meeting also as meeting*, like FS. [mem_3bba1c534f9e]
  why: The deal booking prefills the modal and the history; held_meeting alone is only the step snapshot, so an edit stored there was never shown again (mem_538bbc132547, the bug this decision closed).
  where: monorepo-frontend:apps/crm/src/features/recruitment-kanban/board/recruitment-step-form.ts:272
  PRO-10055
