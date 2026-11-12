# Backoffice

owner: bohdantanasov (most confirmed records) · records: 3 (3 confirmed) · rules guarded by a test: 0 of 2 · last change: 2026-10-22

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- @tiptap/extension-placeholder, @tiptap/react and @tiptap/starter-kit in backoffice must always be on the same exact version; tiptap packages peer-depend on an exact @tiptap/core/@tiptap/pm version. [mem_a65da0046fce]
  why: Placeholder 3.32.0 pulls @tiptap/extensions 3.32.0 which peers core/pm 3.32.0 exactly, while react/starter-kit 3.31.4 pin 3.31.4: raising one alone installs two @tiptap/core copies and mixed prosemirror-model, breaking the conference description editor.
  do: Raise all three together; after the lockfile update check there is a single prosemirror-model/transform/view entry.
  guarded: no test yet
  where: monorepo-frontend:apps/backoffice/package.json:47
- Before touching the identity detail salesman panel or create-success verify, decide the gate for platform-salesmen reads; until then both fail for operators without eplan-access. [mem_0d6d9d6e4490]
  why: Operators without eplan-access are denied by the ePLAN gateway, and the BFF opens GET /salesmen/1.0/{id} only to backoffice-salesmen* operators, so moving the panel to platform-salesmen needs a decision on the gate.
  guarded: no test yet
  where: monorepo-frontend:apps/backoffice/src/features/identities/containers/identity-detail-page.tsx:55

## Decisions and why
- Read codelists platform → ePLAN (operator token) → local fallback: BFF /api/platform/codelists, M2M scope <host>/codelists, GET only, gate = any signed-in BO operator. [mem_bd7f4f863240]
  why: The ePLAN gateway authorizer denies operators without eplan-access (lambda-eplan-api-authorizer/main.go:74), so ePLAN /codelists cannot be the primary source for BO operators.
  where: monorepo-frontend:apps/backoffice/src/api/platform-codelists-api.ts:1
