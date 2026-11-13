# D4 Practice Set 2 — Prompting Techniques (20%)

15 original scenario-style questions (not real exam items). Give yourself **30 minutes**. Notes: [../../domains/d4-prompt-engineering.md](../../domains/d4-prompt-engineering.md)

---

**1.** Contract reviews read as generic and miss legal risks. Which change most directly improves the domain focus?
- A. Use the `system` parameter to give Claude a role, e.g. "You are a senior commercial lawyer reviewing supplier contracts for a UK retailer"
- B. Use a higher temperature
- C. Shorten the contract
- D. Ask for bullet points

<details><summary>Answer</summary>

**A.** Role prompting through the system prompt sharpens expertise and tone.
</details>

**2.** You tell Claude "Never use ellipses," and it still uses them sometimes. Which rewrite works better?
- A. "NEVER EVER use ellipses!!!"
- B. "Your response will be read aloud by a text-to-speech engine, so never use ellipses, because the engine can't pronounce them."
- C. Remove the instruction
- D. Add five more rules

<details><summary>Answer</summary>

**B.** Explaining *why* a rule exists helps Claude apply it and generalise from it.
</details>

**3.** You ask, "Can you suggest some changes to improve this function?" and Claude lists suggestions but doesn't edit anything. You wanted it to make the edits. What is the fix?
- A. Raise `max_tokens`
- B. Use a bigger model
- C. Be explicit: "Refactor this function to improve readability. Make the changes directly."
- D. Add examples of suggestions

<details><summary>Answer</summary>

**C.** Recent models follow instructions precisely, so ask for exactly the action you want.
</details>

**4.** You run the same instructions over thousands of different customer emails. How should you structure the prompt?
- A. A template: fixed instructions plus variables (e.g. `{{EMAIL}}`) filled per request, which also makes testing and caching easier
- B. Rewrite the prompt by hand for each email
- C. Put all the emails in one prompt
- D. Put the instructions after each email, with random wording

<details><summary>Answer</summary>

**A.**
</details>

**5.** You want step-by-step reasoning but must parse only the final answer programmatically. What should you do?
- A. Ban reasoning
- B. Parse the last line and hope
- C. Use a higher temperature
- D. Ask for the reasoning in `<thinking>` tags and the final answer in `<answer>` tags, then extract the `<answer>` contents

<details><summary>Answer</summary>

**D.** Tags separate the reasoning from the answer so you can parse reliably.
</details>

**6.** Your five few-shot examples are all short, formal and about billing. Outputs now copy that too closely, even for technical questions. What is the fix?
- A. Remove the examples
- B. Make the examples diverse, covering different topics, lengths and edge cases, so Claude learns the pattern rather than the surface details
- C. Add 50 more billing examples
- D. Lower the temperature

<details><summary>Answer</summary>

**B.**
</details>

**7.** For high-stakes reports you want Claude to catch its own mistakes. Which approach is structured and reliable?
- A. Ask it to "be accurate"
- B. Raise `max_tokens`
- C. A chain: generate the draft, then a review prompt checks it against explicit criteria, then a final prompt revises it using the review
- D. Run it once at temperature 0

<details><summary>Answer</summary>

**C.** Self-correction chains catch errors a single pass misses.
</details>

**8.** For questions about a long report, answers drift from the source. What technique grounds them?
- A. Ask Claude to first extract the relevant quotes into `<quotes>` tags, then answer based on those quotes
- B. Summarise the report first and discard the original
- C. Ask for a shorter answer
- D. Use a higher temperature

<details><summary>Answer</summary>

**A.**
</details>

**9.** You pass ten documents at once, and Claude confuses which fact came from which document. What helps?
- A. Concatenate them without separators
- B. Wrap each one in `<document>` tags with `<source>` and other metadata (title, date) and `<document_content>`
- C. Send them in ten separate API calls with no context
- D. Remove the titles

<details><summary>Answer</summary>

**B.**
</details>

**10.** A compliance team needs every claim in an answer tied to an exact passage in the supplied PDFs. Which API feature fits best?
- A. Prompt caching
- B. The Batch API
- C. Citations enabled on document content blocks, so responses carry references to the exact source text
- D. Extended thinking

<details><summary>Answer</summary>

**C.**
</details>

**11.** With extended thinking on, you wrote a rigid 20-step procedure for how Claude must think, and the results got worse. What does Anthropic's guidance suggest?
- A. Add more steps
- B. Start with high-level instructions to think thoroughly about the problem, and let Claude decide how. Add specific guidance only where needed.
- C. Turn thinking off
- D. Put the steps in the user message in capitals

<details><summary>Answer</summary>

**B.**
</details>

**12.** In a long-context prompt, where should the specific task instruction usually go?
- A. Before the documents only
- B. In the middle of the documents
- C. In a separate API call
- D. After the documents, at the end of the prompt

<details><summary>Answer</summary>

**D.** Putting the query last improves results with long inputs.
</details>

**13.** Which go in the `system` prompt rather than the user turn? (Choose 2)
- A. Claude's role and persona
- B. Today's specific customer email to process
- C. Persistent rules that apply to every turn (tone, constraints, output conventions)
- D. The user's one-off question

<details><summary>Answer</summary>

**A and C.**
</details>

**14.** User-supplied text is inserted into your prompt, and some users try "ignore previous instructions". What prompt-level measure helps, alongside controls in code?
- A. Wrap the untrusted input in clear tags (e.g. `<user_input>`) and tell Claude to treat that content as data to analyse, not as instructions
- B. Remove the system prompt
- C. Raise the temperature
- D. Paste the user text into the system prompt

<details><summary>Answer</summary>

**A.** It helps, but it isn't sufficient on its own. Pair it with least privilege and validation.
</details>

**15.** You need a solid first-draft prompt for a new task quickly, written in Anthropic's recommended style. Which tool fits?
- A. The Batch API
- B. Token counting
- C. The Messages API `stop_sequences` parameter
- D. The prompt generator and improver in the Claude Console

<details><summary>Answer</summary>

**D.**
</details>

---

<details><summary>Quick answer key</summary>

1 A · 2 B · 3 C · 4 A · 5 D · 6 B · 7 C · 8 A · 9 B · 10 C · 11 B · 12 D · 13 A,C · 14 A · 15 D
</details>
