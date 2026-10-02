## Chapter 5: Optimizing and Operationalizing Generative Solutions — Practice Questions

**Q1.** A customer service chatbot is generating responses that are too creative and sometimes factually incorrect. The chatbot uses Azure OpenAI GPT-4 with temperature=0.9, top_p=0.95, and presence_penalty=0.6. Which single change will MOST effectively reduce hallucinations while maintaining helpful responses?

A) Reduce top_p to 0.1 and increase presence_penalty to 1.0
B) Increase max_tokens to 2000 and add frequency_penalty=0.8
C) Add a system message instructing the model to be accurate
D) Reduce temperature to 0.2 and keep all other parameters the same

<details>
<summary>Answer</summary>

**Correct: D**

Temperature directly controls randomness and creativity. High temperature (0.9) causes diverse, creative but potentially inaccurate outputs. Lowering to 0.2 makes outputs more deterministic and factual, which is critical for customer service. This single parameter change has the strongest impact on hallucination reduction.

**Why the others are wrong:**
- top_p=0.1 is too restrictive and would create repetitive responses; presence_penalty affects topic diversity, not factual accuracy
- max_tokens controls length, not accuracy; frequency_penalty reduces repetition within a response, not hallucinations
- System messages help but are less reliable than temperature reduction; the model can still ignore instructions with high temperature

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** An e-commerce company processes product descriptions through Azure OpenAI. The prompt template is 500 tokens, completion averages 150 tokens, and the system processes 100,000 requests daily. Azure OpenAI GPT-4 costs $0.03 per 1K prompt tokens and $0.06 per 1K completion tokens. Implementing prompt caching reduces prompt processing by 80%. What is the daily cost savings?

A) $900
B) $1,500
C) $1,200
D) $2,100

<details>
<summary>Answer</summary>

**Correct: C**

Current prompt cost: (500 tokens × 100,000 requests × $0.03) / 1000 = $1,500/day. With 80% caching: $1,500 × 0.80 = $1,200 saved. Completion tokens are not cached, so those costs remain unchanged. The calculation focuses only on prompt token savings.

**Why the others are wrong:**
- $900 incorrectly uses 60% reduction instead of 80%
- $1,500 would be the total current prompt cost, not the savings from caching
- $2,100 incorrectly includes completion token costs, which are not affected by prompt caching

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A legal document analyzer must extract specific clauses with 99.9% accuracy. The solution uses Azure OpenAI GPT-4 with temperature=0.1 for extraction. Despite the low temperature, 2% of extractions contain minor formatting errors or miss edge cases. Which hybrid architecture pattern BEST ensures the accuracy requirement?

A) Use chain-of-thought prompting to have the model explain its extraction logic
B) Increase max_tokens to allow the model more space to think through edge cases
C) Add presence_penalty=1.5 to encourage the model to cover all possible clause variations
D) Implement a deterministic regex-based validator that checks extracted clauses against known patterns and rejects invalid outputs

<details>
<summary>Answer</summary>

**Correct: D**

Hybrid architecture combining LLM + deterministic validation is the correct pattern for high-accuracy requirements. The regex validator provides 100% reliable checks for formatting and known patterns. LLMs alone, even at low temperature, cannot guarantee 99.9% accuracy due to their probabilistic nature. This pattern appears frequently on the exam.

**Why the others are wrong:**
- Chain-of-thought improves reasoning quality but does not guarantee accuracy; still probabilistic output
- max_tokens affects output length, not accuracy or edge case handling
- presence_penalty encourages topic diversity in generation; it does not improve extraction accuracy or handle edge cases

</details>

---

**Q4.** A RAG solution retrieves 10 documents (avg 400 tokens each) and generates responses using Azure OpenAI GPT-4. Latency breakdown: retrieval=800ms, prompt processing=1200ms, generation=2000ms, total=4000ms. Users complain about slow responses. Which optimization will reduce total latency by the LARGEST amount?

A) Implement semantic caching to reuse responses for similar queries, reducing generation time
B) Switch to GPT-4 Turbo which has 2x faster inference speed for generation
C) Reduce retrieved documents from 10 to 5, cutting prompt processing time in half
D) Move retrieval to parallel execution with prompt preparation

<details>
<summary>Answer</summary>

**Correct: C**

Prompt processing (1200ms) is 30% of total latency. Reducing documents from 10 to 5 cuts ~4000 tokens to ~2000 tokens, reducing prompt processing by approximately 600ms (half). This is the single largest reduction. The exam tests understanding of latency breakdown and token-based optimization.

**Why the others are wrong:**
- Semantic caching helps only for repeated queries; doesn't address first-time query latency which is the primary complaint
- GPT-4 Turbo reduces generation time (2000ms) by ~50% = 1000ms savings, but this is less than reducing documents (600ms prompt + reduced generation from smaller context)
- Retrieval and prompt preparation are sequential in RAG; retrieval output feeds prompt construction, so parallelization doesn't apply here

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** A content moderation system uses Azure OpenAI to classify user comments as safe/unsafe. The system requires 99.5% precision (no false positives). Currently, a GPT-4 model with temperature=0 achieves 97% precision. Which orchestration pattern BEST meets the requirement?

