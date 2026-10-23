# Platform salesmen

owner: bohdantanasov (most confirmed records) · records: 2 (1 confirmed · 1 proposed) · rules guarded by a test: 0 of 2 · last change: 2026-10-21

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- PUT /salesmen/{version}/{salesman_id} keeps accepting a legal-person record exactly as GET returns it; after a change in IdentityIntegrationService or AddressIdentityRequestAdapter re-test the BO salesman-settings save. [mem_1a1434376f6c]
  why: salesman-settings saves titles by re-PUTting the whole record; after PRO-9367 (82718800b) the eMakler import lost the address and the PUT answered 422 (PRO-10052).
  guarded: no test yet
  where: platform-salesmen:app/Services/IdentityIntegrationService.php:116, platform-salesmen:app/Services/Adapters/AddressIdentityRequestAdapter.php
  PRO-10052
- Every frontend picker that calls searchSalesmen debounces (the BO supervisor picker does); prefer the exact prosight_id path; fix the candidate path server-side. [mem_1d7d1465aa09] (unconfirmed)
  why: A non-6-digit search streams up to 50k salesmen ids to platform-identities per request: 7 s on test and 16-27 s under repeated calls (backoffice.test 2026-10-21: search=novak 7362/16272/26935 ms; search=122017 198-398 ms), slowing other requests too; an exact 6-digit prosight_id skips that path.
  guarded: no test yet
  where: platform-salesmen:app/Repositories/SalesmanRepository.php:72
