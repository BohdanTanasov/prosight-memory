# Backoffice › Codelists

owner: bohdantanasov (most confirmed records) · records: 1 (1 confirmed) · rules guarded by a test: 0 of 0 · last change: 2026-10-02

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Decisions and why
- Back Office reads countries/genders/academic titles/municipalities from platform-codelists via its own BFF (/api/platform/codelists, M2M scope <host>/codelists, GET only, gate = any signed-in BO operator). ePLAN /codelists is only a fallback because the ePLAN gateway authorizer denies operators without eplan-access. Order: platform -> ePLAN (operator token) -> local fallback. [mem_bd7f4f863240]
  why: The ePLAN gateway authorizer denies operators without eplan-access (lambda-eplan-api-authorizer/main.go:74), so ePLAN /codelists cannot be the primary source for BO operators.
  do: Read codelists platform → ePLAN (operator token) → local fallback: BFF /api/platform/codelists, M2M scope <host>/codelists, GET only, gate = any signed-in BO operator.
  where: prosight-devdeck:apps/backoffice/src/api/platform-codelists-api.ts:1
