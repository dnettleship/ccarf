# Study Plan

Default plan: **4 weeks at 60–90 minutes a day**. Use the 2-week plan if you already ship Claude agents in production, and the 8-week plan if the Agent SDK, MCP and Claude Code are new to you.

## Week 0 — Baseline (day 1)
- [ ] Answer every question in [practice/questions.md](practice/questions.md) without notes and record the score per domain in [progress.md](progress.md).
- [ ] Set up an API key, the Python or TypeScript SDK, and Claude Code.

## Week 1 — D1 Agentic Architecture (27%)
- [ ] Read [domains/d1-agentic-architecture.md](domains/d1-agentic-architecture.md)
- [ ] Read Anthropic's "Building effective agents" (see [resources.md](resources.md))
- [ ] Lab 1: hand-written agent loop driven by `stop_reason`
- [ ] Lab 2: orchestrator plus two subagents with explicit handoff
- [ ] Flashcards for D1 every day

## Week 2 — D2 Tools & MCP (18%) + D4 Prompting & Structured Output (20%)
- [ ] Read the D2 and D4 notes
- [ ] Lab 3: build a small MCP server (one tool, one resource) and connect it to Claude Code
- [ ] Lab 4: structured extraction with a forced tool call, schema validation and a retry loop
- [ ] Rewrite three weak tool descriptions from your own projects

## Week 3 — D3 Claude Code (20%) + D5 Context & Reliability (15%)
- [ ] Read the D3 and D5 notes
- [ ] Lab 5: project `CLAUDE.md`, a path-scoped rule, a Skill, a hook, and a headless `claude -p` run in CI
- [ ] Lab 6: prompt caching, then measure `cache_read_input_tokens`
- [ ] Practise designing a "case facts" block and a human-handoff summary

## Week 4 — Consolidate and mock
- [ ] Re-take the practice questions and compare with week 0
- [ ] Take at least one full timed mock (60 questions in 120 minutes, about 2 minutes each)
- [ ] Re-read the notes for any domain scoring under 80%
- [ ] Night before: [cheat-sheet.md](cheat-sheet.md) only

## Variants
- **2 weeks:** Days 1–4 D1, days 5–7 D2 and D4, days 8–10 D3 and D5, days 11–14 mocks and review.
- **8 weeks:** Spend two weeks per week above and do every lab twice, the second time in the other SDK language.

## Exam-day technique
- Read the **scenario constraints** first: latency, cost, compliance, human review, and scale. Most distractors are "technically works" answers that break one of them.
- Prefer the **simplest architecture that meets the requirement**, such as a single agent or a workflow before multi-agent.
- Prefer **deterministic enforcement** (hooks, schemas, permissions, code) over "add an instruction to the prompt" when the scenario says something *must* happen.
- For multiple-response items, select exactly the number of answers the question asks for.
