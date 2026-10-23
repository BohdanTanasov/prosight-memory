# Platform documents › Queues

owner: bohdantanasov (most confirmed records) · records: 1 (1 confirmed) · rules guarded by a test: 0 of 1 · last change: 2026-10-01

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- TransferS3Metadata names queue key queue.queues.transfer_metadata_jobs, which config/queue.php does not define: the job silently runs on the connection's default queue (data-jobs). [mem_62a4e2f931a4]
  why: config/queue.php defines only queues.data_jobs, so the job silently runs on the connection default queue (data-jobs). Verified in code 2026-10-01; still so on 2026-10-22 (TransferS3Metadata.php:41 reads the undefined key).
  do: Define queue.queues.transfer_metadata_jobs in config/queue.php (and a worker for it) or make the job read the queue it really uses; do not add jobs that read an undefined key.
  guarded: not yet
  where: prosight-devdeck:platform-documents/app/Jobs/TransferS3Metadata.php:41
