# Devdeck › Envs

owner: bohdantanasov (most confirmed records) · records: 1 (1 confirmed) · rules guarded by a test: 0 of 0 · last change: 2026-10-02

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Decisions and why
- DevDeck will not replace platform.dev.sh / eplan.dev.sh with its own stack description: the scripts are the team's (Windows included) and remain the launcher; DevDeck orchestrates compose and `exec` over them and adds readiness facts, proposals and the Start-work mount steps (plan docs/plans/local-env.md, Bohdan's track). [mem_565070df1d66]
  why: platform.dev.sh / eplan.dev.sh are the team launcher (Windows included), so a DevDeck-only stack description would fork the team setup.
  do: DevDeck orchestrates compose and `exec` over the scripts and adds readiness facts, proposals and the Start-work mount steps (docs/plans/local-env.md §13); it does not describe the stack itself.
  where: prosight-devdeck:docker-dev-env/platform.dev.sh:1
