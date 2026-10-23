# Devdeck › Spec

owner: bohdantanasov (most confirmed records) · records: 1 (1 confirmed) · rules guarded by a test: 0 of 0 · last change: 2026-10-09

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Testing notes (what a spec must know)
- `ask` is no step of the process map (RUN_STEP maps it under `brief`), so the deck starts it through the gate-tech step's own `run`; SpecPage.runStep maps the id `gate-tech` to the `ask` kind. [mem_6ad670c77044]
  why: `ask` is no step of the process map (RUN_STEP maps it under `brief`).
  do: The deck starts the ask through the gate-tech step own run; SpecPage.runStep maps the id gate-tech to the ask kind — keep the mapping when the process map changes.
  where: monorepo-frontend:tools/devdeck/web/src/features/spec/containers/SpecPage.tsx:182
