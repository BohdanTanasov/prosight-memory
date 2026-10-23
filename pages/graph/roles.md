# Graph › Roles

owner: bohdantanasov (most confirmed records) · records: 1 (1 confirmed) · rules guarded by a test: 0 of 0 · last change: 2026-10-01

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## How it works
- Rule identifiers the roles answer carries are extractor names, not meanings: `app` = any user of the app, `owner` = the record's own salesman or eplan-admin-routes, `adminPath` = /admin/ routes for eplan-admin-routes only, `seeAll` = any salesman's records reads only, `protectedProcedure` = any signed-in user (not a group), `publicProcedure` = no sign-in, `streamAdminProcedure` = any backoffice group, `noAction` = the proxy maps the write to no action and refuses it. The UI shows them raw today; a reader takes `protectedProcedure` under "what the API allows beyond them" for a restriction. [mem_c6b5bfc31994]
  why: The identifiers are extractor names: app = any user of the app; owner = the record own salesman or eplan-admin-routes; adminPath = /admin/ routes (eplan-admin-routes only); seeAll = reads of any salesman records; protectedProcedure = any signed-in user (not a group); publicProcedure = no sign-in; streamAdminProcedure = any backoffice group; noAction = the proxy maps the write to no action and refuses it. Shown raw, a reader takes protectedProcedure for a restriction.
  do: Render the meaning, not the extractor name, before a roles answer reaches a person.
  where: prosight-devdeck:prosight-graph/poc/svc_roles.py:8
