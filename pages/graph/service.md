# Graph › Service

owner: bohdantanasov (most confirmed records) · records: 1 (1 confirmed) · rules guarded by a test: 0 of 1 · last change: 2026-10-01

> Compiled by DevDeck from the team memory’s records — do not edit by hand: a change here is a record.

## Must not break
- The graph service is live for every running DevDeck, which validates each response against its zod contract. A new value shape in a response (here strings in drift values) broke the installed DevDeck's drift tab with contract.response until the service sent it only to clients that ask (compare=all). New shapes must be opt-in by query param. [mem_ce01a66c4645]
  why: Every running DevDeck validates each graph response against its zod contract; strings in drift values broke the installed DevDeck drift tab with contract.response until the service sent them only to clients that ask (compare=all).
  do: A new value shape in a graph response is opt-in by query param; never change a shape clients already parse.
  guarded: not yet
  where: prosight-devdeck:poc/service.py:90
