# D3 Practice Set 2 — Memory, Settings & Hooks (20%)

15 original scenario-style questions (not real exam items). Give yourself **30 minutes**. Notes: [../../domains/d3-claude-code.md](../../domains/d3-claude-code.md)

---

**1.** You're starting to use Claude Code on an existing repo that has no CLAUDE.md. What is the fastest good first step?
- A. Run `/init` to generate a starter CLAUDE.md from the codebase, then edit it
- B. Copy a CLAUDE.md from the internet
- C. Write a 50-page CLAUDE.md by hand
- D. Skip memory entirely

<details><summary>Answer</summary>

**A.**
</details>

**2.** Which content belongs in a project CLAUDE.md?
- A. The full API reference for every dependency
- B. Build, test and lint commands, key conventions, architecture pointers, and known gotchas, kept concise
- C. Personal API keys
- D. Your chat history

<details><summary>Answer</summary>

**B.** CLAUDE.md is loaded every session, so keep it short and specific.
</details>

**3.** Security requires that every developer's Claude Code follows the same secure-coding rules, and individuals must not be able to override them. What is the right mechanism?
- A. Each repo's CLAUDE.md
- B. A Slack reminder
- C. Enterprise managed policy files (managed CLAUDE.md and managed settings) deployed by IT
- D. User-level `~/.claude/CLAUDE.md`

<details><summary>Answer</summary>

**C.** Managed policy sits at the top of the precedence order.
</details>

**4.** At the start of every session, you want to load the current sprint's open tickets into Claude's context automatically. Which hook event fits?
- A. `PostToolUse`
- B. `PreCompact`
- C. `Stop`
- D. `SessionStart`

<details><summary>Answer</summary>

**D.** A `SessionStart` hook can add context when a session begins.
</details>

**5.** You want to block prompts that contain something that looks like an AWS secret key before Claude ever sees them. Which hook event?
- A. `UserPromptSubmit`
- B. `PostToolUse`
- C. `Notification`
- D. `SubagentStop`

<details><summary>Answer</summary>

**A.** It runs before the prompt is processed and can block it.
</details>

**6.** Claude sometimes says it's done while tests are still failing. Which hook can make it keep working until the tests pass?
- A. `SessionEnd`
- B. A `Stop` hook that runs the tests and blocks stopping (with feedback) if they fail
- C. `Notification`
- D. `PreCompact`

<details><summary>Answer</summary>

**B.** A `Stop` hook can prevent Claude from finishing and tell it why.
</details>

**7.** A `PreToolUse` hook should apply only to shell commands. How do you scope it?
- A. Check the tool name inside a hook that runs on everything
- B. Put it in CLAUDE.md
- C. Set the hook's `matcher` to `Bash`
- D. Use a separate settings file per tool

<details><summary>Answer</summary>

**C.** Matchers select the tools a hook runs for. Patterns like `Edit|Write` or `mcp__.*` also work.
</details>

**8.** Instead of using exit codes, a `PreToolUse` hook wants to *ask* the user for confirmation on certain commands. How?
- A. Exit with code 0
- B. Print "please confirm" to stdout
- C. Exit with code 1
- D. Return JSON output with a permission decision of `"ask"` (or `"allow"` / `"deny"`) plus a reason

<details><summary>Answer</summary>

**D.** Structured JSON output gives finer control than exit codes.
</details>

**9.** You'd like a desktop alert whenever Claude Code is waiting for your input. Which hook event fits?
- A. `Notification`
- B. `PreToolUse`
- C. `UserPromptSubmit`
- D. `SessionStart`

<details><summary>Answer</summary>

**A.**
</details>

**10.** What is the key security point about hooks?
- A. They run in a sandbox with no access
- B. They're only suggestions to the model
- C. They run automatically with your user's permissions, so review any hook configuration (especially from shared repos) before trusting it
- D. They can only read files

<details><summary>Answer</summary>

**C.**
</details>

**11.** You want Claude to run `git diff` and `git log` with any arguments without asking, but nothing else in git. Which rules?
- A. `Bash(*)`
- B. `Bash(git diff:*)` and `Bash(git log:*)` in `permissions.allow`
- C. `Bash(git:*)`
- D. `bypassPermissions`

<details><summary>Answer</summary>

**B.** Use prefix rules that are as narrow as possible.
</details>

**12.** Claude needs to read a shared library in a sibling folder outside the project directory. What is the right approach?
- A. Copy the library into the repo
- B. Run Claude Code from `/`
- C. Use `bypassPermissions`
- D. Add the folder as an extra working directory (`--add-dir` or `/add-dir`)

<details><summary>Answer</summary>

**D.**
</details>

**13.** Which belong in `.claude/settings.json` rather than CLAUDE.md? (Choose 2)
- A. Permission allow and deny rules
- B. "We use tabs, not spaces"
- C. Hook definitions
- D. The architecture overview

<details><summary>Answer</summary>

**A and C.** Settings are enforced configuration. CLAUDE.md is guidance for the model.
</details>

**14.** Every Claude Code session in the project needs `NODE_ENV=test` set. What is a clean way?
- A. Ask Claude to export it each time
- B. Put it in CLAUDE.md
- C. Use the `env` key in `.claude/settings.json`
- D. A `Stop` hook

<details><summary>Answer</summary>

**C.**
</details>

**15.** A team member's `~/.claude/settings.json` allows `Bash(curl:*)`, but the project's `.claude/settings.json` denies `Bash(curl:*)`. What happens when Claude tries `curl`?
- A. It's denied
- B. It's allowed, because user settings come first
- C. Claude Code errors out
- D. It's allowed only on weekdays

<details><summary>Answer</summary>

**A.** Deny rules take precedence, and project settings outrank user settings anyway.
</details>

---

<details><summary>Quick answer key</summary>

1 A · 2 B · 3 C · 4 D · 5 A · 6 B · 7 C · 8 D · 9 A · 10 C · 11 B · 12 D · 13 A,C · 14 C · 15 A
</details>
