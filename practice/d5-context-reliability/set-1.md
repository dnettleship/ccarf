# D5 Practice Set 1 — Context Management & Reliability (15%)

15 original scenario-style questions (not real exam items). Give yourself **30 minutes**. Notes: [../domains/d5-context-reliability.md](../../domains/d5-context-reliability.md)

---

**1.** A 150k-token prompt contains a critical refund policy halfway through, and Claude often ignores it. What is the best fix?
- A. Raise `max_tokens`
- B. Move the policy to the start of the prompt (or repeat the key constraint near the question)
- C. Lower the temperature
- D. Add more documents

<details><summary>Answer</summary>

**B.** Information in the middle of a long context is used less reliably ("lost in the middle").
</details>

**2.** In 50-turn support conversations, order numbers and agreed amounts drift because the history is repeatedly summarised. What should you do?
- A. Keep a structured case-facts block (IDs, amounts, decisions) that is carried verbatim and updated explicitly, outside the summarised history
- B. Summarise more often
- C. Use a smaller model
- D. Delete old turns without summarising

<details><summary>Answer</summary>

**A.** Exact facts must never go through repeated summarisation.
</details>

**3.** A coding agent's context fills up with old, large file contents it has already used. What is the best technique?
- A. Restart the task from scratch
- B. Raise the temperature
- C. Use a larger `max_tokens`
- D. Clear or replace old tool results, keeping a reference (such as the file path) so the content can be fetched again if needed

<details><summary>Answer</summary>

**D.** This is tool-result clearing combined with just-in-time retrieval.
</details>

**4.** An agent works on a migration across several days and sessions. How should it keep track of progress?
- A. Rely on the context window
- B. Put everything into the system prompt
- C. Write structured progress notes to an external file or the memory tool, and read them back at the start of each session
- D. Re-run all prior steps each day

<details><summary>Answer</summary>

**C.** External, structured memory survives across sessions.
</details>

**5.** In what order does prompt caching build its prefix?
- A. `tools` → `system` → `messages`
- B. `messages` → `system` → `tools`
- C. `system` → `messages` → `tools`
- D. The order doesn't matter

<details><summary>Answer</summary>

**A.** A change at any point invalidates the cache for everything after it.
</details>

**6.** `cache_read_input_tokens` is always 0. The code builds the tool list from a Python `set` on each request. What is the likely cause?
- A. Caching only works with Opus
- B. The TTL is too long
- C. The tool order changes between requests, so the prefix differs. Use a stable, deterministic order.
- D. You used too few breakpoints

<details><summary>Answer</summary>

**C.** The cached prefix has to match exactly.
</details>

**7.** Users send a request against the same large document about every 20 minutes, and the default cache keeps missing. What is the best fix?
- A. Send a dummy request every minute
- B. Use the 1-hour cache TTL for that breakpoint
- C. Remove the document
- D. Use the Batch API

<details><summary>Answer</summary>

**B.** The default 5-minute TTL expires between requests. The 1-hour option costs more to write but gets hits here.
</details>

**8.** A response's `usage` shows `cache_creation_input_tokens: 0` and `cache_read_input_tokens: 48000`. What does that mean?
- A. The cache failed
- B. 48k tokens were written to the cache
- C. Caching is disabled
- D. 48k tokens were served from the cache (a cache hit, billed at the much cheaper read rate)

<details><summary>Answer</summary>

**D.**
</details>

**9.** Each night you need to classify 50,000 support emails, and results are needed by morning. What is the most cost-effective approach?
- A. The Message Batches API (about 50% cheaper, asynchronous, finishes within 24 hours), plus caching the shared prompt
- B. 50,000 streaming requests
- C. Extended thinking on every request
- D. Opus with real-time calls

<details><summary>Answer</summary>

**A.** The work isn't urgent and is high-volume, which suits batch processing.
</details>

**10.** At peak times you see intermittent `529 overloaded` and `429` errors. What is the right handling?
- A. Fail immediately and show an error to the user
- B. Retry immediately in a tight loop
- C. Retry with exponential backoff and jitter, respect `retry-after`, and fall back (to another model or a queue) if retries run out
- D. Switch off the error logs

<details><summary>Answer</summary>

**C.** These are transient errors, so back off and retry.
</details>

**11.** Your service keeps retrying a request that returns `400 invalid_request_error`. What should change?
- A. Retry more times
- B. Don't retry. Log it, then fix the malformed request (e.g. the schema, roles or parameters).
- C. Switch models
- D. Add jitter

<details><summary>Answer</summary>

**B.** A 400 is a client error, so retrying the same request won't help.
</details>

**12.** Requests with a very large `max_tokens` sometimes fail with HTTP timeouts before the response arrives. What is the fix?
- A. Lower the quality
- B. Use the Batch API for an interactive feature
- C. Remove tools
- D. Use streaming

<details><summary>Answer</summary>

**D.** Streaming keeps the connection active, and the SDKs recommend it for long outputs.
</details>

**13.** A `charge_card` tool timed out, the agent retried, and the customer was charged twice. What prevents this?
- A. Idempotency keys, so repeating the same logical charge has no extra effect
- B. Removing retries entirely
- C. A longer timeout only
- D. Telling Claude not to retry in the prompt

<details><summary>Answer</summary>

**A.** Tools with side effects must be safe to retry.
</details>

**14.** Which model-selection choices are sound? (Choose 2)
- A. Opus for every subagent, regardless of task
- B. Haiku for the hardest long-horizon reasoning
- C. Haiku for high-volume, simple classification or routing
- D. A stronger model (e.g. Opus) as orchestrator, with cheaper or faster models as workers on narrower tasks

<details><summary>Answer</summary>

**C and D.**
</details>

**15.** A support agent escalates to humans, who say they can't tell what has already been tried. Which handoff design fixes this?
- A. Forward the entire raw transcript
- B. A structured handoff: case facts, the customer's goal, actions taken with their results, the current blocker, options with a recommendation, and a confidence level
- C. A sentiment score only
- D. Ask the customer to explain the issue again

<details><summary>Answer</summary>

**B.**
</details>

---

<details><summary>Quick answer key</summary>

1 B · 2 A · 3 D · 4 C · 5 A · 6 C · 7 B · 8 D · 9 A · 10 C · 11 B · 12 D · 13 A · 14 C,D · 15 B
</details>
