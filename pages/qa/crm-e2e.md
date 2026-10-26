# QA › CRM e2e

owner: bohdantanasov (most confirmed records) · records: 6 (6 confirmed) · rules guarded by a test: 1 of 5 · last change: 2026-10-24

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- CRM blocks ePLAN client creation for accounts with the tipér/trainee role, even when ePLAN creation is otherwise enabled on the contact. [mem_860fe6531bcb]
  why: Error: CRM contact creation failed with HTTP 403: V CRM tipéra/trainee nemožno vytvoriť klienta v ePLANe.
  do: When testing or automating CRM POST contact with ePLAN creation, use a non-trainee CRM role/account, not a tipér/trainee one.
  guarded: test:automation-tests:tests/crm/crm-eplan-post-mapping.spec.ts:33
  where: automation-tests:tests/crm/crm-eplan-post-mapping.spec.ts:33
- Keep trace: "off" in playwright.crm.config.ts (and the ePLAN config) while sign-in happens inside tests. [mem_273fb312e10b]
  why: Sign-in is typed inside the traced tests; a Playwright trace records the fill() value — the test user password — into the uploaded report.
  guarded: no test yet
  where: automation-tests:playwright.crm.config.ts
- The e2e helper selectStatusFromMenu matches recruitment status menu items by exact label text, which comes from ContactProcessStatus.label(code, processType, locale); any change to that label wording/translation will silently break the e2e selector without a compile-time link between them. [mem_80a0381278cc]
  why: Reviewer flagged that selectStatusFromMenu relies on exact label match while the menu label is generated via ContactProcessStatus.label(), and the fix hardcoded 'Dnes nie spolupracovník' / RECRUITMENT_STATUS_LABELS.blacklist instead of deriving it from the shared source.
  do: When changing any ContactProcessStatus label text or translation, also check and update the matching string literals used in apps/crm/e2e/helpers/process-status-change.ts and quick-case-filters.ts.
  guarded: no test yet
  where: monorepo-frontend:apps/crm/e2e/helpers/process-status-change.ts:20
  PRO-10062
- No shared storageState in the CRM config; sign in inside the test. [mem_cda43d1b37f2]
  why: Auth-hub sign-in skips the login form when the browser is already signed in, so with a shared storageState every signInThroughAppAs (other users, PASSWORD_1…) would silently run as the default user.
  guarded: no test yet
  where: automation-tests:src/auth-hub.ts:53
- A test cannot reach SEK "approved" through DevDeck; seed the SEK state through data (.data.ts) or the API instead. [mem_24ef6f97a423]
  why: envMailApprove reads the token from the mail sink and confirms it via docker exec tinker on the host; the Playwright run/data containers get no DevDeck URL or env id (DEVDECK_* vars are scrubbed).
  guarded: no test yet
  where: monorepo-frontend:tools/devdeck/src/core/envs/routes.ts:107
  TD-2146

## Testing notes (what a spec must know)
- QA case ids C<n> restart per technical description (C1 exists in both TD-2113 and TD-2146), so a [C<n>] in a test title only means something together with the TD key the spec file names. [mem_e5739a5c25b1]
  where: automation-tests:tests/crm/crm-recruitment-emails.spec.ts:70
