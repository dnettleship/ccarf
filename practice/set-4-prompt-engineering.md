# Practice Set 4 — Prompt Engineering & Structured Output (D4, 20%)

15 original scenario-style questions (not real exam items). Give yourself **30 minutes**. Notes: [../domains/d4-prompt-engineering.md](../domains/d4-prompt-engineering.md)

---

**1.** A summarisation prompt produces output in a different format and tone on every run. What is the most effective single change?
- A. Set `temperature` to 1
- B. Add 3–5 diverse examples of the desired output, wrapped in `<example>` tags
- C. Write the instructions in capital letters
- D. Shorten the prompt

<details><summary>Answer</summary>

**B.** Few-shot examples are the strongest lever for consistent format and tone.
</details>

**2.** You need answers about an 80-page contract. Where should things go in the prompt?
- A. The contract at the top, the instructions and question at the end, and a request to quote the relevant clauses before answering
- B. The question first, then the contract
- C. Split the contract across several system prompts
- D. Put the question in the middle of the contract

<details><summary>Answer</summary>

**A.** Long documents go at the top and the query at the end. Asking for quotes first grounds the answer.
</details>

**3.** A prompt mixes instructions, a pasted contract and example outputs, and Claude sometimes treats example text as instructions. What fixes this?
- A. A higher `max_tokens`
- B. More capital letters
- C. Separating the sections with XML tags such as `<instructions>`, `<contract>` and `<examples>`
- D. Removing the examples

<details><summary>Answer</summary>

**C.** XML tags clearly separate the different kinds of content.
</details>

**4.** "Do not use markdown" is often ignored. What is the better instruction?
- A. Repeat "NO MARKDOWN" three times
- B. Lower the temperature
- C. Add a markdown parser
- D. Say what to do instead, e.g. "Write in flowing prose paragraphs," and write the prompt itself in prose too

<details><summary>Answer</summary>

**D.** Positive instructions work better than prohibitions, and the prompt's own style influences the output.
</details>

**5.** Claude makes mistakes on multi-step quantitative reasoning. What is the most direct improvement?
- A. Give it room to think: enable extended thinking with a budget, or ask for step-by-step reasoning in `<thinking>` tags before the answer
- B. Ask for a shorter answer
- C. Raise the temperature
- D. Remove the context

<details><summary>Answer</summary>

**A.** Reasoning before answering helps complex problems, at the cost of latency and tokens.
</details>

**6.** A nightly pipeline extracts 30 fields into a database, and the output must always parse. What should you use?
- A. "Respond only in JSON" in the prompt
- B. Markdown tables
- C. Structured outputs, or a forced tool call whose `input_schema` is the target schema
- D. Prefilling with `[`

<details><summary>Answer</summary>

**C.** These are the most reliable options. A prompt instruction alone can drift.
</details>

**7.** The output is valid against the schema, but sometimes the line items don't add up to `total`. What should you do?
- A. Accept it, since the schema passed
- B. Validate the business rules in code, then retry with the specific error fed back ("line items sum to 412.50 but total is 421.50"), up to N times, then escalate
- C. Remove the `total` field
- D. Raise `max_tokens`

<details><summary>Answer</summary>

**B.** A schema guarantees structure, not correctness.
</details>

**8.** Some source documents have no `po_number`, and the model invents plausible-looking ones. What is the best schema change?
- A. Make it required, with a regex
- B. Add more examples where the PO number is present
- C. Lower the temperature
- D. Make it nullable and instruct the model to return null when it isn't present, optionally with an evidence quote when it is

<details><summary>Answer</summary>

**D.** Required fields push the model to make up values.
</details>

**9.** A `category` enum doesn't cover new ticket types, so the model forces them into the wrong categories. What fixes this?
- A. Add `"other"` to the enum, plus a free-text `category_detail` field
- B. Remove the enum
- C. Add 50 more enum values
- D. Use a higher temperature

<details><summary>Answer</summary>

**A.** Give the model a valid way out, then review the "other" cases to evolve the enum.
</details>

**10.** A team prefills the assistant turn with `{` to get JSON, then turns on extended thinking and the requests fail. What should they do?
- A. Prefill with `[` instead
- B. Use structured outputs or a tool with a schema, because prefill isn't supported with extended thinking
- C. Turn on streaming
- D. Put the `{` in the system prompt

<details><summary>Answer</summary>

**B.** Prefill and extended thinking are incompatible. Get the structure from a schema-based approach instead.
</details>

**11.** A documentation Q&A bot confidently answers questions the docs don't cover. Which combination reduces this most?
- A. A higher temperature and longer answers
- B. A bigger model only
- C. Explicitly allow "I don't know — not in the docs," require supporting quotes or citations, and restrict answers to the provided context
- D. Remove the docs and rely on the model's knowledge

<details><summary>Answer</summary>

**C.** Give the model permission to abstain, and make it ground its answers.
</details>

**12.** You need research → outline → draft → edit, with a human able to inspect each stage. Which approach fits?
- A. One giant prompt
- B. Parallel voting
- C. Routing
- D. Prompt chaining: one focused prompt per stage, with each output passed to the next

<details><summary>Answer</summary>

**D.** Each step gets full attention and can be inspected.
</details>

**13.** How should you tell whether a prompt change is actually an improvement?
- A. Define success criteria first, then run an evaluation set graded by code, an LLM with a rubric, or humans, and compare versions
- B. Try three examples by eye
- C. Ask Claude whether the new prompt is better
- D. Ship it and wait for complaints

<details><summary>Answer</summary>

**A.** Base prompt iteration on evaluations.
</details>

**14.** Which statements about output consistency are true? (Choose 2)
- A. `temperature: 0` guarantees identical output every time
- B. Lowering the temperature reduces variability but doesn't guarantee identical output
- C. Temperature matters more than examples for format consistency
- D. Few-shot examples combined with a schema are stronger levers for consistency than temperature

<details><summary>Answer</summary>

**B and D.**
</details>

**15.** In Anthropic's AI Fluency "4D" framework, what does **Discernment** refer to?
- A. Deciding which tasks to hand to AI
- B. Writing clear prompts
- C. Critically evaluating AI outputs, processes and behaviour
- D. Taking responsibility and being transparent about AI use

<details><summary>Answer</summary>

**C.** A is Delegation, B is Description, and D is Diligence.
</details>

---

<details><summary>Quick answer key</summary>

1 B · 2 A · 3 C · 4 D · 5 A · 6 C · 7 B · 8 D · 9 A · 10 B · 11 C · 12 D · 13 A · 14 B,D · 15 C
</details>
