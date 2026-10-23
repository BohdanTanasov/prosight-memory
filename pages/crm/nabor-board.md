# CRM › Nábor board

owner: bohdantanasov (most confirmed records) · records: 4 (4 confirmed) · rules guarded by a test: 0 of 0 · last change: 2026-10-17

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Decisions and why
- Render with ContactProcessStatus.label(status, "recruitment"), never shortLabel; pass processType="recruitment" to ContactStatusPill. [mem_67a0b959727e]
  why: Patrik F3 remark (15 Oct 2026): a Nábor status reads "<status> spolupracovník" for all five, Dnes nie / Nie / Blacklist included; FS keeps the bare forms.
  where: monorepo-frontend:packages/crm-timeline/src/shared/enums/crm/contact-process-status.ts:34
  PRO-10055
- A nábor step whose outcome books nothing (Ďalší ÚS, Ďalšie KS) sends the corrected held meeting also as meeting*, like FS. [mem_3bba1c534f9e]
  why: The deal booking prefills the modal and the history; held_meeting alone is only the step snapshot, so an edit stored there was never shown again (mem_538bbc132547, the bug this decision closed).
  where: monorepo-frontend:apps/crm/src/features/recruitment-kanban/board/recruitment-step-form.ts:272
  PRO-10055

## Testing notes (what a spec must know)
- In the Nábor step modal, the outcomes are buttons with role=radio inside a radiogroup labelled 'Aký je výstup?'. Every other choice (reason, removal-status, cooperation-type, closing-rating) is a RadioField: native inputs grouped only by their name attribute, with no radiogroup role. Tests must scope those choices with input[name=...], not getByRole('radiogroup'). [mem_3c6f4df3abf0]
  where: monorepo-frontend:apps/crm/src/features/recruitment-kanban/ui/board/recruitment-step-modal-view.tsx:172
  PRO-10055
- The Nábor board's h1 renders its count as a span with aria-label '<n> rozpracovaných' (plural-aware). So the heading's accessible name is probably not literally 'Nábor (12)'; read the count from the text content. The count covers only open deals: Dohodnutá spolupráca is excluded. [mem_ec69f11dac26]
  where: monorepo-frontend:apps/crm/src/features/recruitment-kanban/ui/board/recruitment-kanban-board-view.tsx:77
  PRO-10055
