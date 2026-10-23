# Devdeck › Theme

owner: bohdantanasov (most confirmed records) · records: 2 (2 confirmed) · rules guarded by a test: 0 of 2 · last change: 2026-10-02

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- The dark theme turns the colour ramps over (white, gray-100, red-200 go dark; bg-gray-900 goes light). Code, logs and payloads must use the kit's CODE_SURFACE and the CODE_INK pairs, never their own light inks on a dark box, or they go unreadable in the dark theme. [mem_5d98dd064073]
  why: The dark theme turns the colour ramps over (white, gray-100, red-200 go dark; bg-gray-900 goes light), so an own light ink on a dark box goes unreadable.
  do: Code, logs and payloads use the kit CODE_SURFACE and CODE_INK pairs from code-surface.ts; no own light inks on dark boxes.
  guarded: not yet
  where: prosight-devdeck:web/src/core-ui/devdeck/code-surface.ts:1
- The DS kit's neon-green ramp is not flipped in .dark (brand colour), while gray ramps are. Any kit component that pairs a neon-green-50/100 background with a flipping ink (dark:text-gray-900) is light-on-light in the dark theme; the app patches these in web/src/styles.css until the kit gets dark values. [mem_d6c83148df1c]
  why: The kit neon-green ramp (brand colour) is not flipped in .dark while gray ramps are, so neon-green-50/100 with a flipping ink (dark:text-gray-900) is light-on-light.
  do: Patch such kit components in web/src/styles.css (the neon block, still there on 2026-10-22) until the kit ships dark values; then remove the patch.
  guarded: not yet
  where: prosight-devdeck:web/src/styles.css:121
