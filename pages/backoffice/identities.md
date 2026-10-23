# Backoffice › Identities

owner: bohdantanasov (most confirmed records) · records: 1 (1 confirmed) · rules guarded by a test: 0 of 1 · last change: 2026-10-02

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- Identity detail salesman panel and create-success verify still call ePLAN GET /salesmen/{id} with the operator token, so they fail for operators without eplan-access. Moving the identity panel to platform-salesmen needs a decision: the BFF opens GET /salesmen/1.0/{id} only to backoffice-salesmen* operators. [mem_0d6d9d6e4490]
  why: Operators without eplan-access are denied by the ePLAN gateway, and the BFF opens GET /salesmen/1.0/{id} only to backoffice-salesmen* operators, so moving the panel to platform-salesmen needs a decision on the gate.
  do: Before touching the identity detail salesman panel or create-success verify, decide the gate for platform-salesmen reads; until then both fail for operators without eplan-access.
  guarded: not yet
  where: prosight-devdeck:apps/backoffice/src/features/identities/containers/identity-detail-page.tsx:55
