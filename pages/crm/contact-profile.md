# CRM › Contact profile

owner: bohdantanasov (most confirmed records) · records: 1 (1 confirmed) · rules guarded by a test: 0 of 0 · last change: 2026-10-12

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## How it works
- The contact-history step header renders title and date as sibling flex spans; the explicit {' '} between them keeps the text node 'Obchodný rozhovor 17.10.2026' (screen readers, Playwright getByText). Do not drop them in a refactor. [mem_ede955cbe739]
  why: Title and date are sibling flex spans; without the explicit {" "} the text node reads "Obchodný rozhovor17.10.2026" for screen readers and Playwright getByText.
  do: Keep the {" "} between title and date in contact-profile-history-step.tsx.
  where: monorepo-frontend:packages/crm-timeline/src/features/contact/profile/ui/contact-profile-history-step.tsx:505
  PRO-10055
