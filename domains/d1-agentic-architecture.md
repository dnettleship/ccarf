# D1 — Agentic Architecture & Orchestration (27%)

The largest domain. It tests whether you can choose the **right shape** for a problem and run the **agent loop** correctly.

## 1. Workflows vs agents
From Anthropic's *Building effective agents*:
- **Workflow:** LLM calls orchestrated through **predefined code paths**. It is predictable, cheaper, and easier to test.
- **Agent:** the LLM **dynamically directs** its own process and tool use. Use it when the steps can't be known in advance.
- Rule of thumb: start with the simplest thing (a single call plus retrieval and examples), then a workflow, and use an agent only when you need its flexibility. Agents trade latency and cost for capability.

### Workflow patterns
| Pattern | What it is | Use when |
|---|---|---|
| Prompt chaining | Output of step N feeds step N+1, optionally with programmatic "gates" | The task splits cleanly into fixed subtasks |
| Routing | Classify input, then send it to a specialised prompt or model | Inputs fall into distinct categories, e.g. sending easy queries to Haiku |
| Parallelisation | **Sectioning** (independent subtasks in parallel) or **voting** (same task several times) | Speed, or more confidence through diverse attempts |
| Orchestrator-workers | A central LLM breaks the task down at runtime and delegates to workers | Subtasks aren't known ahead of time, e.g. multi-file code changes |
| Evaluator-optimiser | One LLM generates and another critiques in a loop | Clear evaluation criteria exist and iteration measurably helps |

## 2. The agent loop
```
messages = [user_msg]
while True:
    resp = client.messages.create(model, tools, messages, ...)
    messages.append({"role": "assistant", "content": resp.content})
    if resp.stop_reason == "tool_use":
        results = [run_tool(b) for b in resp.content if b.type == "tool_use"]
        messages.append({"role": "user", "content": results})  # tool_result blocks
        continue
    break  # end_turn, max_tokens, stop_sequence, refusal, pause_turn ...
```
Key facts:
- **`stop_reason` values:** `end_turn` (finished naturally), `tool_use` (wants a tool run), `max_tokens` (cut off, so raise the limit or continue), `stop_sequence`, `pause_turn` (a server-tool long turn was paused, so send the response back to continue), and `refusal`.
- Every `tool_use` block needs a matching `tool_result` with the same `tool_use_id`, in the **next user message**. Put the tool_result blocks **first** in that message's content.
- Claude can return **several tool_use blocks in one turn** (parallel tool use). Run them all and return all their results together.
- Always add a **guard**: a maximum number of iterations, a token or cost budget, or a timeout. Unbounded loops are a classic distractor.
- Don't decide the loop is finished by parsing the assistant's text. Use `stop_reason`.

## 3. Single-agent vs multi-agent
- **Single agent** with good tools is the default. It is easier to debug and keeps all context in one place.
- **Hub-and-spoke (orchestrator plus subagents)** helps when work is **parallelisable and breadth-first** (for example research across many sources), when subtasks need **separate context windows** to avoid pollution, or when they need different tools or permissions.
- Costs: many times the tokens, coordination overhead, and harder debugging. Anthropic's multi-agent research system reported large quality gains on breadth-heavy research but also much higher token use.
- **Subagents do not inherit the parent's conversation.** Everything a subagent needs (objective, constraints, output format, relevant facts, and the boundaries of what *not* to do) must go in its prompt or task description.
- Subagents should return **condensed, structured results** rather than raw transcripts, so the orchestrator's context stays small.
- Avoid having several agents write to the same shared state at once unless the writes are coordinated. Prefer parallel reads and serialised writes.

## 4. Claude Agent SDK
- The same harness that powers Claude Code, available as a library (Python `claude-agent-sdk`, TypeScript `@anthropic-ai/claude-agent-sdk`).
- It provides the agent loop, built-in tools (file read/edit, bash, search), context compaction, permission modes, **hooks** (`PreToolUse`, `PostToolUse`, `UserPromptSubmit`, `Stop`, `SubagentStop`, `PreCompact`, and others), **subagents** (defined programmatically or as `.claude/agents/*.md`), MCP servers, and session resume.
- Choose the **SDK** when you want an autonomous agent without writing the loop and tool plumbing yourself. Choose the **raw Messages API** when you need full control or a simple, single-shot integration.

## 5. Human-in-the-loop and escalation
- Put approval gates **before irreversible or high-impact actions** such as payments, deletes, external messages and production deploys.
- Enforce gates **deterministically** with permission modes, `PreToolUse` hooks or a tool that requires approval, rather than by asking the model nicely.
- Escalate when: confidence is low, a policy or limit threshold is exceeded, the user explicitly asks for a human, or the same failure has repeated N times.
- The **handoff** should be a structured summary: the goal, what was done, current state, evidence, the open question, and the recommended next action. The human shouldn't need to read the raw transcript.

## 6. Common traps
- Choosing multi-agent for a sequential, tightly coupled task. Use a single agent or a chain instead.
- Assuming a subagent "knows" what the orchestrator saw.
- Treating `max_tokens` as success.
- Leaving a loop with no iteration or cost cap.
- Using a prompt instruction where a hook or permission is required ("must never…").
