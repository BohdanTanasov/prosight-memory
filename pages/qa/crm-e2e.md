# QA › CRM e2e

owner: bohdantanasov (most confirmed records) · records: 4 (1 confirmed · 3 proposed) · rules guarded by a test: 0 of 3 · last change: 2026-10-21

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- Keep trace: "off" in playwright.crm.config.ts (and the ePLAN config) while sign-in happens inside tests. [mem_273fb312e10b] (unconfirmed)
  why: Sign-in is typed inside the traced tests; a Playwright trace records the fill() value — the test user password — into the uploaded report.
  guarded: no test yet
  where: automation-tests:playwright.crm.config.ts
- No shared storageState in the CRM config; sign in inside the test. [mem_cda43d1b37f2] (unconfirmed)
  why: Auth-hub sign-in skips the login form when the browser is already signed in, so with a shared storageState every signInThroughAppAs (other users, PASSWORD_1…) would silently run as the default user.
  guarded: no test yet
  where: automation-tests:src/auth-hub.ts:53
- A test cannot reach SEK "approved" through DevDeck; seed the SEK state through data (.data.ts) or the API instead. [mem_24ef6f97a423]
  why: envMailApprove reads the token from the mail sink and confirms it via docker exec tinker on the host; the Playwright run/data containers get no DevDeck URL or env id (DEVDECK_* vars are scrubbed).
  guarded: no test yet
  where: monorepo-frontend:tools/devdeck/src/core/envs/routes.ts:107
  TD-2146

## Testing notes (what a spec must know)
- QA case ids C<n> restart per technical description (C1 exists in both TD-2113 and TD-2146), so a [C<n>] in a test title only means something together with the TD key the spec file names. [mem_e5739a5c25b1] (unconfirmed)
  where: automation-tests:tests/crm/crm-recruitment-emails.spec.ts:70
