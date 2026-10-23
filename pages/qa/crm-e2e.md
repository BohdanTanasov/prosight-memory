# QA › Crm e2e

owner: bohdantanasov (most confirmed records) · records: 4 (1 confirmed · 3 proposed) · rules guarded by a test: 0 of 3 · last change: 2026-10-21

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- Do not turn trace on in CI for the CRM/ePLAN configs while sign-in is typed inside the traced tests: Playwright traces record the fill() value, i.e. the test user's password, into the uploaded report. [mem_273fb312e10b] (proposed)
  why: Sign-in is typed inside the traced tests; a Playwright trace records the fill() value — the test user password — into the uploaded report.
  do: Keep trace: "off" in playwright.crm.config.ts (and the ePLAN config) while sign-in happens inside tests.
  guarded: not yet
  where: automation-tests:playwright.crm.config.ts
- Auth-hub sign-in skips the login form when the browser is already signed in, so a shared storageState in a CRM config would silently run tests that call signInThroughAppAs (other users, PASSWORD_1…) as the default user. [mem_cda43d1b37f2] (proposed)
  why: Auth-hub sign-in skips the login form when the browser is already signed in, so with a shared storageState every signInThroughAppAs (other users, PASSWORD_1…) would silently run as the default user.
  do: No shared storageState in the CRM config; sign in inside the test.
  guarded: not yet
  where: automation-tests:src/auth-hub.ts:53
- DevDeck's local ECOM approval (envMailApprove: reads the token from the mail sink, confirms it via docker exec tinker) is a host-side DevDeck API. The Playwright run/data containers get no DevDeck URL or env id (DEVDECK_* vars are scrubbed), so a test cannot reach SEK 'approved' through it. [mem_24ef6f97a423]
  why: envMailApprove reads the token from the mail sink and confirms it via docker exec tinker on the host; the Playwright run/data containers get no DevDeck URL or env id (DEVDECK_* vars are scrubbed).
  do: A test cannot reach SEK "approved" through DevDeck; seed the SEK state through data (.data.ts) or the API instead.
  guarded: not yet
  where: monorepo-frontend:tools/devdeck/src/core/envs/routes.ts:107
  TD-2146

## Testing notes (what a spec must know)
- QA case ids C<n> restart per technical description (C1 exists in both TD-2113 and TD-2146), so a [C<n>] in a test title only means something together with the TD key the spec file names. [mem_e5739a5c25b1] (proposed)
  why: QA case ids C<n> restart per technical description (C1 exists in both TD-2113 and TD-2146).
  do: Read a [C<n>] in a test title together with the TD key the spec file names; never join cases across TDs by number.
  where: automation-tests:tests/crm/crm-recruitment-emails.spec.ts:70

## Disputed / open
- Do not turn trace on in CI for the CRM/ePLAN configs while sign-in is typed inside the traced tests: Playwright traces record the fill() value, i.e. the test user's password, into the uploaded report. [mem_273fb312e10b] (proposed) — nobody confirmed it yet
- Auth-hub sign-in skips the login form when the browser is already signed in, so a shared storageState in a CRM config would silently run tests that call signInThroughAppAs (other users, PASSWORD_1…) as the default user. [mem_cda43d1b37f2] (proposed) — nobody confirmed it yet
- QA case ids C<n> restart per technical description (C1 exists in both TD-2113 and TD-2146), so a [C<n>] in a test title only means something together with the TD key the spec file names. [mem_e5739a5c25b1] (proposed) — nobody confirmed it yet
