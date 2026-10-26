# Platform opportunities › Database

owner: bohdantanasov (most confirmed records) · records: 4 (4 confirmed) · rules guarded by a test: 0 of 4 · last change: 2026-10-24

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- Area immutability on task PATCH is enforced inside TaskRepository::update, and PatchTask must pass `process` through without unsetting it for that check to work. [mem_4a74c7999b6a]
  why: The reviewed fix keeps the immutability check in TaskRepository::update and removes the unset of `process` in OpportunityRequestFactory so the check still has the data it needs.
  do: If refactoring PatchTask or OpportunityRequestFactory, keep `process` intact up to TaskRepository::update so the area-unchanged validation on PATCH keeps working.
  guarded: no test yet
  where: platform-opportunities:app/Http/Requests/CreateTaskRequest.php:110
  PRO-10063
- Adding NOT NULL to an existing column takes two releases: first a migration that sets the default and backfills while the column stays nullable, then NOT NULL in a later release. Write column changes with the Schema builder (`Schema::table(...)->...->change()`, a reversible `->change()` in down()), not raw ALTER statements. [mem_701632789b8c]
  why: `.docker/init-app.sh` runs `php artisan migrate` during a rolling deploy, so the old code still running can write null and break a NOT NULL added in the same release; on platform-opportunities#124 a one-step raw-ALTER migration with a full null backfill was split this way (NOT NULL moved to release D25).
  do: Split any NOT NULL tightening into a default/backfill migration now and the constraint in the next release; use Schema::table()->change(), not DB::statement ALTER.
  guarded: no test yet
  PRO-10180
- A Task's stored `process`/area column can remain null even for rows created after the area-backfill logic was added; the API only looks correct because TaskResource now reads through Task::area(), which falls back to the raising step's process and then to financial mediation. [mem_f46e06882e09]
  why: Review remark caught that TaskResource served the raw null `$this->process?->value`; the fix (commit 3fb4824) made TaskResource.php:38 call `$this->task()->area()->value` instead, while the underlying column is still left null on such rows.
  do: Always read a task's area through Task::area() (or TaskResource), never by querying the stored process/area column directly, since that column can be null regardless of what the API reports.
  guarded: no test yet
  PRO-10063
- The `task` table is large enough that a full-table backfill `UPDATE` wrapped in Laravel's default per-migration transaction is a real risk (long lock / big transaction) and must instead run chunked with the transaction disabled. [mem_e761155f50ed]
  why: Reviewer flagged that two `UPDATE task` statements over the whole table ran inside the default migration transaction; fix added `public $withinTransaction = false;` and `chunkById(self::CHUNK, …)` with 500 ids.
  do: For any future migration that updates the `task` table (or similarly large tables) in bulk, disable the default migration transaction and chunk the update by id.
  guarded: no test yet
  where: platform-opportunities:database/migrations/2026_10_20_100000_backfill_task_process.php:33
  PRO-10063
