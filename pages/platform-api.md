# Platform API

owner: bohdantanasov (most confirmed records) · records: 1 (1 confirmed) · rules guarded by a test: 0 of 1 · last change: 2026-10-28

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- Tests that use the forwardThrough helper swap in the proxy by reflection, so they never exercise a controller's own getPlatformService override; any controller tested only that way can have a broken override without any test failing. [mem_8ba29bc10eb4]
  why: Before the remark every test in SalesmenExportControllerTest used forwardThrough, which bypassed app/Http/Controllers/SalesmenExportController.php:24's getPlatformService override entirely.
  do: When a controller overrides getPlatformService (or similar factory methods), add a test that constructs the controller directly (no forwardThrough) and asserts the resulting service/proxy and its base_uri, not just one relying on forwardThrough.
  guarded: no test yet
  where: platform-api:tests/Feature/SalesmenExportControllerTest.php:106
  PRO-10348
