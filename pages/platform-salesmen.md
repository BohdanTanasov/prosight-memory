# Platform salesmen

owner: bohdantanasov (most confirmed records) · records: 3 (3 confirmed) · rules guarded by a test: 0 of 3 · last change: 2026-11-20

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- PUT /salesmen/{version}/{salesman_id} keeps accepting a legal-person record exactly as GET returns it; after a change in IdentityIntegrationService or AddressIdentityRequestAdapter re-test the BO salesman-settings save. [mem_1a1434376f6c]
  why: salesman-settings saves titles by re-PUTting the whole record; after PRO-9367 (82718800b) the eMakler import lost the address and the PUT answered 422 (PRO-10052).
  guarded: no test yet
  where: platform-salesmen:app/Services/IdentityIntegrationService.php:116, platform-salesmen:app/Services/Adapters/AddressIdentityRequestAdapter.php
  PRO-10052
- SalesmanApiTestCase partially fakes the Bus (SynchronizeSalesmanToEmakler, UpdateClientsHierarchicalPath), and a partial BusFake still fakes every Bus::batch: in any feature test extending it a batch is only recorded, its jobs and then/catch never run. [mem_864219f0b3bb]
  why: PRO-10348: 25 salesmen export tests failed in pipeline 996 because the export's batch of parts never ran; it cost a long debug since the batch looked dispatched (pending_jobs 1).
  do: A test of code that dispatches a batch swaps in Tests\Feature\Support\BatchRunningBusFake::swapFor(Bus::getFacadeRoot()) in setUp, as SalesmenExportApiTest does.
  guarded: no test yet
  where: platform-salesmen:tests/Feature/Support/SalesmanApiTestCase.php:45
  PRO-10348
- Every frontend picker that calls searchSalesmen debounces (the BO supervisor picker does); prefer the exact prosight_id path; fix the candidate path server-side. [mem_1d7d1465aa09]
  why: A non-6-digit search streams up to 50k salesmen ids to platform-identities per request: 7 s on test and 16-27 s under repeated calls (backoffice.test 2026-10-21: search=novak 7362/16272/26935 ms; search=122017 198-398 ms), slowing other requests too; an exact 6-digit prosight_id skips that path.
  guarded: no test yet
  where: platform-salesmen:app/Repositories/SalesmanRepository.php:72
