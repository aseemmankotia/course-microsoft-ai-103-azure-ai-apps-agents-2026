# Answer Key — Practice Test 1

**Q1. Correct: B**

Azure AI Search vector indexing with RAG grounding is the correct pattern for knowledge-base scenarios requiring source attribution. This approach: (1) indexes documents in Azure AI Search with vector embeddings, (2) retrieves relevant chunks at query time, (3) grounds the LLM response in retrieved content, (4) automatically provides citations with document references. This is the standard Foundry pattern for minimizing hallucination while maintaining sub-2-second latency at scale.

---

**Q2. Correct: B**

Foundry agents support per-tool approval workflows even in autonomous mode. The correct approach is to run the agent in autonomous mode (allowing self-directed tool execution) but attach approval workflow policies to specific tools (SQL and email). The weather tool executes without approval. This granular control is the intended Responsible AI pattern for tool-access governance in Foundry agents.

---

**Q3. Correct: A**

Azure AI Content Safety provides pre-processing image moderation that analyzes visual content for violence, hate, sexual content, and self-harm BEFORE images reach the generative model. Setting severity thresholds (e.g., block medium-high violence) prevents harmful images from being processed at all. This is the correct Responsible AI pattern—input filtering is more effective than relying on model safety guardrails alone.

---

**Q4. Correct: A**

Azure AI Foundry hub provides centralized management for agents and models with built-in support for managed identity authentication and private endpoints. The correct pattern: (1) create a Foundry hub with managed identity enabled, (2) create environment-specific projects/workspaces under the hub, (3) configure private endpoint + VNet integration for the production workspace. This ensures production agents use managed identity (no API keys) and communicate over private network paths. This is the standard Foundry enterprise deployment architecture.

---

**Q5. Correct: A**

Calculate required TPM: 200 requests/hour × (1,500 input + 800 output tokens) = 200 × 2,300 = 460K tokens/hour. Convert to tokens/minute: 460K ÷ 60 = 7,667 TPM average. However, requests aren't evenly distributed—assuming burst patterns, apply 3.5× peak multiplier for safety: 7,667 × 3.5 ≈ 27K additional TPM needed. Current 50K + 27K = 77K TPM minimum. This calculation accounts for real-world request clustering during peak periods.

---

**Q6. Correct: C**

Azure AI Foundry agents support built-in trace logging that automatically captures the complete execution flow: user prompt → agent reasoning → tool calls (including parameters) → tool results → final response. Enabling trace logging with Azure Monitor Logs integration provides full provenance with timestamps, no custom code required. This is the standard Foundry feature for audit and observability—it captures exactly what's required (prompt, generated SQL in tool call, execution timestamp, result in tool response).

---

**Q7. Correct: A**

Azure AI Foundry agent runtime natively supports multimodal models like GPT-4o (which handles text, images, and can integrate with speech services) and provides built-in tool registration through Foundry Tools. This allows the agent to automatically handle different input modalities and execute registered tools (product search, ticket creation, escalation) through function calling without custom orchestration code. This is the intended architecture pattern for AI-103.

---

**Q8. Correct: D**

Exponential backoff with jitter is the recommended pattern for handling transient rate limits and prevents thundering herd problems. Dynamic quota management in Azure AI Foundry allows temporary quota increases during detected load spikes without permanently over-provisioning. This approach handles the 2-3 minute peak windows efficiently while keeping base costs low (not paying for unused 100K TPM capacity during normal 35K TPM usage). AI-103 emphasizes this elastic scaling pattern.

---

**Q9. Correct: A**

Agent oversight mode with 'approval-required' setting is specifically designed for AI-103 scenarios requiring human-in-the-loop control over agent actions. Tool access policies in Foundry allow granular control over which tools require confirmation. Content safety filters can detect and block PII/financial data patterns in outputs. This combination directly addresses both requirements (external API control and PII prevention) using native Foundry governance features.

