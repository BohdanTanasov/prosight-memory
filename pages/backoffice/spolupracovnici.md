# Backoffice › Spolupracovníci

owner: bohdantanasov (most confirmed records) · records: 3 (3 confirmed) · rules guarded by a test: 0 of 0 · last change: 2026-11-12

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## How it works
- The business term «Aktivita 90/360» is the boolean salesmen.activity column (shown Áno/Nie in the BO salesman profile); there is no separate 90-day or 360-day activity number anywhere in platform-salesmen. [mem_3ca154309ffd]
  why: In PRO-10348 a request for «Aktivita 90/360» was first misread as missing data with no source; nothing in the backend names the field that way, only the frontend i18n key activity90360.
  do: When a ticket asks for «Aktivita 90/360», read Salesman::activity; do not look for or invent a 90/360 metric.
  where: platform-salesmen:app/Models/SalesmanBase.php:40
  PRO-10348
- «Dátum typu spolupráce» in product's words is `cooperation_start_date` (BO app label «Začiatok spolupráce», edited as one pair with `cooperation_type`), synced from Emakler; «test accounts» are Prosight IDs 666666–899999, prefix 9 = backoffice, prefix 1 = PFA (SalesmanRepository / salesmen-type.ts). [mem_21ec1357bd3d]
  why: On TD-2229 the deck asked product what «Dátum typu spolupráce» means; product answered it is a term in daily use, readable from BO app Spolupracovníci.
  where: platform-salesmen:app/Models/SalesmanBase.php:36
  TD-2229 · TD-2229#D8

## Decisions and why
- Access to a BackOffice action or section is always a Cognito user group: a new permission = a new `UserGroups` entry + a `SalesmenActions` value + its row in `SALESMEN_ACTION_GROUPS` (UI gate via `canPerform`), the route→action row in `apps/backoffice/app/lib/platform-proxy.ts`, and the same route→group row in platform-cognito-api; the group itself is created in Cognito and people are added there by the access admin. [mem_5071c12b5b1e]
  why: TD-2082/TD-2089 set this as the one way BO app manages access; on TD-2229 product (Maroš Sedláček) had to answer «who may export» as a product question and asked that this kind of question never be asked again.
  do: Never ask product how a BO permission works; ask only who goes into the new group at the start.
  where: monorepo-frontend:apps/backoffice/src/features/auth/auth-groups.ts:57
  TD-2229 · TD-2229#D7
