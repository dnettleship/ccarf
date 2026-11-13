# D2 Practice Set 2 — Tool Design in Depth (18%)

15 original scenario-style questions (not real exam items). Give yourself **30 minutes**. Notes: [../../domains/d2-tool-design-mcp.md](../../domains/d2-tool-design-mcp.md)

---

**1.** An agent is connected to both Jira and Asana, and each exposes a tool called `search`. Claude mixes them up. What is the best fix?
- A. Namespace the tool names by service (e.g. `jira_search_issues`, `asana_search_tasks`) and make the descriptions explicit
- B. Remove one service
- C. Randomise the tool order
- D. Lower the temperature

<details><summary>Answer</summary>

**A.** Clear, namespaced names reduce confusion between similar tools.
</details>

**2.** A tool has a parameter named `user`, and Claude sometimes passes names, sometimes emails and sometimes IDs. What is the best fix?
- A. Accept anything and guess
- B. Add more tools
- C. Rename it to `user_id`, describe the exact format (e.g. "numeric ID from `find_user`"), and constrain it with a `pattern`
- D. Make the parameter optional

<details><summary>Answer</summary>

**C.** Unambiguous names, documented formats and schema constraints prevent bad inputs.
</details>

**3.** Sometimes the agent needs just IDs from a tool, and sometimes full details. What design helps?
- A. Always return everything
- B. A `response_format` parameter (e.g. `"concise"` or `"detailed"`) so the agent can control how verbose the output is
- C. Two copies of the tool with identical descriptions
- D. Always return IDs only

<details><summary>Answer</summary>

**B.** Letting the agent choose verbosity saves context when full detail isn't needed.
</details>

**4.** To book a meeting, an agent must call `list_users`, `list_events` and `create_event`, which takes many steps and fills its context. What is a better design?
- A. Add a fourth tool
- B. Remove `create_event`
- C. Tell the agent to hurry
- D. A higher-level `schedule_meeting` tool that finds availability and books in one call

<details><summary>Answer</summary>

**D.** Build tools around tasks the agent actually performs, not thin wrappers over API endpoints.
</details>

**5.** A search tool truncates results at 50 items. How should it say so?
- A. Append a note such as "Showing 50 of 1,240 results. Narrow the query (e.g. add a date range) or request the next page."
- B. Say nothing
- C. Return an error
- D. Return random items

<details><summary>Answer</summary>

**A.** Tell the agent how to get what it needs.
</details>

**6.** A date parameter is rejected, and the tool returns a 40-line Python traceback. What is better?
- A. Return the traceback, since it's more detailed
- B. Return `is_error: true` with an actionable message, e.g. "`start_date` must be YYYY-MM-DD; received '13/11/2026'"
- C. Return an empty result
- D. Silently fix the date with a guess

<details><summary>Answer</summary>

**B.** Clear, specific errors let Claude correct the call itself.
</details>

**7.** How should you systematically improve a set of tools?
- A. Rely on intuition
- B. Increase the number of tools
- C. Build realistic evaluation tasks, measure task success and tool-call accuracy, read the agent's transcripts, then refine the names, descriptions and outputs
- D. Only test the happy path

<details><summary>Answer</summary>

**C.**
</details>

**8.** Tool inputs must always match the schema exactly, for example an integer quantity that can never be a string. What helps most, where supported?
- A. Strict tool use (`strict: true`), so inputs are constrained to the schema, plus validation in your tool
- B. Lower the temperature
- C. Use `tool_choice: none`
- D. Remove `required`

<details><summary>Answer</summary>

**A.** Constrain the input where the platform supports it, and still validate on the server side.
</details>

**9.** Two tools must never run in the same turn, because the second depends on the first's result. What setting helps?
- A. `tool_choice: none`
- B. Extended thinking
- C. Streaming
- D. `disable_parallel_tool_use: true`

<details><summary>Answer</summary>

**D.** This limits Claude to at most one tool call per response.
</details>

**10.** A `render_chart` tool produces a PNG that Claude should look at. How should you return it?
- A. As a base64 string inside a text block
- B. As an image content block inside the `tool_result`
- C. As a file path only
- D. As a separate user message after the next turn

<details><summary>Answer</summary>

**B.** A `tool_result` can contain image blocks, so Claude can actually see the image.
</details>

**11.** You enable Anthropic's bash tool. Who actually runs the commands?
- A. Anthropic's servers
- B. Nobody, because it's simulated
- C. Your application, in an environment you control (ideally sandboxed)
- D. The user's browser

<details><summary>Answer</summary>

**C.** Anthropic-defined client tools (bash, text editor, computer use) have built-in schemas, but you execute them.
</details>

**12.** You enable the memory tool for a long-running agent. Where is the memory stored?
- A. Client-side. Your application implements the storage behind the tool's file-like operations.
- B. Automatically inside the model weights
- C. On Anthropic's servers indefinitely
- D. In the prompt cache

<details><summary>Answer</summary>

**A.** You control where memories live and how long they're kept.
</details>

**13.** What should tools with side effects, such as deleting records or sending email, have? (Choose 2)
- A. Least-privilege credentials scoped to exactly what the tool needs
- B. Broad admin credentials, for flexibility
- C. Confirmation or approval for destructive or external actions
- D. No input validation, for speed

<details><summary>Answer</summary>

**A and C.**
</details>

**14.** Guidance on how to use `refund_order` is scattered across a 3-page system prompt, and Claude misuses the tool. Where should most of that guidance go?
- A. A separate user message on every turn
- B. Nowhere, because Claude should infer it
- C. The tool's own description and parameter descriptions, next to the definition Claude reads when choosing and calling it
- D. Code comments in your backend

<details><summary>Answer</summary>

**C.**
</details>

**15.** Which statement about tool definitions and cost is true?
- A. Tool definitions are free
- B. Tool definitions are only billed when a tool is called
- C. Each tool reduces output token cost
- D. Tool definitions count as input tokens on every request, and because they sit at the start of the prefix they can be prompt-cached

<details><summary>Answer</summary>

**D.**
</details>

---

<details><summary>Quick answer key</summary>

1 A · 2 C · 3 B · 4 D · 5 A · 6 B · 7 C · 8 A · 9 D · 10 B · 11 C · 12 A · 13 A,C · 14 C · 15 D
</details>
