# DevDeck

owner: bohdantanasov (most confirmed records) · records: 8 (6 confirmed · 2 proposed) · rules guarded by a test: 0 of 2 · last change: 2026-10-22

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## How it works
- The technical errors layer marks the code down to files (unit, module, file — at most three key parts); a piece of code inside a file has no mark of its own, so its place page reads the file's mark and says 'in its file'. [mem_3fc6cf9ca8ff]
  where: monorepo-frontend:tools/devdeck/web/src/features/graph/containers/PlacePage.tsx:130
- A used service on the product map (`svc:<unit>`) is the same place as the technical unit, so place keys resolve `svc:x` to `x`; `data:*` and `root:` are no place (no page). Keep this mapping in one helper when ids change. [mem_22986f18d7a1]
  where: monorepo-frontend:tools/devdeck/web/src/features/graph/ui/place-keys.ts:21

## Must not break
- Code, logs and payloads use the kit CODE_SURFACE and CODE_INK pairs from code-surface.ts; no own light inks on dark boxes. [mem_5d98dd064073]
  why: The dark theme turns the colour ramps over (white, gray-100, red-200 go dark; bg-gray-900 goes light), so an own light ink on a dark box goes unreadable.
  guarded: no test yet
  where: monorepo-frontend:tools/devdeck/web/src/core-ui/devdeck/code-surface.ts:1
- Patch such kit components in web/src/styles.css (the neon block, still there on 2026-10-22) until the kit ships dark values; then remove the patch. [mem_d6c83148df1c]
  why: The kit neon-green ramp (brand colour) is not flipped in .dark while gray ramps are, so neon-green-50/100 with a flipping ink (dark:text-gray-900) is light-on-light.
  guarded: no test yet
  where: monorepo-frontend:tools/devdeck/web/src/styles.css:121

## Decisions and why
- Use conflictCount() for the card title and the "What to do" step, never links.conflicts.length. [mem_cd373b523f4a] (unconfirmed)
  why: Rows are deduped per package+range and a family peers merged (react+react-dom), so the count reads 11 where the daemon lists 12 conflicts.
  where: monorepo-frontend:tools/devdeck/web/src/features/deps/ui/links.ts:1
- DevDeck orchestrates compose and `exec` over the scripts and adds readiness facts, proposals and the Start-work mount steps (docs/plans/local-env.md §13); it does not describe the stack itself. [mem_565070df1d66]
  why: platform.dev.sh / eplan.dev.sh are the team launcher (Windows included), so a DevDeck-only stack description would fork the team setup.
  where: docker-dev-env:platform.dev.sh:1

## Testing notes (what a spec must know)
- The custom pipeline `devdeck-coverage` exists only on branches whose bitbucket-pipelines.yml has it (feature/devdeck until it merges to develop). Starting it on a branch without it fails, so tests/run defaults to the newest run's branch, not the mainline. [mem_ff3a99f20004] (unconfirmed)
  where: monorepo-frontend:tools/devdeck/src/features/tests/daemon/read.ts:54
- `ask` is no step of the process map (RUN_STEP maps it under `brief`), so the deck starts it through the gate-tech step's own `run`; SpecPage.runStep maps the id `gate-tech` to the `ask` kind. [mem_6ad670c77044]
  where: monorepo-frontend:tools/devdeck/web/src/features/spec/containers/SpecPage.tsx:182
