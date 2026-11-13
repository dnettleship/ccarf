# D3 Practice Set 1 — Claude Code Configuration & Workflows (20%)

15 original scenario-style questions (not real exam items). Give yourself **30 minutes**. Notes: [../domains/d3-claude-code.md](../../domains/d3-claude-code.md)

---

**1.** You want Claude to use British spelling in code comments across all your projects, without affecting your teammates. Where does this instruction go?
- A. `./CLAUDE.md` in each repo
- B. `~/.claude/CLAUDE.md`
- C. `.claude/settings.json`
- D. The enterprise managed policy

<details><summary>Answer</summary>

**B.** User-level memory applies to all your projects and only to you.
</details>

**2.** In a monorepo, the frontend conventions should apply only when Claude works in `packages/frontend/`. Which option fits?
- A. Put them in the root `CLAUDE.md`
- B. Put them in `~/.claude/CLAUDE.md`
- C. Put a `CLAUDE.md` in `packages/frontend/` (or a `.claude/rules/` file with a `paths:` glob)
- D. A `Stop` hook

<details><summary>Answer</summary>

**C.** Subdirectory CLAUDE.md files and path-scoped rules load only when they're relevant, which keeps unrelated context out.
</details>

**3.** You want CLAUDE.md to include the team's existing `docs/conventions.md` without copying it. How?
- A. Paste its contents in
- B. Link to it in a markdown comment
- C. Add it to `.mcp.json`
- D. Add the line `@docs/conventions.md`

<details><summary>Answer</summary>

**D.** `@path` imports another file into memory.
</details>

**4.** Prettier must run automatically after Claude edits any file. What is the best mechanism?
- A. A `PostToolUse` hook matching `Edit|Write` that runs the formatter
- B. An instruction in CLAUDE.md
- C. A slash command
- D. Plan Mode

<details><summary>Answer</summary>

**A.** For something that must always happen, use a deterministic hook. Instructions can be forgotten.
</details>

**5.** Claude must never read `.env`. What is the strongest control?
- A. "Please don't read .env" in CLAUDE.md
- B. A `permissions.deny` rule such as `Read(./.env)` (optionally backed by a `PreToolUse` hook)
- C. Rename the file
- D. Use `acceptEdits` mode

<details><summary>Answer</summary>

**B.** Deny rules are enforced by the harness, not left to the model.
</details>

**6.** A `PreToolUse` hook script exits with code **2**. What happens?
- A. The tool runs and a warning is logged
- B. Claude Code crashes
- C. The tool call is blocked, and the hook's stderr is passed back to Claude so it can adjust
- D. The session is cleared

<details><summary>Answer</summary>

**C.** Exit code 2 is a blocking error for that event.
</details>

**7.** The team's `.claude/settings.json` allows `Bash(npm run:*)`. Your `.claude/settings.local.json` denies `Bash(npm run deploy)`. What happens when Claude tries `npm run deploy`?
- A. It runs, because the team allow rule is broader
- B. Claude Code asks every time
- C. It runs, because project settings beat local settings
- D. It's blocked, because deny rules take precedence

<details><summary>Answer</summary>

**D.** Deny wins.
</details>

**8.** You need a personal settings override, such as a different model, that must never be committed. Where does it go?
- A. `.claude/settings.local.json`
- B. `.claude/settings.json`
- C. `CLAUDE.md`
- D. `.mcp.json`

<details><summary>Answer</summary>

**A.** Local project settings are personal and git-ignored.
</details>

**9.** You want a code-review subagent that can read the code but can't edit anything. How do you define it?
- A. Add "be read-only" to CLAUDE.md
- B. `.claude/agents/reviewer.md` with a clear `description` and `tools: Read, Grep, Glob`
- C. A slash command with no arguments
- D. Run Claude Code in Plan Mode forever

<details><summary>Answer</summary>

**B.** A subagent's `tools` field limits what it can do, and its `description` drives when Claude delegates to it.
</details>

**10.** You created `.claude/skills/release-notes/SKILL.md`, but Claude never uses it. What is the most likely cause?
- A. Skills only work in Plan Mode
- B. The skill needs a hook to activate it
- C. The frontmatter `description` is vague, so Claude can't tell when the skill applies
- D. Skills must be written in JSON

<details><summary>Answer</summary>

**C.** Claude decides whether to load a skill from its `name` and `description`. Say what it does and when to use it.
</details>

**11.** You're deep into one task and the context is nearly full, but you want to keep going on the same task. What should you do?
- A. `/clear`
- B. Start a new terminal
- C. Lower `max_tokens`
- D. `/compact`, optionally with instructions on what to keep

<details><summary>Answer</summary>

**D.** `/compact` summarises the history and keeps the thread. `/clear` is for switching to an unrelated task.
</details>

**12.** Claude's last three edits went in the wrong direction. What is the quickest way to restore the files and the conversation to before them?
- A. Press Esc twice (or run `/rewind`) and pick a checkpoint
- B. `git reset --hard origin/main`
- C. `/compact`
- D. Restart Claude Code

<details><summary>Answer</summary>

**A.** Checkpoints restore code and/or conversation. Note that they don't track changes made through bash commands, so keep using git too.
</details>

**13.** A nightly CI job should have Claude fix lint errors, and the pipeline parses the result. Which invocation fits?
- A. Interactive `claude` with `bypassPermissions` on a shared server
- B. `claude -p "fix lint errors" --output-format json --max-turns 10 --allowedTools "Read,Edit,Bash(npm run lint:*)"`
- C. `claude -c`
- D. A slash command run by hand

<details><summary>Answer</summary>

**B.** Headless mode, machine-readable output, a turn limit, and the minimum tools needed.
</details>

**14.** Which steps belong in a test-driven workflow with Claude Code? (Choose 2)
- A. Write the tests first and confirm they fail before implementing
- B. Let Claude change the tests so they pass
- C. Tell Claude to implement until the tests pass without modifying them
- D. Skip running the tests to save time

<details><summary>Answer</summary>

**A and C.** The tests act as a fixed target that Claude can check itself against.
</details>

**15.** You often type the same long prompt to fetch a GitHub issue by number and fix it. What is the best way to make that reusable?
- A. Put it in CLAUDE.md
- B. A `SessionStart` hook
- C. An MCP prompt on a remote server
- D. A custom slash command in `.claude/commands/fix-issue.md` that uses `$ARGUMENTS` for the issue number

<details><summary>Answer</summary>

**D.** You then run it as `/fix-issue 123`.
</details>

---

<details><summary>Quick answer key</summary>

1 B · 2 C · 3 D · 4 A · 5 B · 6 C · 7 D · 8 A · 9 B · 10 C · 11 D · 12 A · 13 B · 14 A,C · 15 D
</details>
