# Devdeck › Graph

owner: bohdantanasov (most confirmed records) · records: 2 (2 confirmed) · rules guarded by a test: 0 of 0 · last change: 2026-10-01

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## How it works
- The technical errors layer marks the code down to files (unit, module, file — at most three key parts); a piece of code inside a file has no mark of its own, so its place page reads the file's mark and says 'in its file'. [mem_3fc6cf9ca8ff]
  why: The technical errors layer marks code down to files (unit, module, file — at most three key parts); a piece of code inside a file has no mark of its own.
  do: A place page for code inside a file reads the file mark and says "in its file"; do not invent a finer mark.
  where: prosight-devdeck:web/src/features/graph/containers/PlacePage.tsx:130
- A used service on the product map (`svc:<unit>`) is the same place as the technical unit, so place keys resolve `svc:x` to `x`; `data:*` and `root:` are no place (no page). Keep this mapping in one helper when ids change. [mem_22986f18d7a1]
  why: A used service on the product map (svc:<unit>) is the same place as the technical unit, so one page serves both ids; data:* and root: have no page.
  do: Change the svc:x → x mapping only in place-keys.ts (one helper), never inline where ids are compared.
  where: prosight-devdeck:web/src/features/graph/ui/place-keys.ts:21
