# Backoffice › Konferencie

owner: bohdantanasov (most confirmed records) · records: 1 (1 confirmed) · rules guarded by a test: 0 of 1 · last change: 2026-10-22

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- @tiptap/extension-placeholder, @tiptap/react and @tiptap/starter-kit in backoffice must always be on the same exact version; tiptap packages peer-depend on an exact @tiptap/core/@tiptap/pm version. [mem_a65da0046fce]
  why: Placeholder 3.32.0 pulls @tiptap/extensions 3.32.0 which peers core/pm 3.32.0 exactly, while react/starter-kit 3.31.4 pin 3.31.4: raising one alone installs two @tiptap/core copies and mixed prosemirror-model, breaking the conference description editor.
  do: Raise all three together; after the lockfile update check there is a single prosemirror-model/transform/view entry.
  guarded: not yet
  where: monorepo-frontend:apps/backoffice/package.json:47
