# D3 Practice Set 3 — Workflows, Subagents, Skills, Plugins & CI (20%)

15 original scenario-style questions (not real exam items). Give yourself **30 minutes**. Notes: [../../domains/d3-claude-code.md](../../domains/d3-claude-code.md)

---

**1.** On a tricky bug, Claude jumps straight into editing and makes things worse. Which workflow does Anthropic recommend?
- A. Explore → plan → code → commit: have Claude read the relevant files and propose a plan *before* writing code
- B. Code first, then read the files
- C. Ask for the fix in one sentence
- D. Use `bypassPermissions` so it goes faster

<details><summary>Answer</summary>

**A.**
</details>

**2.** You want Claude to start a session in read-only planning mode from the command line. How?
- A. `claude --dry-run`
- B. `claude --permission-mode plan` (or press Shift+Tab during a session)
- C. `claude -p plan`
- D. `/compact plan`

<details><summary>Answer</summary>

**B.**
</details>

**3.** You're building UI from a design mock-up. What gives the best results?
- A. A text description only
- B. A larger CLAUDE.md
- C. Give Claude the mock-up image and a way to screenshot its output, so it can compare and iterate
- D. Plan Mode only

<details><summary>Answer</summary>

**C.** Visual targets plus a way to check against them let Claude iterate well.
</details>

**4.** You want two Claude sessions working at the same time on different features in the same repo without stepping on each other. What should you use?
- A. Two terminals in the same checkout
- B. `bypassPermissions`
- C. `/clear` between edits
- D. Git worktrees, one per session

<details><summary>Answer</summary>

**D.**
</details>

**5.** Running the full test suite floods the main conversation with thousands of log lines. What is the best fix?
- A. Delegate test runs to a subagent that returns only the failures and a summary
- B. Stop running tests
- C. Raise the context window
- D. Turn off output

<details><summary>Answer</summary>

**A.** Subagents keep noisy work out of the main context.
</details>

**6.** A documentation-lookup subagent should use a cheaper, faster model. How?
- A. It can't. Subagents always use the main model.
- B. Set `model: haiku` in the subagent's frontmatter
- C. Use a separate API key
- D. Add "be fast" to its prompt

<details><summary>Answer</summary>

**B.**
</details>

**7.** How do Skills avoid filling the context when you have dozens of them installed?
- A. Only one Skill can be installed at a time
- B. They're compressed
- C. Only each Skill's name and description are loaded up front. The full `SKILL.md` and its bundled files load only when the Skill is relevant (progressive disclosure).
- D. They run on a separate model

<details><summary>Answer</summary>

**C.**
</details>

**8.** A "db-migration" Skill should only let Claude use read and SQL-runner tools while it's active. Which frontmatter field restricts this?
- A. `allowed-tools`
- B. `permissions`
- C. `matcher`
- D. `scope`

<details><summary>Answer</summary>

**A.**
</details>

**9.** Your platform team wants to distribute a bundle of slash commands, subagents, hooks and MCP servers to 40 repos and keep it versioned. What is the best mechanism?
- A. Copy the files into each repo by hand
- B. Email a zip file
- C. One giant CLAUDE.md
- D. A Claude Code plugin, published through a plugin marketplace

<details><summary>Answer</summary>

**D.**
</details>

**10.** A `/commit` slash command should include the current `git status` and `git diff` output in its prompt automatically. How?
- A. Ask Claude to remember the status
- B. In the command file, use the `!` prefix to run bash and inject its output (allowing the commands with `allowed-tools`)
- C. Use a `Stop` hook
- D. Put the diff in CLAUDE.md

<details><summary>Answer</summary>

**B.**
</details>

**11.** How does the Claude Code GitHub Action typically get triggered on a PR?
- A. Automatically on every commit to every branch, with no configuration
- B. By emailing Anthropic
- C. By mentioning `@claude` in a PR or issue comment (or by a configured workflow trigger), with the API key held in GitHub secrets
- D. Only manually from a laptop

<details><summary>Answer</summary>

**C.**
</details>

**12.** A CI dashboard should show Claude's progress live while a headless run is happening. Which output format?
- A. `--output-format stream-json`
- B. `--output-format text`
- C. `--output-format json`
- D. `--verbose` only

<details><summary>Answer</summary>

**A.** `stream-json` emits events as they happen. `json` gives a single result at the end.
</details>

**13.** You closed your terminal and want to pick up yesterday's session, choosing from a list. Which command?
- A. `claude -c`
- B. `claude --init`
- C. `claude -p`
- D. `claude --resume` (`-r`)

<details><summary>Answer</summary>

**D.** `-c` continues the most recent session. `--resume` lets you pick one.
</details>

**14.** When is `--permission-mode bypassPermissions` acceptable? (Choose 2)
- A. In an isolated container or devcontainer with no access to production credentials
- B. On your everyday laptop with SSH keys and cloud credentials
- C. In a disposable CI sandbox with restricted network access
- D. Whenever prompts get annoying

<details><summary>Answer</summary>

**A and C.**
</details>

**15.** A new joiner wants Claude Code to explain *why* it makes changes, to help them learn the codebase. Which feature changes Claude Code's behaviour this way?
- A. Plan Mode
- B. Hooks
- C. Output styles (e.g. an explanatory or learning style)
- D. `/compact`

<details><summary>Answer</summary>

**C.** Output styles change Claude Code's system prompt for a different interaction style.
</details>

---

<details><summary>Quick answer key</summary>

1 A · 2 B · 3 C · 4 D · 5 A · 6 B · 7 C · 8 A · 9 D · 10 B · 11 C · 12 A · 13 D · 14 A,C · 15 C
</details>
