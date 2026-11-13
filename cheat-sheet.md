# CCAR-F Cheat Sheet

**Decision heuristics (most distractors break one of these)**
1. Simplest architecture that works: single call → workflow → single agent → multi-agent.
2. "Must / never / always" → enforce in **code** (hooks, permissions, schemas, tool validation), not prompts.
3. Subagents start with **empty context**, so brief them fully and get condensed results back.
4. Exact facts survive only if kept **verbatim** (case facts block), never through repeated summaries.
5. Static content first, dynamic content last, for **caching**.
6. Distinguish **transient** errors (retry) from **business / permission** errors (explain or escalate).
7. Not latency-sensitive plus bulk → **Batch API**.
8. Tool output is **untrusted** data.

**Agent loop:** `tool_use` → run tools → `tool_result` (same id) → loop. `end_turn` → done. `max_tokens` → truncated. `pause_turn` → send back to continue. Always cap iterations.

**tool_choice:** auto · any · tool(name) · none

**MCP:** Tools (model) · Resources (app) · Prompts (user). stdio / Streamable HTTP. Scopes: local · project (`.mcp.json`) · user.

**Claude Code:** CLAUDE.md (managed > project > user), `@imports`, `.claude/rules` with `paths:`. Settings: managed > CLI > local > project > user; deny wins. Modes: default / acceptEdits / plan / bypassPermissions. Hooks: exit code 2 blocks. Skills: `SKILL.md` needs name and description, loaded on demand. Subagents: `.claude/agents/*.md`. Headless: `claude -p --output-format json --max-turns`.

**Structured output:** structured outputs / forced tool ≫ prefill ≫ "please return JSON". Nullable fields. Validate and retry.

**Prompting:** clear and direct → examples → thinking → XML tags → role → chaining. Long docs on top, query last.

**Caching:** `cache_control: {type: "ephemeral"}`, 5 min (or 1 hr), ≤ 4 breakpoints, tools → system → messages.

**Models:** Haiku = fast and cheap workers or routing · Sonnet = balanced default · Opus = hardest reasoning and orchestrators.