---

**Q10. Correct: C**

Azure AI Search indexers support change tracking on blob storage data sources and can be configured with real-time triggers to detect document modifications and incrementally update the index immediately. This ensures the vector index reflects current data within minutes rather than waiting up to 6 hours. This is the standard pattern for maintaining index freshness that AI-103 tests, particularly for grounding scenarios requiring up-to-date knowledge.

---

**Q11. Correct: C**

Azure AI Foundry Evaluators are specifically designed for AI-103 to assess LLM application quality across dimensions like groundedness (accuracy of grounding), relevance (response quality), and safety (content compliance). Evaluators can be integrated into CI/CD pipelines to run automated assessments on test datasets, generating scores that serve as quality gates before production deployment. This directly addresses the requirement to validate prompt quality, grounding, and safety in an automated pipeline.

---

**Q12. Correct: D**

System-assigned managed identity with RBAC role assignment (Storage Blob Data Reader) provides keyless authentication—no secrets to manage or rotate. DefaultAzureCredential in Azure SDK automatically uses the managed identity when running in Container Apps. This is the recommended pattern for AI-103 and follows Azure security best practices for workload identity, eliminating credential management entirely.

---

**Q13. Correct: C**

RAG (Retrieval-Augmented Generation) is the standard pattern for grounding LLM responses in specific document collections. Azure AI Search provides vector indexing and semantic search to retrieve relevant chunks, which are then passed as context to the LLM along with conversation history. This enables citations, handles token limits efficiently, maintains context across turns, and is the recommended Foundry pattern for knowledge-grounded applications.

---

**Q14. Correct: B**

In Foundry multi-agent orchestration, the coordinator agent uses custom tools (functions) to invoke specialist agents. Each tool call represents delegating to a specialist, the response contains the result, and the coordinator maintains orchestration state (which specialist did what, success/failure) in its conversation context. This enables tracking, error handling, and reassignment logic within the agent framework.

---

**Q15. Correct: C**

Self-critique (also called self-reflection or chain-of-verification) is an effective technique for reducing fabrication. The LLM generates a response, then explicitly verifies each claim against the retrieved context in a second reasoning step. If fabrications are detected, it regenerates. This leverages the model's reasoning capabilities and is a recommended Foundry pattern for improving factual grounding.

---

**Q16. Correct: A**

Azure AI Foundry agents include a built-in Code Interpreter tool specifically designed for executing generated code in isolated sandboxes. It supports timeout configuration, handles isolation automatically, integrates natively with the agent framework, and is the recommended approach for code execution in agentic workflows. This avoids custom infrastructure and uses Foundry's managed capability.

---

**Q17. Correct: C**

Semiautonomous workflows with approval flows are the Foundry pattern for scenarios requiring human oversight before actions are taken. The agent generates the recommendation autonomously, but the workflow pauses at an approval gate where a human reviews and either approves or rejects. This is built into Foundry's agent framework specifically for compliance, high-stakes decisions, and safety requirements.

---

**Q18. Correct: D**

GPT-4 Turbo has significantly faster inference speed than base GPT-4 while maintaining comparable quality, and it also reduces cost. For 10,000 daily documents with high token counts, this model selection change directly reduces actual latency per request. This is a standard Foundry optimization: choosing the right model variant for latency requirements.

---

**Q19. Correct: A**

Azure AI Search with vector embeddings is the recommended approach for RAG in Azure AI Foundry. Hybrid search (combining vector and keyword) provides both semantic understanding and exact match capabilities with optimized query performance. The text-embedding-ada-002 model (or newer embedding models) creates the vector representations needed for semantic retrieval.

---

**Q20. Correct: B**

Setting 'strict' mode on function calling ensures the model adheres more closely to the function schema and reduces hallucination. A clear, specific function description helps the model understand when the function is required. This is the proper way to configure reliable function calling in Azure AI Foundry agents.

---

