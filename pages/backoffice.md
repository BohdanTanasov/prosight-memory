# Backoffice

owner: bohdantanasov (most confirmed records) · records: 5 (5 confirmed) · rules guarded by a test: 0 of 2 · last change: 2026-10-22

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## How it works
- «Dátum typu spolupráce» in product's words is `cooperation_start_date` (BO app label «Začiatok spolupráce», edited as one pair with `cooperation_type`), synced from Emakler; «test accounts» are Prosight IDs 666666–899999, prefix 9 = backoffice, prefix 1 = PFA (SalesmanRepository / salesmen-type.ts). [mem_21ec1357bd3d]
  why: On TD-2229 the deck asked product what «Dátum typu spolupráce» means; product answered it is a term in daily use, readable from BO app Spolupracovníci.
  where: platform-salesmen:app/Models/SalesmanBase.php:36
  TD-2229 · TD-2229#D8

## Must not break
- @tiptap/extension-placeholder, @tiptap/react and @tiptap/starter-kit in backoffice must always be on the same exact version; tiptap packages peer-depend on an exact @tiptap/core/@tiptap/pm version. [mem_a65da0046fce]
  why: Placeholder 3.32.0 pulls @tiptap/extensions 3.32.0 which peers core/pm 3.32.0 exactly, while react/starter-kit 3.31.4 pin 3.31.4: raising one alone installs two @tiptap/core copies and mixed prosemirror-model, breaking the conference description editor.
  do: Raise all three together; after the lockfile update check there is a single prosemirror-model/transform/view entry.
  guarded: no test yet
  where: monorepo-frontend:apps/backoffice/package.json:47
- Before touching the identity detail salesman panel or create-success verify, decide the gate for platform-salesmen reads; until then both fail for operators without eplan-access. [mem_0d6d9d6e4490]
  why: Operators without eplan-access are denied by the ePLAN gateway, and the BFF opens GET /salesmen/1.0/{id} only to backoffice-salesmen* operators, so moving the panel to platform-salesmen needs a decision on the gate.
  guarded: no test yet
  where: monorepo-frontend:apps/backoffice/src/features/identities/containers/identity-detail-page.tsx:55

## Decisions and why
- Access to a BackOffice action or section is always a Cognito user group: a new permission = a new `UserGroups` entry + a `SalesmenActions` value + its row in `SALESMEN_ACTION_GROUPS` (UI gate via `canPerform`), the route→action row in `apps/backoffice/app/lib/platform-proxy.ts`, and the same route→group row in platform-cognito-api; the group itself is created in Cognito and people are added there by the access admin. [mem_5071c12b5b1e]
  why: TD-2082/TD-2089 set this as the one way BO app manages access; on TD-2229 product (Maroš Sedláček) had to answer «who may export» as a product question and asked that this kind of question never be asked again.
  do: Never ask product how a BO permission works; ask only who goes into the new group at the start.
  where: monorepo-frontend:apps/backoffice/src/features/auth/auth-groups.ts:57
  TD-2229 · TD-2229#D7
- Read codelists platform → ePLAN (operator token) → local fallback: BFF /api/platform/codelists, M2M scope <host>/codelists, GET only, gate = any signed-in BO operator. [mem_bd7f4f863240]
  why: The ePLAN gateway authorizer denies operators without eplan-access (lambda-eplan-api-authorizer/main.go:74), so ePLAN /codelists cannot be the primary source for BO operators.
  where: monorepo-frontend:apps/backoffice/src/api/platform-codelists-api.ts:1
