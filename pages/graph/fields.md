# Graph › Fields

owner: bohdantanasov (most confirmed records) · records: 2 (2 confirmed) · rules guarded by a test: 0 of 0 · last change: 2026-10-01

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## How it works
- API field links close only field-contract changes: of 18 real cross-repo changes after 2026-08-15 one was such (PRO-9773 loan_goal, now both files at rank 1); the rest were services, request factories, routes, enums, proxies. [mem_bf22b6e10135]
  why: Of 18 real cross-repo changes after 2026-08-15 only one (PRO-9773 loan_goal) was a field-contract change; the rest were services, request factories, routes, enums, proxies.
  do: Do not expect API field links to rank the files of a cross-repo change; they close field-contract changes only.
  where: prosight-devdeck:poc/fields.py:38

## Decisions and why
- Required/null/type drift is spec vs backend only. The frontend Yup schema is tied to an operation by voting on field names, not by a call, and Yup required means 'filled on screen', not API presence (values are transformed before submit). Add the frontend only as 'likely', once a form is tied to its API call. [mem_bfb72dd8515a]
  why: The frontend Yup schema is tied to an operation by voting on field names, not by a call, and Yup required means "filled on screen", not API presence (values are transformed before submit).
  do: Keep required/null/type drift spec vs backend; add the frontend only as "likely", once a form is tied to its API call.
  where: prosight-devdeck:poc/fields.py:197