**Q21. Correct: A**

Groundedness measures whether the generated content is factually supported by the source material (the original ticket). This directly addresses fabrication/hallucination detection, which is a core evaluation capability in Azure AI Foundry for RAG and summarization applications.

---

**Q22. Correct: B**

In multi-agent systems, each agent should maintain its own conversation context scoped to its responsibility to prevent context pollution and reduce token usage. The coordinator maintains orchestration state separately. This is the recommended pattern in Azure AI Foundry for multi-agent solutions.

---

**Q23. Correct: D**

Latency in LLM calls is heavily influenced by prompt token count. With 3,500 tokens of context, the model must process all of that before generating the 200-token response. Prompt compression is the most direct way to reduce latency and is a key optimization technique covered in the AI-103 exam. A 60% reduction in prompt tokens can significantly reduce processing time.

---

**Q24. Correct: D**

This implements proper safeguards and approval flows for autonomous agents. The validation occurs at the function schema/execution level (not relying on the model's judgment), and the workflow pattern with human-in-the-loop is the recommended approach in Azure AI Foundry for semiautonomous agents with approval requirements.

---

**Q25. Correct: D**

Option B correctly implements an orchestrated workflow with conditional logic, which is the appropriate pattern when tool execution must follow a specific sequence with decision points. Azure AI Foundry supports inspecting tool results and conditionally executing subsequent tools based on those results, enabling the required business logic (only generate letter if approved). This is a core exam topic for building production-grade agent workflows.

---

**Q26. Correct: C**

Option A addresses both issues with production-appropriate solutions. Groundedness evaluation detects fabrication by comparing generated content to retrieved chunks (core exam topic). Metadata filtering at the vector search layer prevents discontinued products from being retrieved (exam tests knowledge of vector store filtering). Model reflection/self-critique is a tested optimization technique that adds minimal latency. This combination maintains the 3-second SLA while improving quality.

---

**Q27. Correct: D**

Option D targets the actual problems revealed by token analytics and tracing. Prompt caching (a key exam topic for cost optimization) eliminates the repeated cost of the 60% system prompt tokens across requests. Optimized few-shot examples improve output quality while reducing prompt size. max_tokens controls output costs directly. This achieves significant cost reduction (caching the large system prompt is the biggest win) without compromising quality. The exam tests understanding of prompt caching, token analytics interpretation, and parameter tuning.

---

**Q28. Correct: A**

Azure AI Foundry's image generation capabilities specifically support mask-based inpainting workflows for removing unwanted objects and provide built-in watermark injection features required for brand protection. This is the primary service for image generation and editing tasks in AI-103.

---

**Q29. Correct: D**

GPT-4o provides grounded visual understanding for generating evidence-based alt-text descriptions, and visual content filters in Azure Content Safety specifically detect unsafe visual content including medical imagery that violates policies. This combination addresses both accessibility and safety requirements.

---

**Q30. Correct: A**

Indirect prompt injection via embedded text in images/video is a specific AI-103 exam topic under Responsible AI for multimodal content. Multimodal content filters analyze visual elements including embedded text that could manipulate generation behavior, addressing this attack vector directly.

---

**Q31. Correct: B**

Pro-mode Content Understanding pipelines support component and region identification optimized for large-scale video analysis. Adaptive frame sampling reduces processing costs by intelligently selecting frames while maintaining accuracy for object detection across long videos.

---

**Q32. Correct: B**

Prohibited-symbol flagging in Azure Content Understanding is specifically designed to detect brand/logo violations in images. Combined with pre-generation prompt analysis, this prevents generation of prohibited content and detects violations in output, implementing comprehensive brand protection.

---

**Q33. Correct: B**

Prompt-driven image modifications with reference images and generation controls are specifically designed for editing workflows that preserve certain elements while modifying others. This allows maintaining room architecture while adding furniture based on user descriptions.

---

**Q34. Correct: C**

Azure AI Foundry agents with structured output mode allow you to define a JSON schema and extract entities directly through generative prompting while built-in content safety filters detect PII/PHI. This provides an integrated solution meeting all requirements in a single agent configuration.

---

**Q35. Correct: B**

Batch processing with GPT-4o-mini using a single multi-task prompt minimizes costs by: 1) using the cheaper mini model, 2) processing in batch mode (50% discount), 3) one API call per email instead of three. A well-designed prompt can extract all three outputs in one pass, staying well under budget for 50K emails.

