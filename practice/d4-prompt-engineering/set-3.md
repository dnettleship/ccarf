# D4 Practice Set 3 — Structured Output, Schemas & Evals (20%)

15 original scenario-style questions (not real exam items). Give yourself **30 minutes**. Notes: [../../domains/d4-prompt-engineering.md](../../domains/d4-prompt-engineering.md)

---

**1.** A ticket classifier sometimes outputs "Billing issue", "billing" or "BILLING". Downstream code expects one exact value. What is the best fix?
- A. Normalise the strings with regexes afterwards
- B. Use a forced tool or structured output whose `category` field is an `enum` of the exact allowed values
- C. Ask nicely for lowercase
- D. Lower the temperature

<details><summary>Answer</summary>

**B.** Constrain the output to the exact set of allowed values.
</details>

**2.** You're extracting invoices, which have a variable number of line items. How should the schema represent them?
- A. An `array` of `object`s, each with typed fields (`description`, `quantity`, `unit_price`)
- B. One long string
- C. Fields `item1` to `item50`
- D. A separate API call per line

<details><summary>Answer</summary>

**A.**
</details>

**3.** Low-quality extractions should go to human review automatically. What should the schema include?
- A. Nothing extra
- B. A `notes` field for jokes
- C. Fewer fields
- D. Fields such as `confidence` and `evidence` (supporting quotes), which code can use to route uncertain records to review

<details><summary>Answer</summary>

**D.**
</details>

**4.** An extraction still fails validation after 3 retries with error feedback. What should the pipeline do?
- A. Retry forever
- B. Insert it anyway
- C. Stop retrying and send the record to a dead-letter or human-review queue with the errors attached
- D. Delete the source document

<details><summary>Answer</summary>

**C.** Limit retries, then escalate.
</details>

**5.** Your Python service already uses Pydantic models. What is a clean way to define and enforce the output schema?
- A. Write the JSON Schema by hand and never validate
- B. Generate the schema from the Pydantic model for structured outputs or tool input, then validate the response against the same model
- C. Use regexes
- D. Use free text and split on commas

<details><summary>Answer</summary>

**B.** One source of truth for both the schema and validation. Zod plays the same role in TypeScript.
</details>

**6.** Structured outputs guarantee schema-valid JSON. What do they *not* guarantee?
- A. That the JSON parses
- B. That required fields are present
- C. That the values are factually correct and consistent with the source
- D. That types match the schema

<details><summary>Answer</summary>

**C.** You still need semantic checks and evaluations.
</details>

**7.** The model keeps putting the shipping address into `billing_address`. What is a low-cost schema fix?
- A. Add clear `description`s to each field in the JSON schema (e.g. "Address printed under 'Bill to'; not the delivery address")
- B. Remove one of the fields
- C. Raise `max_tokens`
- D. Use a higher temperature

<details><summary>Answer</summary>

**A.** Field descriptions guide the model just like tool descriptions do.
</details>

**8.** How should you evaluate a classifier that has exactly one correct label per input?
- A. An LLM judge with a 10-point rubric
- B. Gut feel
- C. User complaints
- D. Code-based grading: exact match against the labelled answers, with accuracy and a confusion matrix

<details><summary>Answer</summary>

**D.** When there's an exact right answer, use code-based grading. It's fast and objective.
</details>

**9.** You use an LLM-as-judge to grade open-ended summaries. What makes it trustworthy?
- A. A clear, specific rubric, ideally with reasoning before the score, checked against a sample of human-graded examples
- B. Asking it "is this good?"
- C. Using the same prompt that generated the summary
- D. Grading at a high temperature

<details><summary>Answer</summary>

**A.**
</details>

**10.** What makes a good evaluation dataset?
- A. Only the five examples you used while writing the prompt
- B. Only easy cases
- C. A representative sample of real inputs, plus deliberate edge cases (missing fields, odd formats, adversarial inputs)
- D. Synthetic data that never resembles production

<details><summary>Answer</summary>

**C.**
</details>

**11.** A teammate "improves" the extraction prompt, and a field that used to work silently breaks. What process prevents this?
- A. Peer review by eye only
- B. Run the evaluation suite automatically on every prompt change (like CI tests) and block changes that regress
- C. Freeze the prompt forever
- D. Use a bigger model

<details><summary>Answer</summary>

**B.**
</details>

**12.** How do you measure how consistent a prompt is, not just whether it's accurate?
- A. Run each test case once
- B. Read one output
- C. Count tokens
- D. Run each test case several times and measure how much the outputs vary or how often each passes

<details><summary>Answer</summary>

**D.**
</details>

**13.** Which statements about using a forced tool call for extraction are true? (Choose 2)
- A. The structured result is read from the `tool_use` block's `input`
- B. Claude will also write a natural-language explanation before the tool call
- C. You don't have to execute any real function. The "tool" can exist only to carry the schema.
- D. It works only with Haiku

<details><summary>Answer</summary>

**A and C.**
</details>

**14.** Can you run schema-based (tool-use) extraction through the Message Batches API for a large, non-urgent backfill?
- A. No. Batches only support plain text.
- B. Yes. Batched requests support the same Messages API features, including tools, at lower cost.
- C. Only with streaming
- D. Only with extended thinking turned off

<details><summary>Answer</summary>

**B.**
</details>

**15.** Documents arrive as invoices, receipts or contracts, each needing a different schema. What is a neat single-call design?
- A. Three separate prompts, run every document through all three
- B. One giant schema containing every possible field
- C. Define one extraction tool per document type and set `tool_choice: {"type":"any"}`, so Claude classifies the document by choosing the right tool and fills its schema
- D. Ask Claude to output YAML

<details><summary>Answer</summary>

**C.**
</details>

---

<details><summary>Quick answer key</summary>

1 B · 2 A · 3 D · 4 C · 5 B · 6 C · 7 A · 8 D · 9 A · 10 C · 11 B · 12 D · 13 A,C · 14 B · 15 C
</details>
