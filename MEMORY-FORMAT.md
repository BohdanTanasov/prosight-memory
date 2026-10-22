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

## Fields added 2026-10-22

Additive only: every field above keeps its name and meaning, and a reader that does not know these fields loses
nothing. All are optional; a line without them (every line written before) is as valid as before, and a reader
treats a missing field, or one of the wrong type, as absent (`null` / `[]`) rather than dropping the line. Written
by DevDeck since 2026-10-22 (prosight-devdeck `docs/DECISIONS.md` D419–D423).

| Field | Required | Meaning |
| --- | --- | --- |
| `why` | no | the reason, string or null: the incident, the ticket, what broke. DevDeck's intake requires it for `decision`, `must_not_break`, `risk` and `tried` |
| `do` | no | what to do about it, string or null. `text` stays the "what", so a reader of `text` alone still reads the fact |
| `topic` | no | the subject it files under, string or null: `<app>/<area-slug>` from the graph's areas of its place (`crm/produkty`), else `<repo>/<first two path segments>` (`monorepo-frontend/apps-crm`) |
| `context` | no | where it was written: `{ "session", "task", "repo", "branch", "commit", "worktree" }`, each a string or null. The task's title lives in `context.task`, no longer as a `task:<title>` evidence item |
| `relations` | no | `[{ "to": "mem_…", "how": "same" \| "refines" \| "contradicts" }]` — the neighbours found when it was proposed. A `contradicts` relation is never settled by the later timestamp: a person decides, and only an explicit `supersedes` replaces a record |

**`source` of an agent.** Still a string: `agent:session:<id>` when the session that wrote it is known (its id is
also in `context.session`), else `agent:mcp`; a record taken from an approved technical description is
`analysis:spec:<KEY>`.

**Typed evidence.** Every `evidence` item DevDeck writes since this date is a typed id matching
`^(jira|td|commit|pr|test|incident|sentry|session|slack|confluence|figma|file):\S` — never a URL or free text, so a
link does not rot when a URL changes (readers resolve the id to a URL). Older lines may still hold free text, URLs
or `task:<title>`; a reader shows such an item as text.

| Prefix | Format | Example |
| --- | --- | --- |
| `jira:` | `jira:<KEY>` | `jira:PRO-10005` |
| `td:` | `td:<epic>[#D<n>\|#<story>]` | `td:TD-2146#D7` |
| `commit:` | `commit:<repo>@<sha>` | `commit:monorepo-frontend@2f96e520d` |
| `pr:` | `pr:<repo>#<n>` | `pr:monorepo-frontend#5572` |
| `test:` | `test:<repo>:<path>[:<line>]` | `test:automation-tests:tests/crm/x.spec.ts:227` |
| `file:` | `file:<repo>:<path>[:<line>]` | `file:monorepo-frontend:apps/crm/src/a.ts:12` |
| `incident:` | `incident:<id>` | `incident:2026-10-03-fs-board` |
| `sentry:` | `sentry:<issue>` | `sentry:4567` |
| `session:` | `session:<id>` | `session:9f2c41e0` |
| `slack:` | `slack:<permalink>` | `slack:https://prosight.slack.com/archives/C1/p1` |
| `confluence:` | `confluence:<page id>` | `confluence:123456` |
| `figma:` | `figma:<url>` | `figma:https://www.figma.com/design/…` |

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

## How DevDeck writes since 2026-10-22 (stage 2)

Additive: the format above does not change, and every line written before stays valid and is read as before. What
changes is what DevDeck writes (prosight-devdeck `docs/DECISIONS.md` D424–D426):

- **A record is written `confirmed`.** An agent's candidate waits in DevDeck as a proposal; nothing reaches this
  repository until a person saves it, so the line a person approved carries `"status": "confirmed"`. A candidate a
  person turns down ("not needed", with its reason) is never written; the reason stays in DevDeck. Lines written
  `proposed` before stay valid and read as not checked yet.
- **After the merge.** A record whose place names a commit that is not yet on its repository's mainline is written
  once that commit is there; it carries the branch in `context.branch`. If the PR is declined, the branch is gone,
  or nothing merges in time, it is never written.
- **Removal instead of a status line.** DevDeck no longer writes `rejected`, `archived` or `superseded`. A record that
  no longer holds is taken out: its record line and every status line on it are deleted — in whichever author's file
  they are — in one commit. A new record that names an old one in `supersedes` deletes the old one's lines in the
  same commit as its own line. Readers still read the old status lines as described above.
- **The commit body is the history.** The commit that adds or deletes a record says who approved it or removed it,
  why, and what replaced it, e.g.

  ```text
  remove mem_3f9a1c2b7d10

  Removed by person:bohdan.
  Why: the board no longer shares this service since PRO-10070
  Replaced by: mem_8b21c4d5e6f7
  Records:
  - mem_3f9a1c2b7d10 (must_not_break, bohdan): The products kanban and the FS board share this service…
  ```

  A record's history is `git log -S<id> -- records`. Reverting a removal commit brings its lines back.

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
