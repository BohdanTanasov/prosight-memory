# CRM › Recruitment emails

owner: bohdantanasov (most confirmed records) · records: 5 (5 confirmed) · rules guarded by a test: 0 of 2 · last change: 2026-10-16

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## How it works
- communication_log.subject_variant is a plain text column (drizzle text({enum}) is a TS type only, no PG enum or CHECK). D32's 'additive PG enum migration' is not needed: adding subject variants is a TypeScript change. [mem_913551e17227]
  where: monorepo-frontend:apps/crm/src/features/communication/communication.schema.ts
  TD-2146 · TD-2146#D32

## Must not break
- Write subject and greeting assertions per formality; tests written from the tykanie list fail. [mem_6a602fd2ad46]
  why: Under vykanie the wizard preselects the *_SUBJECTS_VYKANIE wording («Vitajte v tíme», «3 pravidlá, ktoré Vám pomôžu uspieť»), not Subject1 of the tykanie list, and Kariérne stretnutie greets with «!» instead of «,» (PRO-10177, PR #5622). The default formality is Vykanie.
  guarded: no test yet
  where: monorepo-frontend:packages/email-templates/emails/recruitment/karierne-stretnutie.tsx:30
  PRO-10177
- Replace the vykanie copy with the design texts before release (PRO-10178). [mem_f6312bcbec7c]
  why: The design has vykanie frames only for "Dnes nie – spolupráca"; the vykanie copy of the five templates is the tykanie text in the formal register — still so on 2026-10-22 (TODO(PRO-10178) in hladame-posily.tsx:57). D14 gates the Nábor release on the design vykanie texts; PRO-10177 changed subjects and greetings only.
  guarded: no test yet
  where: monorepo-frontend:packages/email-templates/emails/recruitment/hladame-posily.tsx
  TD-2146 · TD-2146#D14

## Tried — did not work
- Typing into the CRM wizard's meeting date segments (dd/mm/yyyy spinbuttons) from Playwright is unreliable: digits get dropped between keystrokes (19 -> 09, 2026 -> 0026), and ArrowUp/PageUp stepping does not work either. Pick the date in the calendar instead (Open datepicker, then the day button named like 'pondelok 19. októbra 2026'); past days are clickable. [mem_a933282a7e19]
  where: automation-tests:helpers/crm/email-wizard.ts:285
  TD-2146

## Testing notes (what a spec must know)
- Nábor cards in the email wizard have empty descriptions, so their accessible button name is exactly the title (FS cards append the description). A locator expecting `^<title> ` misses every Nábor card; match `^<title>(\s|$)`. [mem_5f1a96920d0f]
  where: automation-tests:helpers/crm/email-wizard.ts:329
  PRO-10177