---

**Q36. Correct: B**

Custom speech models with phrase lists directly address domain-specific vocabulary issues by training the speech recognition on the specific product names and their pronunciations. This is the standard solution for improving accuracy on specialized terminology in Azure AI speech services.

---

**Q37. Correct: C**

Azure Translator supports custom translation models trained with glossaries for domain-specific terminology and has built-in features to preserve entities like numbers and special terms. Calling the custom model endpoint from a Foundry agent provides the required integration while maintaining translation accuracy.

---

**Q38. Correct: D**

Azure AI Content Safety is specifically designed for detecting hate speech, violence, self-harm and other harmful content categories at scale. However, it focuses on safety issues, not nuanced tone analysis. Adding a generative prompt for tone analysis (aggressive vs. constructive) complements the safety detection with the subjective judgment needed for tone assessment.

---

**Q39. Correct: B**

Detecting frustration from voice characteristics (tone, pitch, rate) requires analyzing the audio signal itself, not just the transcribed words. Multimodal reasoning with audio-capable models allows the agent to process paralinguistic features directly from the audio input to assess emotional state beyond what text conveys.

---

**Q40. Correct: C**

Azure AI Content Understanding (2026) provides multimodal document analysis combining OCR, layout analysis, and field extraction in a unified pipeline. It produces clean markdown or structured outputs optimized for RAG grounding. Azure AI Search with vector embeddings enables semantic/hybrid search, and registering the index as an agent tool provides the proper integration path for Azure AI agents to retrieve grounded information.

---

**Q41. Correct: D**

Document Layout skill (part of Azure AI Content Understanding) analyzes complex layouts including multi-column formats and preserves structural hierarchy. Following with OCR captures text including footer annotations, custom skill cleans formatting artifacts, Text Merge reconstructs proper document flow, and embedding with metadata preservation ensures accurate grounding with layout context retained.

---

**Q42. Correct: C**

Azure AI Content Understanding (2026) natively handles multimodal content including medical images and documents. Integrated vectorization with GPT-4o supports unified embeddings across modalities. Hybrid search combines vector similarity (for visual matching) and keyword search (for clinical terms). A unified index simplifies agent integration and ensures coherent grounding across image and text content.

---

**Q43. Correct: A**

Azure AI Speech batch transcription provides accurate transcription with speaker diarization (speaker identification). Azure AI Language topic modeling extracts discussed topics. Azure AI Search with custom skills can index transcript chunks with timestamp metadata, enabling precise temporal retrieval. Hybrid search supports both semantic topic queries and keyword matching for exact phrase retrieval, all integrated into agent tools.

---

**Q44. Correct: D**

Diagrams contain spatial relationships that standard text chunking destroys. Document Layout skill identifies diagram regions and preserves their boundaries. Alternatively, Image Analysis skill can generate natural language descriptions of diagrams (e.g., 'flowchart showing process X with steps A, B, C') which embed better than OCR'd labels. Configuring chunking to respect layout boundaries prevents splitting diagram context across chunks.

---

**Q45. Correct: A**

Custom Web API skills integrate seamlessly into Azure AI Search skillset pipelines, enabling real-time enrichment during indexing. The skill can query external databases, merge data, and pass enriched documents to subsequent skills like Azure OpenAI Embedding within the same execution graph. This approach keeps all enrichment logic in the skillset, ensuring consistency and proper dependency ordering.

---

