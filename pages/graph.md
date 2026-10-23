# Graph

owner: bohdantanasov (most confirmed records) · records: 4 (4 confirmed) · rules guarded by a test: 0 of 1 · last change: 2026-10-01

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## How it works
- Rule identifiers the roles answer carries are extractor names, not meanings: `app` = any user of the app, `owner` = the record's own salesman or eplan-admin-routes, `adminPath` = /admin/ routes for eplan-admin-routes only, `seeAll` = any salesman's records reads only, `protectedProcedure` = any signed-in user (not a group), `publicProcedure` = no sign-in, `streamAdminProcedure` = any backoffice group, `noAction` = the proxy maps the write to no action and refuses it. The UI shows them raw today; a reader takes `protectedProcedure` under "what the API allows beyond them" for a restriction. [mem_c6b5bfc31994]
  where: prosight-graph:poc/svc_roles.py:8
- API field links close only field-contract changes: of 18 real cross-repo changes after 2026-08-15 one was such (PRO-9773 loan_goal, now both files at rank 1); the rest were services, request factories, routes, enums, proxies. [mem_bf22b6e10135]
  where: prosight-graph:poc/fields.py:38

## Must not break
- A new value shape in a graph response is opt-in by query param; never change a shape clients already parse. [mem_ce01a66c4645]
  why: Every running DevDeck validates each graph response against its zod contract; strings in drift values broke the installed DevDeck drift tab with contract.response until the service sent them only to clients that ask (compare=all).
  guarded: no test yet
  where: prosight-graph:poc/service.py:90

## Decisions and why
- Keep required/null/type drift spec vs backend; add the frontend only as "likely", once a form is tied to its API call. [mem_bfb72dd8515a]
  why: The frontend Yup schema is tied to an operation by voting on field names, not by a call, and Yup required means "filled on screen", not API presence (values are transformed before submit).
  where: prosight-graph:poc/fields.py:197
