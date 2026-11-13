# Hands-on Labs

The exam is scenario-based, and building these makes the answers obvious. Put your code for each lab in `labs/NN-name/`.

Setup: `export ANTHROPIC_API_KEY=...` (never commit it), then `pip install anthropic claude-agent-sdk mcp` or the npm equivalents. Use the latest models (e.g. `claude-sonnet-5-5`, `claude-haiku-5-5`, `claude-opus-5-5`).

## Lab 1 — Raw agent loop (D1, D2)
Build a loop with two tools (`get_weather`, `convert_units`) using the Messages API.
- Branch on `stop_reason`, handle parallel `tool_use` blocks, and cap iterations at 10.
- Make one tool fail with `is_error: true` and watch Claude recover.
- ✅ Done when a multi-step question finishes with `end_turn` and you've logged every `stop_reason`.

## Lab 2 — Orchestrator and subagents (D1, D5)
With the Agent SDK (or your own loop), create an orchestrator that splits a research question into 3 subtasks for Haiku subagents.
- Write a complete delegation brief: objective, scope, what's already covered, and output schema.
- Subagents return ≤ 200-word structured findings, which the orchestrator combines.
- ✅ Compare token usage against a single-agent version.

## Lab 3 — MCP server (D2)
Write a stdio MCP server with one tool (`search_notes`), one resource (`notes://index`), and one prompt (`summarise_notes`).
- `claude mcp add notes -- python server.py`, then use it from Claude Code and run the prompt as a slash command.
- ✅ Rewrite the tool description twice and see how selection changes.

## Lab 4 — Structured extraction (D4)
Extract invoice fields from 5 sample texts.
- Version A: prompt only. Version B: forced tool. Version C: structured outputs.
- Make `po_number` nullable, add semantic validation (line items add up to the total), and add a retry loop that feeds the errors back.
- ✅ Record the parse failure rate for each version.

## Lab 5 — Claude Code configuration (D3)
In a sample repo, add:
- `CLAUDE.md` with build and test commands, plus a `.claude/rules/api.md` scoped with `paths:`
- `.claude/settings.json` with allow and deny rules (deny `Edit(./db/migrations/**)`)
- A `PreToolUse` hook that blocks writes to `.env` (exit 2)
- A Skill in `.claude/skills/release-notes/SKILL.md`
- A subagent in `.claude/agents/test-runner.md`
- A headless run: `claude -p "run tests and summarise" --output-format json --max-turns 5`
- ✅ Each one demonstrably triggers.

## Lab 6 — Prompt caching (D5)
Send a 10k-token document plus question 3 times with `cache_control` on the document.
- Log `cache_creation_input_tokens` and `cache_read_input_tokens`.
- Then add a timestamp at the start of the system prompt and watch the cache break.
- ✅ You can explain every number in `usage`.
