# D2 Practice Set 3 — MCP in Depth (18%)

15 original scenario-style questions (not real exam items). Give yourself **30 minutes**. Notes: [../../domains/d2-tool-design-mcp.md](../../domains/d2-tool-design-mcp.md)

---

**1.** A company wants the same internal knowledge-base integration available in Claude Code, Claude Desktop and its own agents. What is the best approach?
- A. Write a separate custom integration for each one
- B. Build one MCP server and connect each host to it
- C. Paste the knowledge base into every prompt
- D. Use a Skill only

<details><summary>Answer</summary>

**B.** MCP's value is one integration that many hosts can reuse.
</details>

**2.** In MCP, how do hosts, clients and servers relate?
- A. One client serves many hosts
- B. Servers connect to each other directly
- C. The host runs one MCP client per server connection, and each client talks to exactly one server
- D. The LLM connects to servers directly without a client

<details><summary>Answer</summary>

**C.**
</details>

**3.** What message format does MCP use?
- A. GraphQL
- B. JSON-RPC 2.0
- C. gRPC/protobuf
- D. SOAP

<details><summary>Answer</summary>

**B.**
</details>

**4.** How do an MCP client and server learn which features (tools, resources, prompts and so on) the other supports?
- A. Capability negotiation during the `initialize` handshake
- B. They guess
- C. From the server's README
- D. From the model's training data

<details><summary>Answer</summary>

**A.**
</details>

**5.** A platform team wants every engineer to have standard, parameterised workflows like "review this PR" or "draft a release" that users choose explicitly. Which MCP primitive fits?
- A. Tools
- B. Resources
- C. Sampling
- D. Prompts

<details><summary>Answer</summary>

**D.** Prompts are user-controlled templates. In Claude Code they appear as slash commands.
</details>

**6.** A server should expose any database table's schema without listing every table up front. What is the right feature?
- A. One tool per table
- B. A resource template with a parameterised URI, e.g. `db://tables/{name}/schema`
- C. A prompt per table
- D. A static resource list of 10,000 entries

<details><summary>Answer</summary>

**B.**
</details>

**7.** In Claude Code, you want to add a remote HTTP MCP server and share its configuration with the team through the repo. Which command?
- A. `claude mcp add --transport http --scope project linear https://mcp.example.com/mcp`
- B. `claude mcp add --scope user linear`
- C. `claude -p "add linear"`
- D. Edit `~/.claude/CLAUDE.md`

<details><summary>Answer</summary>

**A.** Project scope writes the server to `.mcp.json`, which you check in.
</details>

**8.** In Claude Code permission rules, how do you refer to a specific MCP tool?
- A. `MCP(github.create_issue)`
- B. `github:create_issue`
- C. `mcp__github__create_issue`
- D. `tool/github/create_issue`

<details><summary>Answer</summary>

**C.** MCP tools are named `mcp__<server>__<tool>`.
</details>

**9.** Claude should be able to create GitHub issues without asking, but must not merge PRs without approval. What is the best configuration?
- A. Allow all `mcp__github__*` tools
- B. Use `bypassPermissions`
- C. Add a CLAUDE.md note
- D. Allow `mcp__github__create_issue` explicitly, and leave merge tools on ask, or deny them

<details><summary>Answer</summary>

**D.** Use fine-grained permission rules per MCP tool.
</details>

**10.** A stdio MCP server randomly breaks with protocol parse errors. Its developer uses `print()` for debug logging. What is the cause?
- A. Logging to stdout corrupts the JSON-RPC stream. Log to stderr (or a file) instead.
- B. Python is unsupported
- C. The server needs OAuth
- D. Too many tools

<details><summary>Answer</summary>

**A.** In stdio transport, stdout carries the protocol messages.
</details>

**11.** You want a Messages API application to use tools from a remote MCP server without writing an MCP client. What can you use?
- A. The Batch API
- B. The MCP connector: the `mcp_servers` parameter on the Messages API
- C. Prompt caching
- D. A Skill

<details><summary>Answer</summary>

**B.**
</details>

**12.** An MCP server needs an LLM to summarise data, but it shouldn't hold its own model API key. Which MCP feature lets it ask the client's model instead?
- A. Resources
- B. Roots
- C. Sampling
- D. Prompts

<details><summary>Answer</summary>

**C.** Sampling lets a server request a completion through the client, with the user in control.
</details>

**13.** Your team wants to install a popular third-party MCP server. What is the main security consideration?
- A. None, because MCP servers are sandboxed by the protocol
- B. Only check its star count
- C. Disable all other servers
- D. Review it before trusting it, since tool descriptions and outputs can carry prompt injection or malicious instructions. Use least privilege and pin versions.

<details><summary>Answer</summary>

**D.**
</details>

**14.** When should a capability be an MCP *tool* rather than a *resource*? (Choose 2)
- A. When it performs an action with side effects (e.g. creating a ticket)
- B. When it's static, read-only reference data that the application attaches as context
- C. When the model should decide on its own whether and when to invoke it
- D. When it's a template the user picks

<details><summary>Answer</summary>

**A and C.** B describes a resource, and D describes a prompt.
</details>

**15.** A newly added MCP server's tools don't appear in Claude Code. What are good first debugging steps?
- A. Reinstall the OS
- B. Run `/mcp` in Claude Code to check connection status, and test the server on its own with the MCP Inspector
- C. Rewrite the server in another language
- D. Increase `max_tokens`

<details><summary>Answer</summary>

**B.**
</details>

---

<details><summary>Quick answer key</summary>

1 B · 2 C · 3 B · 4 A · 5 D · 6 B · 7 A · 8 C · 9 D · 10 A · 11 B · 12 C · 13 D · 14 A,C · 15 B
</details>
