## Chapter 3: Building Generative Applications with Foundry — Practice Questions

**Q1.** A company is building a customer support chatbot that must answer questions about their product documentation. The documentation is updated weekly with new features. The solution must minimize response latency and provide accurate, up-to-date answers. Which approach BEST meets these requirements?

A) Fine-tune a GPT-4 model on the product documentation and redeploy weekly
B) Implement RAG with Azure AI Search vector store, embed documentation chunks, and use hybrid retrieval with semantic ranking
C) Use few-shot prompting by including 10-15 product documentation examples in the system message
D) Deploy a GPT-3.5-turbo model with temperature=0 and include full documentation in each prompt

<details>
<summary>Answer</summary>

**Correct: B**

RAG architecture separates knowledge (stored in searchable vector database) from the model, enabling real-time updates without redeployment. Hybrid retrieval (keyword + vector) maximizes relevance. This is the exam's preferred pattern for dynamic knowledge scenarios.

**Why the others are wrong:**
- Fine-tuning requires expensive retraining and redeployment for each update. Not suitable for weekly changes. Exam tests understanding that fine-tuning is for behavior/style, not dynamic knowledge.
- Few-shot examples are limited by context window (8K-128K tokens). Cannot fit entire documentation. Exam tests knowing few-shot is for task demonstration, not knowledge storage.
- Context window limits prevent including full documentation. GPT-3.5-turbo has 16K token limit. Even GPT-4-turbo (128K) becomes slow and expensive with large contexts. Temperature=0 doesn't solve knowledge retrieval.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A developer is using the Azure AI Foundry SDK to call a deployed GPT-4 model for code generation. Users report that responses sometimes repeat the same phrases and lack creativity. The model is currently configured with temperature=0.2 and top_p=0.1. What configuration change would BEST address this issue?

A) Add frequency_penalty=2.0 and presence_penalty=2.0
B) Decrease max_tokens to 500 and add stop sequences
C) Increase temperature to 0.8 and top_p to 0.95
D) Switch to a fine-tuned model trained on creative code examples

<details>
<summary>Answer</summary>

**Correct: C**

Temperature controls randomness (0=deterministic, 2=very random). Top_p (nucleus sampling) controls diversity of token selection. For creative tasks, temperature 0.7-0.9 and top_p 0.9-0.95 are recommended. Current settings (0.2, 0.1) produce repetitive, low-diversity outputs.

**Why the others are wrong:**
- Penalty values max at 2.0, but this is overly aggressive and causes incoherent output. Exam tests knowing penalties range 0-2, with 0.5-1.0 typical. Setting both to 2.0 breaks output quality.
- Max_tokens limits response length but doesn't affect creativity or repetition. Stop sequences control where generation ends, not diversity. Wrong parameter for the problem.
- Fine-tuning changes model behavior/style, not output diversity. Diversity is controlled by inference parameters (temperature, top_p). Exam tests: use parameters first, fine-tuning for specialized tasks only.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** An application uses RAG to answer questions about medical research papers. The Azure AI Search index contains 50,000 documents with vector embeddings. Retrieval returns 10 chunks but responses often fabricate citations not present in the retrieved content. Which technique would BEST reduce fabrication?

A) Increase the number of retrieved chunks from 10 to 50 to provide more context
B) Add a system message instructing the model to answer ONLY using provided context and cite specific chunk IDs
C) Set temperature to 0 to make responses fully deterministic
D) Use a fine-tuned model trained on medical papers to improve domain knowledge

<details>
<summary>Answer</summary>

**Correct: B**

Grounding the model with explicit instructions to use only provided context is the primary fabrication mitigation technique. Including chunk IDs enables citation verification. System messages set behavior constraints. This is the exam's RAG best practice pattern.

**Why the others are wrong:**
- More chunks increase context window cost and noise. LLMs can still fabricate regardless of context quantity. Optimal chunk count is 5-15. Exam tests knowing that quality of grounding instructions matters more than chunk quantity.
- Temperature=0 makes output deterministic but doesn't prevent fabrication. The model can still confidently generate plausible but false information. Exam tests: temperature controls randomness, not factuality.
- Fine-tuning adds domain knowledge but increases fabrication risk (model 'knows' information not in retrieved context). RAG specifically avoids fine-tuning for knowledge. Exam tests: RAG for facts, fine-tuning for behavior.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** A development team is building a multi-step workflow where a GPT-4 model must: 1) analyze a customer query, 2) call an API to check inventory, 3) generate a response with product availability. Using Azure AI Foundry SDK, which implementation pattern is CORRECT?

A) Use function calling with a tool definition for the inventory API, allowing the model to request function execution when needed
B) Single chat completion call with a system message containing the full workflow logic and API documentation
C) Chain three separate chat completion calls: query analysis, inventory check, response generation, passing context between calls
D) Fine-tune the model on examples of the complete workflow including inventory data

<details>
<summary>Answer</summary>

**Correct: A**

Function calling (tools) is the Foundry SDK's native pattern for model-driven tool invocation. Model decides when to call functions, application executes, model integrates results. This is the exam's preferred agentic pattern for external data integration.

**Why the others are wrong:**
- Single call cannot execute external APIs. Model would fabricate inventory data or return incomplete responses. Exam tests: LLMs cannot execute code, need function calling for external actions.
- Chaining manual calls works but is inefficient and error-prone. Model cannot determine when inventory check is needed. Function calling automates this orchestration and is the best practice tested on exam.
- Fine-tuning cannot include real-time inventory data. Model would learn outdated patterns. Exam tests: dynamic data requires runtime retrieval (RAG or function calling), not fine-tuning.

