# Design system › Kit

owner: bohdantanasov (most confirmed records) · records: 2 (2 confirmed) · rules guarded by a test: 0 of 1 · last change: 2026-10-02

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- DS Modal: the Root's `variant` (default 'modal') overrides Modal.Body's `type`, so `<Modal.Body type="sidebar">` alone renders a centred dialog. A side drawer needs `<Modal variant="sidebar">` on the root. [mem_de7476d362a5]
  why: The Root `variant` (default "modal") overrides Modal.Body `type`.
  do: A side drawer needs <Modal variant="sidebar"> on the root; <Modal.Body type="sidebar"> alone renders a centred dialog.
  guarded: not yet
  where: prosight-devdeck:web/src/core-ui/devdeck/DetailDrawer.tsx:73

## Testing notes (what a spec must know)
- The kit's PillTabs activate on mouseDown (Radix), so a test switching tabs must fire mouseDown with button 0; fireEvent.click leaves the tab unchanged and the next findByTestId times out. [mem_84f470d02b98]
  why: Radix tabs (the kit PillTabs) activate on mouseDown; fireEvent.click leaves the tab unchanged and the next findByTestId times out.
  do: In a test that switches PillTabs fire mouseDown with button 0.
  where: prosight-devdeck:web/src/features/graph/place.test.tsx:60
