# Api spec › Opportunities

owner: bohdantanasov (most confirmed records) · records: 1 (1 confirmed) · rules guarded by a test: 0 of 1 · last change: 2026-10-15

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- Do not put a `default` keyword (or create/upsert rules) on fields in OpportunityCrmContactFields: the request, the Opportunity response and the OpportunitySnapshot history all compose it. Write request-only defaults in the createOrUpdateOpportunity requestBody description instead. [mem_5d0136729013]
  why: OpportunityCrmContactFields is composed by the request, the Opportunity response and the OpportunitySnapshot history, so a `default` there would be documented on responses too.
  do: Write request-only defaults in the createOrUpdateOpportunity requestBody description; no default/create/upsert rules on the shared fields.
  guarded: not yet
  where: api-specification:platform/opportunities/model/OpportunityCrmContactFields.yaml:85
  TD-2175
