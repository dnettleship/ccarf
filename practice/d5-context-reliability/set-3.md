# D5 Practice Set 3 — Production Reliability & Operations (15%)

15 original scenario-style questions (not real exam items). Give yourself **30 minutes**. Notes: [../../domains/d5-context-reliability.md](../../domains/d5-context-reliability.md)

---

**1.** Your service hits rate limits at peak. In which dimensions are API rate limits typically measured?
- A. Only requests per day
- B. Requests per minute, plus input and output tokens per minute
- C. Characters per second
- D. There are no rate limits

<details><summary>Answer</summary>

**B.** Smooth out bursts with queues, and back off on 429s.
</details>

**2.** During a provider-side overload, your premium feature still needs to answer. What is a sensible resilience pattern?
- A. Return HTTP 500 to users
- B. Retry forever
- C. Disable the feature permanently
- D. After bounded retries, fall back to an alternative model or degraded mode, and queue anything that can wait

<details><summary>Answer</summary>

**D.**
</details>

**3.** How should you configure the official SDK client for a latency-sensitive endpoint?
- A. Set an explicit request timeout and a sensible `max_retries`, based on your latency budget
- B. Leave everything at defaults and hope
- C. Set the timeout to 24 hours
- D. Set `max_retries` to 1000

<details><summary>Answer</summary>

**A.**
</details>

**4.** A user reports a bad response from last Tuesday, and you need to investigate it with Anthropic support. What should you have logged?
- A. Nothing
- B. Only the user ID
- C. The `request-id` from each API response, along with your own trace ID, the model, `stop_reason` and usage
- D. The full API key

<details><summary>Answer</summary>

**C.**
</details>

**5.** The LLM provider has an outage, and your support bot can't answer. What is the best degraded behaviour?
- A. Show a stack trace
- B. Pretend to answer with canned guesses
- C. Loop retries until it recovers
- D. Tell the user, and create a ticket in the human queue with the conversation so far

<details><summary>Answer</summary>

**D.**
</details>

**6.** One downstream tool fails on every call for ten minutes, and agents keep hammering it. Which pattern helps?
- A. A circuit breaker: after N failures, stop calling the tool for a cool-down period and return a clear "temporarily unavailable" error
- B. More parallel calls
- C. Remove error handling
- D. Use a bigger model

<details><summary>Answer</summary>

**A.**
</details>

**7.** An agent drafts customer refunds. Before any refund executes, what reliability layer should exist?
- A. None. Trust the model.
- B. A post-hoc monthly audit only
- C. Output validation and guardrails in code (amount limits, account checks, policy rules) before the side effect happens
- D. A prompt asking for honesty

<details><summary>Answer</summary>

**C.**
</details>

**8.** You submitted 10,000 requests in a message batch. How do you match each result to its input?
- A. By position, since results always come back in order
- B. By the `custom_id` you set on each request, because results aren't guaranteed to be in order
- C. By timestamp
- D. By the response length

<details><summary>Answer</summary>

**B.**
</details>

**9.** Some requests in a large batch didn't finish before the batch's processing window ended. What happens, and what should you do?
- A. They expire. Resubmit those requests in a new batch.
- B. They run forever
- C. They're silently dropped with no status
- D. They're billed twice

<details><summary>Answer</summary>

**A.** Check each result's type (succeeded, errored, expired and so on) and handle it.
</details>

**10.** Monthly API spend jumped unexpectedly. Which controls help prevent and detect this? (Choose 2)
- A. Track the `usage` tokens per feature and set spend alerts or limits
- B. Remove all logging
- C. Set appropriate `max_tokens` caps and iteration limits on agents
- D. Always use the largest model

<details><summary>Answer</summary>

**A and C.**
</details>

**11.** Claude sometimes passes a negative `quantity` to `create_order`. Where must this be caught?
- A. Only in the prompt
- B. Nowhere. The database will cope.
- C. In the system prompt in capital letters
- D. In the tool itself: validate the inputs on the server side before executing, and return an actionable `is_error` if they're invalid

<details><summary>Answer</summary>

**D.** Never trust tool inputs from the model without validation.
</details>

**12.** How should you test an agent whose behaviour is non-deterministic?
- A. Run each scenario once and ship
- B. An evaluation suite run with multiple trials per scenario, tracking success rate (and variance) over time
- C. Test only in production
- D. Make it deterministic by turning off tools

<details><summary>Answer</summary>

**B.**
</details>

**13.** Which practices make tool calls safe to retry? (Choose 2)
- A. Idempotency keys passed to downstream APIs
- B. Retrying side-effecting calls blindly without checks
- C. Check-before-create (e.g. look for an existing ticket with the same external reference before creating a new one)
- D. Random delays only

<details><summary>Answer</summary>

**A and C.**
</details>

**14.** A streaming response starts with HTTP 200, then an `overloaded_error` event arrives partway through. What should your client do?
- A. Assume success because the status was 200
- B. Display half an answer as final
- C. Crash
- D. Handle error events in the stream as well as the HTTP status, and retry or recover appropriately

<details><summary>Answer</summary>

**D.** With streaming, errors can arrive after the 200 status.
</details>

**15.** A six-hour data-migration agent crashes at hour five. What design would have avoided redoing everything?
- A. Nothing can help
- B. A bigger context window
- C. External checkpoints of progress and state (a progress file or database, completed-item markers) so a new run can resume where it stopped
- D. Running it at temperature 0

<details><summary>Answer</summary>

**C.**
</details>

---

<details><summary>Quick answer key</summary>

1 B · 2 D · 3 A · 4 C · 5 D · 6 A · 7 C · 8 B · 9 A · 10 A,C · 11 D · 12 B · 13 A,C · 14 D · 15 C
</details>
