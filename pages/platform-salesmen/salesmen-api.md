# Platform salesmen › Salesmen api

owner: bohdantanasov (most confirmed records) · records: 2 (1 confirmed · 1 proposed) · rules guarded by a test: 0 of 2 · last change: 2026-10-21

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- PUT /salesmen/{version}/{salesman_id} must keep working for legal-person salesmen: salesman-settings saves titles by re-PUTting the whole record, and after PRO-9367 (commit 82718800b) the address was lost on eMakler import, so the PUT returned 422 (PRO-10052). [mem_1a1434376f6c]
  why: salesman-settings saves titles by re-PUTting the whole record; after PRO-9367 (82718800b) the eMakler import lost the address and the PUT answered 422 (PRO-10052).
  do: PUT /salesmen/{version}/{salesman_id} keeps accepting a legal-person record exactly as GET returns it; after a change in IdentityIntegrationService or AddressIdentityRequestAdapter re-test the BO salesman-settings save.
  guarded: not yet
  where: platform-salesmen:app/Services/IdentityIntegrationService.php:116, platform-salesmen:app/Services/Adapters/AddressIdentityRequestAdapter.php
  PRO-10052
- Any non-6-digit `search` on GET /salesmen goes through the identity-candidate path: it streams up to 50k salesmen ids to platform-identities per request. On test it took 7 s and degraded to 16-27 s under repeated calls, slowing other requests too. An exact 6-digit prosight_id skips that path and answers in about 0.3 s. Every frontend picker that calls searchSalesmen must debounce. [mem_1d7d1465aa09] (proposed)
  why: A non-6-digit search streams up to 50k salesmen ids to platform-identities per request: 7 s on test and 16-27 s under repeated calls (backoffice.test 2026-10-21: search=novak 7362/16272/26935 ms; search=122017 198-398 ms), slowing other requests too; an exact 6-digit prosight_id skips that path.
  do: Every frontend picker that calls searchSalesmen debounces (the BO supervisor picker does); prefer the exact prosight_id path; fix the candidate path server-side.
  guarded: not yet
  where: platform-salesmen:app/Repositories/SalesmanRepository.php:72

## Disputed / open
- Any non-6-digit `search` on GET /salesmen goes through the identity-candidate path: it streams up to 50k salesmen ids to platform-identities per request. On test it took 7 s and degraded to 16-27 s under repeated calls, slowing other requests too. An exact 6-digit prosight_id skips that path and answers in about 0.3 s. Every frontend picker that calls searchSalesmen must debounce. [mem_1d7d1465aa09] (proposed) — nobody confirmed it yet
