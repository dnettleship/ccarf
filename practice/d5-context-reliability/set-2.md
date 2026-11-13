# D5 Practice Set 2 — Context Engineering & Caching (15%)

15 original scenario-style questions (not real exam items). Give yourself **30 minutes**. Notes: [../../domains/d5-context-reliability.md](../../domains/d5-context-reliability.md)

---

**1.** A team plans to "just put everything in context", since the window is huge. What is the main risk?
- A. None, because more context is always better
- B. As context grows, recall and focus degrade ("context rot"), and cost and latency rise. Aim for the smallest set of high-signal tokens.
- C. The API rejects anything over 10k tokens
- D. Caching stops working

<details><summary>Answer</summary>

**B.**
</details>

**2.** An agent's system prompt is either a brittle 200-line if/else rulebook or a one-line "be helpful". What is the better target?
- A. The rulebook
- B. The one-liner
- C. The "right altitude": clear guidance and principles that are specific enough to steer behaviour but flexible enough to generalise, organised into sections
- D. No system prompt

<details><summary>Answer</summary>

**C.**
</details>

**3.** When compacting a long coding session, what should the summary keep?
- A. Every raw tool output, verbatim
- B. Architectural decisions, unresolved bugs, key implementation details and next steps, while dropping redundant tool output
- C. Only the last message
- D. Only the user's first message

<details><summary>Answer</summary>

**B.**
</details>

**4.** How does Claude Code mostly handle a large codebase without loading it all up front?
- A. It loads every file at startup
- B. It refuses large repos
- C. It summarises the repo once and throws the summary away
- D. Just-in-time retrieval: it keeps lightweight references (paths) and uses tools like grep, glob and file reads to load what it needs, with CLAUDE.md loaded up front

<details><summary>Answer</summary>

**D.**
</details>

**5.** A multi-hour agent keeps losing track of which subtasks are done. What is a simple, robust technique?
- A. Structured note-taking: the agent maintains a progress or to-do file that it updates and re-reads
- B. Raise the temperature
- C. Repeat the task description every turn
- D. Restart every hour

<details><summary>Answer</summary>

**A.**
</details>

**6.** Before sending a very large prompt, you want to know its exact size so you can trim it if needed. What should you use?
- A. Character count divided by 2
- B. The token counting endpoint
- C. Send it and see if it fails
- D. `max_tokens`

<details><summary>Answer</summary>

**B.**
</details>

**7.** You use extended thinking in a tool-use loop. What must you do with the thinking blocks in the assistant turn that called the tool?
- A. Strip them out to save tokens
- B. Summarise them
- C. Pass them back unmodified together with the `tool_use` blocks when you return the tool results
- D. Move them into the system prompt

<details><summary>Answer</summary>

**C.** Thinking blocks must be preserved during tool use so the reasoning continues correctly.
</details>

**8.** A long-running agent's context fills mostly with old tool results, and you want the platform to remove stale ones automatically as it approaches a limit. Which feature fits?
- A. Context editing (automatic clearing of old tool results)
- B. The Batch API
- C. Citations
- D. Structured outputs

<details><summary>Answer</summary>

**A.**
</details>

**9.** A RAG system retrieves 40 chunks, and the answer often ignores the most relevant one, which lands in the middle. What helps?
- A. Retrieve 100 chunks
- B. Shuffle the chunks randomly
- C. Raise `max_tokens`
- D. Rerank, keep fewer high-relevance chunks, and put the best ones where attention is strongest (start, or next to the question)

<details><summary>Answer</summary>

**D.**
</details>

**10.** In a support agent, the customer changes their delivery address on turn 12. How should the case-facts block be updated?
- A. Explicitly. Replace the address field (e.g. through a dedicated `update_case_facts` step or tool), keep the block verbatim, and note when the change happened.
- B. Let the next summary pick it up
- C. Append a sentence to the system prompt
- D. Don't update it

<details><summary>Answer</summary>

**A.**
</details>

**11.** In a multi-turn chat, you want each new turn to reuse the cached earlier conversation. Where should the cache breakpoint go?
- A. Only on the system prompt, never moving
- B. On the last message of the conversation each turn, so the growing prefix is cached incrementally
- C. On the first user message only
- D. Breakpoints can't be used in chats

<details><summary>Answer</summary>

**B.**
</details>

**12.** You add `cache_control` to a 300-token system prompt and never see cache reads. What is the likely reason?
- A. The TTL is too short
- B. The prompt is below the model's minimum cacheable length, so it isn't cached
- C. Caching needs the Batch API
- D. You need two breakpoints

<details><summary>Answer</summary>

**B.**
</details>

**13.** Which changes invalidate an existing cached prefix? (Choose 2)
- A. Modifying a tool definition
- B. Appending a new user message after the breakpoint
- C. Editing the system prompt text
- D. Reading the `usage` field

<details><summary>Answer</summary>

**A and C.**
</details>

**14.** You need to cache tools, a system prompt, a large document *and* the growing conversation separately. How many breakpoints can one request use?
- A. 1
- B. 2
- C. Unlimited
- D. Up to 4

<details><summary>Answer</summary>

**D.**
</details>

**15.** Why is it good practice to put static content first and dynamic content last in a prompt?
- A. It's alphabetical
- B. The cache matches on the exact prefix, so stable content first maximises cache hits
- C. Dynamic content is ignored otherwise
- D. It reduces output tokens

<details><summary>Answer</summary>

**B.**
</details>

---

<details><summary>Quick answer key</summary>

1 B · 2 C · 3 B · 4 D · 5 A · 6 B · 7 C · 8 A · 9 D · 10 A · 11 B · 12 B · 13 A,C · 14 D · 15 B
</details>