A) Use GPT-4 for initial classification, then use a small fine-tuned classification model to validate unsafe predictions, only flagging items both models agree on
B) Lower temperature to -0.5 to make outputs more deterministic
C) Use two different GPT-4 prompts and only flag items that both prompts classify as unsafe
D) Increase presence_penalty to 2.0 to encourage more conservative classifications

<details>
<summary>Answer</summary>

**Correct: A**

Multi-model orchestration with specialized models is the correct pattern. GPT-4 provides broad understanding, while a fine-tuned small model (like a BERT classifier) provides high-precision validation on the specific task. Only items flagged by both models are marked unsafe, increasing precision. This 'small model for classification, large model for understanding' pattern is heavily tested.

**Why the others are wrong:**
- Temperature range is 0.0 to 2.0; negative values are invalid
- Using two prompts with the same model provides minimal improvement; both suffer from the same model limitations
- presence_penalty affects topic diversity in generation, not classification precision

</details>

---

**Q6.** A medical diagnosis assistant generates treatment recommendations using Azure OpenAI GPT-4. To reduce liability, the system must validate that recommendations align with approved treatment protocols stored in a database. Which hybrid architecture component is MOST appropriate?

A) Increase temperature to 1.5 to generate more diverse treatment options
B) Use chain-of-thought prompting to have the model cite protocols in its reasoning
C) Add a second GPT-4 call to verify the first model's recommendations
D) A rules engine that checks each generated recommendation against protocol database and assigns confidence scores

<details>
<summary>Answer</summary>

**Correct: D**

Rules engine + LLM is the classic hybrid architecture for deterministic validation. The rules engine provides 100% reliable checks against the protocol database, assigns confidence scores, and can enforce hard constraints. This pattern separates creative generation (LLM) from deterministic validation (rules engine), which the exam tests frequently.

**Why the others are wrong:**
- Higher temperature increases randomness and reduces reliability, opposite of what's needed for safety-critical validation
- Chain-of-thought improves reasoning transparency but doesn't guarantee protocol compliance; the model can hallucinate protocol citations
- A second LLM call is still probabilistic; it cannot provide the deterministic validation required for medical liability concerns

</details>

---

**Q7.** A code generation tool uses Azure OpenAI to create Python functions. Developers report the model often generates the same variable names repeatedly within a function (e.g., 'temp', 'temp', 'temp'). Which parameter adjustment will MOST effectively encourage diverse variable naming?

A) Increase frequency_penalty from 0 to 1.2
B) Increase presence_penalty from 0 to 1.2
C) Decrease temperature from 0.7 to 0.2
D) Decrease top_p from 0.95 to 0.5

<details>
<summary>Answer</summary>

**Correct: A**

frequency_penalty specifically reduces repetition of tokens that have already appeared in the current completion. Higher values (1.2) penalize repeated tokens like 'temp', encouraging the model to use different variable names. This is the exact use case for frequency_penalty and is heavily tested.

**Why the others are wrong:**
- presence_penalty discourages repeating topics/themes across the response but doesn't specifically target repeated tokens within the response
- Lower temperature makes outputs more deterministic but doesn't specifically address token repetition
- Lower top_p restricts the token selection pool but doesn't specifically penalize repeated tokens

</details>

---

**Q8.** An AI solution generates marketing copy using Azure OpenAI GPT-4 with temperature=0.8, top_p=0.9, max_tokens=500. The prompt template uses role-based prompting: 'You are an expert copywriter. Write engaging product descriptions.' Cost per request: prompt=200 tokens, completion=450 tokens average. The business wants to reduce costs by 40% while maintaining quality. Which approach is MOST effective?

A) Implement semantic caching to reuse generated copy for similar products, reducing total API calls by 50%
B) Reduce max_tokens to 300, forcing more concise outputs while using the same model and parameters
C) Switch to GPT-3.5-Turbo which costs 90% less than GPT-4 but may reduce quality
D) Lower temperature to 0.3 to reduce the number of tokens the model considers, lowering costs

<details>
<summary>Answer</summary>

**Correct: A**

Semantic caching with 50% reduction directly achieves >40% cost savings (50% fewer API calls = 50% cost reduction) while maintaining quality since cached responses are from the same model. Marketing copy often has similar products, making caching highly effective. This optimization strategy is commonly tested.

**Why the others are wrong:**
- Reducing max_tokens from 500 to 300 saves only (450-300)×$0.06/1000 per request, which is minimal; also may hurt quality with incomplete descriptions
- GPT-3.5-Turbo achieves cost reduction but explicitly risks quality reduction, which violates the 'while maintaining quality' requirement
- Temperature affects randomness, not token count or cost; lowering it doesn't reduce costs, just makes outputs more deterministic

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

