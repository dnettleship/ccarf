# Practice Set 1 — Agentic Architecture & Orchestration (D1, 27%)

15 original scenario-style questions (not real exam items). Give yourself **30 minutes**. Answers are in collapsed sections. Notes: [../domains/d1-agentic-architecture.md](../domains/d1-agentic-architecture.md)

---

**1.** A finance team processes supplier invoices in the same steps every time: extract the fields, check them against the purchase order, then send them to the right approver by amount. Which design fits best?
- A. A multi-agent system with one subagent per step
- B. A prompt chain (workflow) with programmatic checks between the steps
- C. An autonomous agent with access to every finance tool
- D. An evaluator–optimiser loop

<details><summary>Answer</summary>

**B.** The steps are fixed and known in advance, so a workflow is cheaper, more predictable and easier to test. Agents and multi-agent setups add cost and unpredictability without any benefit here.
</details>

**2.** A support inbox receives billing, technical and legal requests. Each type needs a different system prompt and toolset, and cost matters. Which pattern fits?
- A. Orchestrator–workers
- B. Evaluator–optimiser
- C. Routing: a cheap classifier (e.g. Haiku) sends each request to a specialised handler
- D. Parallel voting

<details><summary>Answer</summary>

**C.** The inputs fall into distinct categories, which is exactly what routing is for. A small model is usually enough for the classification step.
</details>

**3.** A moderation system must be more confident on borderline content before it auto-removes a post. Which pattern helps most?
- A. Parallelisation (voting): run several independent judgements and act on agreement or a threshold
- B. Prompt chaining
- C. Routing
- D. A single call with a higher `max_tokens`

<details><summary>Answer</summary>

**A.** Voting runs the same task several times to gain confidence, and the removal threshold can be tuned.
</details>

**4.** Marketing copy must be translated to a strict brand style guide. Reviewers can clearly say what's wrong, and two rounds of revision noticeably improve the result. Which pattern fits?
- A. Routing
- B. Sectioning
- C. Orchestrator–workers
- D. Evaluator–optimiser

<details><summary>Answer</summary>

**D.** Clear evaluation criteria, plus revisions that measurably help, is the textbook case for evaluator–optimiser.
</details>

**5.** A response contains two `tool_use` blocks (`get_weather` and `get_calendar`). What should your loop do?
- A. Run only the first tool and let Claude ask for the second one later
- B. Run both and return both `tool_result` blocks, each with its matching `tool_use_id`, in a single user message
- C. Send each `tool_result` in its own user message
- D. Re-prompt Claude to request one tool at a time

<details><summary>Answer</summary>

**B.** Claude can request tools in parallel. All the results go back together in the next user message, with the `tool_result` blocks first.
</details>

**6.** The API rejects your request with an error saying some `tool_use` IDs have no matching `tool_result`. What is the most likely cause?
- A. The previous assistant turn requested a tool whose result your code didn't include in the next user message
- B. The tool's `input_schema` is invalid
- C. `max_tokens` is too low
- D. The system prompt is too long

<details><summary>Answer</summary>

**A.** Every `tool_use` block needs a corresponding `tool_result` in the user message that immediately follows it.
</details>

**7.** An agent sometimes stops halfway through a task. The loop ends whenever the assistant's text contains the word "done". What is the right fix?
- A. Ask Claude never to say "done"
- B. Raise the temperature
- C. Decide whether to continue from `stop_reason`: keep going on `tool_use`, finish on `end_turn`, and handle the other values explicitly
- D. Add more tools

<details><summary>Answer</summary>

**C.** Use the structured `stop_reason` field, not text parsing, to control the loop.
</details>

**8.** You're using Anthropic's server-side web search tool, and a long research turn returns `stop_reason: "pause_turn"`. What should you do?
- A. Treat it as an error and retry from scratch
- B. Show the partial text to the user as the final answer
- C. Switch to a client-side search tool
- D. Send the assistant's response back as-is in a follow-up request so Claude can continue

<details><summary>Answer</summary>

**D.** `pause_turn` means a long-running server-tool turn was paused. Pass the content back and Claude continues where it left off.
</details>

**9.** An orchestrator delegates with the instruction "Research competitor pricing as we discussed." The subagent returns generic, irrelevant information. What is the best fix?
- A. Rewrite the delegation as a complete brief: which competitors, which products and regions, the time window, sources to prefer, and the output format
- B. Use a bigger model for the subagent
- C. Give the subagent more tools
- D. Lower the temperature

<details><summary>Answer</summary>

**A.** Subagents don't inherit the orchestrator's conversation. "As we discussed" refers to context the subagent never saw.
</details>

**10.** A multi-agent research system costs far too much. Most user questions are simple fact lookups, but the orchestrator always starts 5–10 subagents. What should change?
- A. Remove the orchestrator
- B. Tell the orchestrator to scale effort to query complexity: answer simple lookups directly or with one agent, and fan out only for broad research
- C. Switch all agents to Opus
- D. Disable parallel tool calls

<details><summary>Answer</summary>

**B.** Multi-agent setups use many times more tokens. Reserve fan-out for breadth-heavy questions.
</details>

**11.** When is an orchestrator-plus-subagents design most justified? (Choose 2)
- A. Breadth-first work that can run in parallel, such as surveying many independent sources
- B. A tightly coupled edit to a single file where each step depends on the previous one
- C. Subtasks that need isolated contexts or different tool permissions
- D. When minimising token cost is the top priority

<details><summary>Answer</summary>

**A and C.** B suits a single agent. D argues *against* multi-agent.
</details>

**12.** A customer-service agent needs escalation rules. Which set of triggers is best?
- A. Escalate any time the customer uses a negative word
- B. Never escalate; the agent should always solve the issue
- C. Escalate when the customer explicitly asks for a human, when an action exceeds a policy limit, or after repeated failures or low confidence
- D. Escalate at random to sample quality

<details><summary>Answer</summary>

**C.** These are clear, policy-based triggers. A floods the human queue, and B is unsafe.
</details>

**13.** Human agents complain that escalated cases take too long because they have to read the full transcript. What should the agent hand over?
- A. The full raw transcript, highlighted
- B. A one-line summary
- C. Only the customer's last message
- D. A structured handoff: case facts (IDs, amounts), the goal, actions taken and their results, the open question, and a recommended next step

<details><summary>Answer</summary>

**D.** A human should be able to act on the handoff without reading the transcript.
</details>

**14.** A team wants an autonomous coding agent that edits files, runs shell commands, compacts its context and supports hooks. They don't want to write the loop or the tool plumbing. What should they use?
- A. The Claude Agent SDK
- B. The raw Messages API with a hand-written loop
- C. The Message Batches API
- D. A single prompt with extended thinking

<details><summary>Answer</summary>

**A.** The Agent SDK provides the Claude Code harness as a library: the loop, built-in tools, compaction, hooks, subagents and permissions.
</details>

**15.** Two subagents running in parallel both edit `config.yaml`, and their changes overwrite each other. What is the best fix?
- A. Ask them to be careful
- B. Let them read in parallel, but serialise writes through one owner (the orchestrator or a single writer agent)
- C. Add a retry loop
- D. Raise the context window

<details><summary>Answer</summary>

**B.** Avoid uncoordinated concurrent writes to shared state.
</details>

---

<details><summary>Quick answer key</summary>

1 B · 2 C · 3 A · 4 D · 5 B · 6 A · 7 C · 8 D · 9 A · 10 B · 11 A,C · 12 C · 13 D · 14 A · 15 B
</details>
