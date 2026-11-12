# Infrastructure

owner: bohdantanasov (most confirmed records) · records: 1 (1 confirmed) · rules guarded by a test: 0 of 1 · last change: 2026-11-12

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- The eic-cache read scope must only accept requests on GET /integrations/eic/cache/funds, not a wildcard proxy path; widening it back to {proxy+} would let that scope authorize any route under the integration. [mem_5fa0c2ccdf87]
  why: Commit 59362ef swapped `{proxy+}` for `/funds` in docs/platform-integrations-stack.md:104, following a reviewer remark judged right.
  do: When changing the eic-cache scope or route pattern, also check the authorizer in the monorepo-integrations repo's eic/template.yaml, since it lives outside this repository and enforces the same restriction.
  guarded: no test yet
  where: infrastructure:docs/platform-integrations-stack.md:104
  PRO-10536