</details>

---

**Q5.** You are implementing chain-of-thought prompting for a math word problem solver. Which prompt structure demonstrates CORRECT chain-of-thought technique?

A) System: 'Solve step-by-step, showing your reasoning.' User: 'Problem: A store has 23 apples and sells 7. How many remain? Let's approach this systematically:'
B) System: 'You are a math tutor.' User: 'Solve: A store has 23 apples and sells 7. How many remain?'
C) System: 'You are an expert.' User: 'Examples: Q: 10-3? A: 7. Q: 15-5? A: 10. Now solve: 23-7?'
D) System: 'Answer concisely.' User: 'Calculate: 23 apples - 7 apples sold = ?'

<details>
<summary>Answer</summary>

**Correct: A**

Chain-of-thought requires explicit instruction to show reasoning ('step-by-step') and a prompt structure that invites reasoning ('Let's approach this systematically'). System message sets expectation, user prompt triggers the reasoning pattern. This matches exam's CoT definition.

**Why the others are wrong:**
- No chain-of-thought instruction. Model would likely give direct answer without showing reasoning steps. Exam tests: CoT requires explicit reasoning instruction, not just role assignment.
- This is few-shot prompting (providing examples), not chain-of-thought. Few-shot demonstrates task format. CoT shows reasoning process. Exam differentiates these techniques.
- Direct calculation prompt. No reasoning instruction. Model gives answer without showing work. Exam tests: CoT is about reasoning process visibility, not answer accuracy.

</details>

---

**Q6.** A company is evaluating a RAG-based customer service chatbot before production deployment. They need to measure response quality across 500 test cases. Which evaluation approach BEST aligns with generative AI assessment best practices?

A) Use GPT-4 as a judge to score each response on relevance (1-5), coherence (1-5), and fabrication presence (yes/no), with human spot-checks on 10% of cases
B) Measure average response time and token count per response
C) Calculate BLEU and ROUGE scores comparing generated responses to reference answers
D) Track user satisfaction ratings collected after deployment over 30 days

<details>
<summary>Answer</summary>

**Correct: A**

LLM-as-judge is the current best practice for scalable generative AI evaluation. GPT-4 can assess subjective qualities (relevance, coherence) consistently across large datasets. Human validation on subset ensures judge quality. Fabrication detection is critical for RAG. This is exam's preferred evaluation pattern.

**Why the others are wrong:**
- Response time and token count measure performance, not quality. Exam tests understanding that generative AI evaluation requires content assessment (relevance, coherence, fabrication), not just metrics.
- BLEU/ROUGE compare surface-level token overlap with references. Inappropriate for open-ended generation where many valid responses exist. Exam tests: BLEU/ROUGE for translation/summarization, not chat/RAG.
- Post-deployment user ratings are useful but not sufficient for pre-deployment evaluation. Need systematic quality assessment before release. Exam tests: evaluate before deploy, not after.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** A developer is using the Azure AI Foundry SDK to implement streaming chat completions. The application must display tokens as they are generated and calculate total token usage after completion. Which code pattern is CORRECT?

A) Call client.complete() with stream=False, parse response.choices[0].message.content into tokens, display progressively
B) Use client.stream() which returns AsyncIterator, await each chunk, extract delta content, track tokens manually
C) Enable streaming in deployment configuration, call client.complete() normally, SDK automatically streams
D) Call client.complete() with stream=True, iterate chunks, concatenate deltas, read usage from final chunk

<details>
<summary>Answer</summary>

**Correct: D**

Foundry SDK chat completion streaming uses stream=True parameter. Response is iterator of chunks with delta content. Final chunk contains usage metadata (prompt_tokens, completion_tokens, total_tokens). This is the documented SDK pattern tested on exam.

**Why the others are wrong:**
- stream=False returns complete response at once, no progressive display. Cannot tokenize and display progressively from non-streaming response. Exam tests: must use stream=True for streaming.
- client.stream() is not a Foundry SDK method. SDK uses client.complete(stream=True). Manual token tracking is unnecessary as SDK provides usage in final chunk. Exam tests exact SDK method names.
- Streaming is request-level (stream=True), not deployment config. SDK doesn't auto-stream without explicit parameter. Exam tests: streaming must be requested per call.

</details>

---

**Q8.** An enterprise application uses Azure AI Foundry to generate customer emails. Compliance requires logging all model inputs, outputs, token usage, and latency for audit purposes. Which SDK feature provides this capability with minimal custom code?

A) Enable Application Insights integration and configure custom telemetry for each completion call
B) Implement custom middleware to intercept requests and responses, logging to Azure Monitor
C) Use Foundry SDK's built-in tracing feature which automatically captures prompts, completions, token counts, and performance metrics
D) Enable verbose logging on the client and parse log files for required data

<details>
<summary>Answer</summary>

**Correct: C**

Azure AI Foundry SDK includes native tracing/observability that captures all inference details (prompts, completions, tokens, latency, metadata) automatically. Integrated with Azure ML workspace logging. This is the exam's recommended observability pattern for Foundry applications.

**Why the others are wrong:**
- Application Insights works but requires significant custom code to capture model-specific data (prompts, token counts). Foundry SDK tracing provides this out-of-box. Exam tests: use built-in features before custom solutions.
- Custom middleware works but is unnecessary when SDK provides built-in tracing. More code = more maintenance. Exam tests preference for SDK native features over custom implementations.
- Verbose logging captures debug info but not structured inference telemetry. Log parsing is fragile. SDK tracing provides structured, queryable data. Exam tests: structured observability over log parsing.

</details>

---

