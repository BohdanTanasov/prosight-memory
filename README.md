# prosight-memory

The shared memory of the Prosight team and its agents: what we decided, what must not break, what was risky, what
we tried and why it failed, who knows a part — each bound to a place in code (at a commit) and to the ticket it was
for. The format is [MEMORY-FORMAT.md](MEMORY-FORMAT.md); the decision behind it is prosight-devdeck D119.

- **Data only.** The code that reads and writes it lives in prosight-graph (`poc/memory.py`, `poc/memory_mcp.py`,
  `poc/memory_hook.py`), so a reader can be swapped (ITHZ) without touching the data.
- **Append only.** A change is a new line; nothing is edited or deleted.
- **Agents propose, people confirm.** `python poc/memory.py list --status proposed`, then
  `python poc/memory.py confirm <id>` / `reject <id>` (run from prosight-graph with its venv).
- **Where agents meet it.** MCP `prosight-memory` (`get_context_pack`, `finalize_task`) and three Claude Code hooks:
  the memory of the branch's ticket and changed files at session start, the memory of a file before its first edit,
  one request to propose what was learned at the end. Registered at user scope; silent outside ~/Projects/prosight.
