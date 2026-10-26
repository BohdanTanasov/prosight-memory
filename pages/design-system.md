# Design system

owner: bohdantanasov (most confirmed records) · records: 2 (2 confirmed) · rules guarded by a test: 0 of 2 · last change: 2026-10-24

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- A side drawer needs <Modal variant="sidebar"> on the root; <Modal.Body type="sidebar"> alone renders a centred dialog. [mem_de7476d362a5]
  why: The Root `variant` (default "modal") overrides Modal.Body `type`.
  guarded: no test yet
  where: monorepo-frontend:tools/devdeck/web/src/core-ui/devdeck/DetailDrawer.tsx:73
- The shared Card's `open` prop falls back to its internal `localOpen` (which defaults to false) as soon as the external condition driving `open` becomes false again, so any consumer that only sets `open=true` while some transient condition (like an error) holds will see the card auto-close the instant that condition clears. [mem_e9576e393a1e]
  why: Before the fix the card was driven `open` only while the error was active and then fell back to `localOpen` = false (packages/components/src/card/card.tsx:79-80), so it closed again once the error disappeared.
  do: When controlling Card's open state from a transient condition, keep a persistent local boolean (set true by the event, never auto-reset) and pass that as `open`/`onOpenChange` instead of deriving `open` directly from the transient condition.
  guarded: no test yet
  where: monorepo-frontend:apps/crm/src/features/recruitment-kanban/ui/board/recruitment-step-modal-view.tsx:374
  PRO-10062
