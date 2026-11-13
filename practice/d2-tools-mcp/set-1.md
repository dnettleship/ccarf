# D2 Practice Set 1 — Tool Design & MCP Integration (18%)

15 original scenario-style questions (not real exam items). Give yourself **30 minutes**. Notes: [../domains/d2-tool-design-mcp.md](../../domains/d2-tool-design-mcp.md)

---

**1.** An agent has both `search_customers` and `find_customer`, each with a one-line description, and often calls the wrong one. What is the best fix?
- A. Raise the temperature
- B. Put them in alphabetical order
- C. Merge them, or rewrite both descriptions to say exactly when to use each and when not to
- D. Remove tool descriptions to save tokens

<details><summary>Answer</summary>

**C.** Claude chooses tools mainly from their names and descriptions. Overlapping tools need to be merged or clearly distinguished.
</details>

**2.** Every request must produce a call to `classify_ticket`. Which `tool_choice` should you use?
- A. `{"type": "tool", "name": "classify_ticket"}`
- B. `{"type": "auto"}`
- C. `{"type": "any"}`
- D. `{"type": "none"}`

<details><summary>Answer</summary>

**A.** This forces that specific tool. `any` only guarantees that *some* tool is called.
</details>

**3.** Claude must call one of `search_kb`, `search_tickets` or `search_docs`, but it should choose which one. Which `tool_choice` fits?
- A. `auto`
- B. `none`
- C. `tool` with `search_kb`
- D. `any`

<details><summary>Answer</summary>

**D.** `any` requires at least one tool call and leaves the choice of tool to Claude.
</details>

**4.** With a forced `tool_choice`, you notice Claude never writes any explanatory text before the `tool_use` block. Why?
- A. A bug in the SDK
- B. When `tool_choice` is `any` or a specific tool, the response is prefilled to call the tool, so there's no natural-language preamble
- C. `max_tokens` is too low
- D. The system prompt forbids text

<details><summary>Answer</summary>

**B.** This is documented behaviour. Use `auto` with an instruction if you want text and a tool call together.
</details>

**5.** You turn on extended thinking and set `tool_choice: {"type": "any"}`, and the request fails. Why?
- A. Extended thinking doesn't support tools at all
- B. You must also set `temperature` to 0
- C. With extended thinking, only `auto` and `none` are supported for `tool_choice`
- D. `any` is deprecated

<details><summary>Answer</summary>

**C.** Forced tool choice isn't compatible with extended thinking.
</details>

**6.** A `get_order` tool returns 500 lines of raw JSON, including internal fields and UUIDs. Answers get worse and context fills up. What is the best fix?
- A. Return only the fields Claude needs, with human-readable identifiers, and paginate or truncate with a note saying more is available
- B. Raise `max_tokens`
- C. Ask Claude to ignore irrelevant fields
- D. Compress the JSON with base64

<details><summary>Answer</summary>

**A.** Tools should return concise output that carries the information Claude actually needs.
</details>

**7.** When the database is down, `lookup_customer` returns `[]`, and Claude tells the user "No such customer exists." What is the fix?
- A. Retry forever
- B. Add "the database might be down" to the system prompt
- C. Return an empty object instead
- D. Return a `tool_result` with `is_error: true` and a message such as "Customer database unavailable (timeout); temporary — try again shortly"

<details><summary>Answer</summary>

**D.** Claude can't tell "no results" from "the call failed" unless the tool says so.
</details>

**8.** Which of these belong in a tool description? (Choose 2)
- A. The internal database table names it queries
- B. What the tool does, and when to use it and when not to
- C. A marketing tagline
- D. What each parameter means, its format, and an example value

<details><summary>Answer</summary>

**B and D.** A and C add tokens without helping Claude choose or call the tool.
</details>

**9.** You want an MCP server to expose a database schema as read-only context that the *application* decides when to include. Which MCP primitive fits?
- A. Tools
- B. Resources
- C. Prompts
- D. Sampling

<details><summary>Answer</summary>

**B.** Resources are application-controlled, read-only data addressed by URI.
</details>

**10.** A vendor hosts an MCP server for its SaaS product, and many customers connect to it over the internet. Which transport and auth are typical?
- A. stdio, no auth
- B. stdio with an API key in `.mcp.json`
- C. Streamable HTTP with OAuth
- D. WebSockets with basic auth

<details><summary>Answer</summary>

**C.** stdio is for local subprocesses. Remote servers use Streamable HTTP, usually with OAuth.
</details>

**11.** A team wants to share an internal MCP server's configuration through the repo, but the server needs an API token. What is the best approach?
- A. Commit `.mcp.json` and reference the token through environment variable expansion (e.g. `${TICKETS_TOKEN}`). Each developer sets it locally.
- B. Commit the token into `.mcp.json`
- C. Put the token in CLAUDE.md
- D. Email the token and ask everyone to paste it into the config

<details><summary>Answer</summary>

**A.** Share the configuration, never the secrets.
</details>

**12.** A new team member opens the repo and Claude Code asks them to approve the MCP servers from `.mcp.json`. Why?
- A. Their licence has expired
- B. MCP servers must be reinstalled on every launch
- C. `.mcp.json` is corrupt
- D. Project-scoped servers come from a checked-in file, so Claude Code asks for approval before using them, as a security measure

<details><summary>Answer</summary>

**D.** This protects users from running servers configured by whoever last edited the repo.
</details>

**13.** An agent needs web search, and the team doesn't want to build or host search infrastructure. What should they use?
- A. A client tool that scrapes Google
- B. Anthropic's server-side web search tool, which Anthropic executes and returns results with citations
- C. Prefilling the answer
- D. A Skill that contains a search engine

<details><summary>Answer</summary>

**B.** Server tools run on Anthropic's side. You don't run them yourself.
</details>

**14.** One agent has 60 tools across six business systems, and it increasingly picks the wrong tool. What is the best architectural fix?
- A. Raise the temperature
- B. Merge all 60 tools into a single `do_anything` tool
- C. Give specialised subagents only the tools for their area, or load tools on demand through tool search
- D. Remove all the descriptions

<details><summary>Answer</summary>

**C.** Selection accuracy and context cost get worse as the tool list grows, so expose only the tools that are relevant.
</details>

**15.** Your team has a 12-step, brand-compliant procedure for building slide decks, with templates and a helper script. It doesn't touch any external system. What is the best way to package it?
- A. A Skill (`SKILL.md` plus the templates and script), loaded only when relevant
- B. An MCP server
- C. A `PreToolUse` hook
- D. Copy the whole procedure into every prompt

<details><summary>Answer</summary>

**A.** Skills package procedures and know-how and load them on demand. MCP is for external actions and data.
</details>

---

<details><summary>Quick answer key</summary>

1 C · 2 A · 3 D · 4 B · 5 C · 6 A · 7 D · 8 B,D · 9 B · 10 C · 11 A · 12 D · 13 B · 14 C · 15 A
</details>
