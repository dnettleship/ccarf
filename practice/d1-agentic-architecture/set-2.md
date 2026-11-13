# D1 Practice Set 2 — Agent Loop Mechanics & the Agent SDK (27%)

15 original scenario-style questions (not real exam items). Give yourself **30 minutes**. Notes: [../../domains/d1-agentic-architecture.md](../../domains/d1-agentic-architecture.md)

---

**1.** An agent reaches its 25-iteration cap before finishing the task. What should the loop do?
- A. Silently raise the cap and keep going
- B. Return the last assistant text as if it were the final answer
- C. Stop, report a clear "incomplete" status with progress so far, and escalate or ask the user how to proceed
- D. Delete the history and start again

<details><summary>Answer</summary>

**C.** Caps exist to bound cost and runaway behaviour. When one is hit, report it openly rather than hiding it.
</details>

**2.** When you send tool results back, how should the next user message be structured?
- A. The `tool_result` blocks come first in the content, followed by any optional text
- B. Text first, then the `tool_result` blocks
- C. Each `tool_result` in a separate assistant message
- D. Put the results in the system prompt

<details><summary>Answer</summary>

**A.** `tool_result` blocks go in the user message straight after the `tool_use`, at the start of its content.
</details>

**3.** After each response, a developer appends only the assistant's *text* to `messages`, and the next request fails. Why?
- A. Text blocks aren't allowed in history
- B. The `tool_use` blocks were dropped, so the following `tool_result` blocks reference IDs that no longer exist
- C. The system prompt is missing
- D. The model ID changed

<details><summary>Answer</summary>

**B.** Append the full assistant `content`, including `tool_use` blocks (and thinking blocks when extended thinking is on).
</details>

**4.** A response comes back with `stop_reason: "refusal"`. What is the right handling?
- A. Retry the identical request in a loop until it succeeds
- B. Treat it as a tool request
- C. Raise `max_tokens`
- D. Stop the loop, handle the refusal gracefully (inform the user, log it, or route for review), and don't blindly retry the same request

<details><summary>Answer</summary>

**D.** A refusal is a deliberate outcome, not a transient error.
</details>

**5.** A response stops with `max_tokens` partway through a `tool_use` block. What should you do?
- A. Execute the partial tool call
- B. Ignore the tool call and return the text
- C. Retry the request with a higher `max_tokens` so a complete tool call is produced
- D. Switch `tool_choice` to `none`

<details><summary>Answer</summary>

**C.** An incomplete `tool_use` block can't be executed safely.
</details>

**6.** An Agent SDK coding agent must never run `rm -rf` or `git push --force`, whatever the prompt says. What is the best control?
- A. A `PreToolUse` hook that inspects Bash commands and denies matching patterns
- B. A sentence in the system prompt
- C. A `Stop` hook
- D. Lower temperature

<details><summary>Answer</summary>

**A.** Hooks enforce rules deterministically before a tool runs.
</details>

**7.** An Agent SDK job runs unattended inside a throwaway container with no secrets or network access to production. Which permission approach is acceptable?
- A. `default` mode, with a human approving every edit
- B. `bypassPermissions`, because the environment is isolated and disposable
- C. `plan` mode, so it can't edit anything
- D. No permission configuration at all on a developer laptop

<details><summary>Answer</summary>

**B.** Bypassing permissions is only reasonable in a sandbox like this. Never do it on a host with real data or credentials.
</details>

**8.** In the Agent SDK, how do you define a specialised "test-runner" subagent in code?
- A. Paste its instructions into the user prompt
- B. Create a new API key for it
- C. Start a separate process with no configuration
- D. Pass an agent definition with a `description` (which tells the main agent when to delegate), a `prompt`, and an allowed `tools` list

<details><summary>Answer</summary>

**D.** Subagents can be defined programmatically or as `.claude/agents/*.md` files with the same fields.
</details>

**9.** A user returns the next day to continue a conversation with your SDK-based agent. What is the most appropriate approach?
- A. Resume the stored session by its session ID so the prior context continues
- B. Re-send every original tool call
- C. Ask the user to repeat everything
- D. Put yesterday's transcript in the system prompt as raw text

<details><summary>Answer</summary>

**A.** The SDK supports resuming sessions. For long gaps, combine this with a structured summary or notes.
</details>

**10.** A chatbot must screen each user message for policy violations while generating the reply, without adding latency. Which pattern fits?
- A. Prompt chaining: screen first, then respond
- B. Parallelisation (sectioning): a guardrail call screens the input at the same time as the main call answers
- C. Evaluator–optimiser
- D. Orchestrator–workers

<details><summary>Answer</summary>

**B.** Running the guardrail and the response in parallel avoids adding the screening time to every reply.
</details>

**11.** A task requires fixing a GitHub issue in an unfamiliar repo where the files involved can't be known in advance. What is the best fit?
- A. A fixed three-step prompt chain
- B. A routing classifier
- C. An agent that explores, edits and runs tests in a loop
- D. A single completion call

<details><summary>Answer</summary>

**C.** The path is open-ended and depends on what the agent finds.
</details>

**12.** What distinguishes orchestrator–workers from simple parallelisation?
- A. Orchestrator–workers always uses one model
- B. Parallelisation can't use tools
- C. Orchestrator–workers is always cheaper
- D. In orchestrator–workers, the subtasks aren't predefined. The orchestrator decides them at runtime from the input.

<details><summary>Answer</summary>

**D.**
</details>

**13.** How should an autonomous agent judge whether it is making progress?
- A. By "ground truth" from the environment at each step: tool results, test outcomes, command output
- B. By counting how many tokens it has written
- C. By asking itself whether it feels finished
- D. By the elapsed wall-clock time

<details><summary>Answer</summary>

**A.** Agents rely on feedback from the environment to stay on track.
</details>

**14.** Which are sound stopping conditions for an autonomous agent loop? (Choose 2)
- A. The model returns `end_turn` because the task is complete
- B. The user's message contains a question mark
- C. A hard limit on iterations, tokens, cost or time is reached
- D. The model mentions the word "finished" in its text

<details><summary>Answer</summary>

**A and C.**
</details>

**15.** A long-running agent will change production infrastructure. Where should humans be involved?
- A. Nowhere. The agent should be fully autonomous.
- B. Only after everything is finished
- C. At defined checkpoints, especially before irreversible or high-impact actions, with the agent pausing for approval
- D. Only when the model asks for help

<details><summary>Answer</summary>

**C.** Approval gates belong before high-risk steps, and they should be enforced in code.
</details>

---

<details><summary>Quick answer key</summary>

1 C · 2 A · 3 B · 4 D · 5 C · 6 A · 7 B · 8 D · 9 A · 10 B · 11 C · 12 D · 13 A · 14 A,C · 15 C
</details>
