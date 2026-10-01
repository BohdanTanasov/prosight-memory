# Prosight memory — the record format

The contract of the shared memory (prosight-devdeck `docs/DECISIONS.md` D119). Whatever writes or reads memory —
our `prosight-memory` MCP and hooks today, ITHZ once it writes on macOS and carries `place` and `intent`, DevDeck's
pages — writes and reads exactly this. The repository is the truth; every reader's index is a cache.

## Where records live

```
records/YYYY/MM/<author>.jsonl     one JSON object per line, appended, never edited
```

One file per author and month: two people never append to the same file, so git merges never conflict. A change of
a record (confirmed, rejected, archived) is a new line of `type: "status"`, never an edit of the old line. A record
is replaced by a new record that names it in `supersedes`.

## A record

```json
{
  "schema": "prosight_memory_v1",
  "type": "record",
  "event_id": "mem_3f9a1c2b7d10",
  "kind": "must_not_break",
  "text": "submitRoster must stay idempotent: eMakler rejects a second submission of the same roster.",
  "places": [
    { "repo": "eplan-backend", "path": "apps/proposals/app/Services/ProposalService.php",
      "line": 120, "commit": "97c29d337f904e607a38677460fdd7e4c5506073", "symbol": "ProposalService.submitRoster" }
  ],
  "intent": { "kind": "jira", "key": "PRO-9984" },
  "status": "proposed",
  "supersedes": [],
  "source": "agent:session:9f2c41e0",
  "author": "bohdan",
  "evidence": ["commit:97c29d337f", "https://prosight.atlassian.net/browse/PRO-9984"],
  "git": { "repo": "eplan-backend", "branch": "feature/PRO-9984-beeplan-fail-closed", "commit": "97c29d337f" },
  "tags": [],
  "at": "2026-10-01T10:12:00Z"
}
```

| Field | Required | Meaning |
| --- | --- | --- |
| `schema` | yes | `prosight_memory_v1` |
| `type` | yes | `record` |
| `event_id` | yes | `mem_` + 12 hex; unique; never reused |
| `kind` | yes | `decision` · `must_not_break` · `risk` · `tried` · `owner` · `gate` · `note` |
| `text` | yes | the fact in one or two sentences, ≤ 1000 characters; no secrets, no personal data |
| `places` | no | where in code it holds — each a line **at a commit** (`line` null = the whole file). The graph carries it to today's code (`/v1/where?at=repo:path:line~commit`): same · moved · renamed · changed · gone |
| `intent` | no | what it was for: `{ "kind": "jira", "key": "PRO-9984" }` |
| `status` | yes | `proposed` (an agent, a model run, an import) or `confirmed` (a person wrote it) |
| `supersedes` | no | `event_id`s this record replaces |
| `source` | yes | `agent:session:<id>` · `person:<name>` · `slack:<channel>/<ts>` · `meeting:<id>` · `analysis:<what>` |
| `author` | yes | who appended the line (its file) |
| `evidence` | no | commits, URLs, event ids that support it |
| `git` | no | the checkout it was written from |
| `tags` | no | free words |
| `at` | yes | ISO-8601 UTC |

## A status change

```json
{ "schema": "prosight_memory_v1", "type": "status", "event_id": "mem_8b21…", "target": "mem_3f9a1c2b7d10",
  "status": "confirmed", "by": "person:bohdan", "reason": "still true after PRO-10070", "at": "2026-10-02T08:00:00Z" }
```

`status`: `confirmed` · `rejected` · `archived` · `proposed`. Who may set what:

- an agent writes records as `proposed` only, and never writes a status line;
- a person confirms, rejects or archives;
- the system may confirm on evidence (the intent proven, the check passed) and archives a record whose every place
  is `gone` — with `by: "system:<rule>"` and the evidence in `reason`.

## The effective state of a record (what readers compute, never store)

1. its last status line wins over the record's own `status`;
2. a record named in `supersedes` of a later record that is not rejected is `superseded`;
3. a place the graph says is `changed` makes the record **maybe stale**; every place `gone` makes it **archived** for
   reading (a person can still confirm it back).

Readers show `confirmed` first, then `proposed` marked as unverified; `rejected`, `superseded` and `archived` are
history.

## Mapping to ITHZ

| prosight_memory_v1 | ITHZ (0.1.0a20) |
| --- | --- |
| `event_id` | `event_id` |
| `kind` decision / must_not_break / risk / gate | `kind` decision / must_not_break / risk / gate |
| `kind` tried / owner | `note` with tag `tried` / `owner` (asked to add as kinds) |
| `text` (≤ 1000) | `text` (≤ 1000) |
| `supersedes` | `supersedes` |
| `source` | `source` |
| `git` | `git` |
| `tags` | `tags` |
| `places` | — (asked: a `place` field; today `metadata.places`) |
| `intent` | — (asked: an `intent` field; today `metadata.intent`) |
| `status` + status lines | quarantine / verification state of typed memory |
| this repository (JSONL) | `project.ithz` + its git merge driver (writes need `ithz-native`, Windows-only in 0.1.0a20) |
