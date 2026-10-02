# Answer Key — Practice Test 2

**Q1. Correct: A**

User-assigned managed identities with federated credentials enable keyless authentication for both the SQL database (via Azure AD authentication) and the external OAuth2 API (via workload identity federation). This eliminates credential storage entirely, meets the security policy requirement, allows granular RBAC assignment per agent capability, and integrates directly with AI Foundry agent tool authentication configurations. Federated credentials enable OAuth2 flows without client secrets.

---

**Q2. Correct: A**

Hybrid search combining reduced-dimension vectors (768d from distilled models like MiniLM provide 40-60% dimensionality reduction with minimal accuracy loss) with keyword filtering dramatically reduces search latency by offloading exact-match queries to inverted indexes while maintaining semantic capability. Index aliasing enables atomic index swaps—build the new index in parallel, then switch the alias when ready—achieving zero-downtime updates and eliminating the 45-minute stale window. This directly addresses both the latency and freshness requirements at scale.

---

**Q3. Correct: B**

Human-in-the-loop mode (agent oversight mode with approval gates) directly satisfies the explicit approval requirement before diagnostic actions. Azure AI Foundry's trace logging with prompt flow runtime captures all reasoning steps, tool calls, and model interactions—2557 days equals 7 years retention in Log Analytics. GPT-4o multimodal grounding with citations enabled provides provenance metadata showing which image regions and training patterns influenced outputs. Immutable storage for conversation transcripts ensures long-term auditability. This is the only option that properly addresses all three requirements with native Foundry capabilities.

---

**Q4. Correct: A**

Provisioned throughput (PTU) provides predictable costs and eliminates per-token billing, which at this scale (250K requests/day × 9,200 tokens average = 2.3B tokens/month) typically offers 40-60% cost savings over pay-as-you-go at high volume. 200 PTUs distributed across three regions provides ~66-67 PTU per region with automatic geographic distribution, directly addressing the P95 latency issue through proximity routing and eliminating queue contention. Front Door health probes enable intelligent failover. Prompt optimization to reduce output tokens by 30% (360 fewer tokens × 250K requests = 90M fewer tokens/month) compounds savings without sacrificing capability. This is the only option that addresses cost, latency, and scaling systematically.

---

**Q5. Correct: D**

Hybrid retrieval (vector + BM25 keyword) is specifically effective for financial data where exact numerical matching is critical—BM25 ensures precise figures are retrieved even if semantic embedding doesn't capture them. Query expansion with financial synonyms improves recall for domain-specific terminology. Chunk-level attributions force the model to ground specific claims in specific source text, directly addressing the hallucination problem. The verification tool (agent self-validation via re-querying) enables the agent to double-check numerical facts before output—a proven pattern for reducing hallucinations in high-stakes domains. This is the only option that combines retrieval improvement, attribution enforcement, and automated verification.

---

**Q6. Correct: B**

Annotated content filters in Azure OpenAI allow custom keyword lists (competitors, political terms) to be enforced AT GENERATION TIME—the model's output is filtered token-by-token, blocking violations before the response is completed. This is the only native capability that enables real-time blocking during generation rather than post-hoc detection. The dual-agent safety architecture provides domain-specific policy evaluation (GPT-4 can understand nuanced brand safety rules that keyword matching misses). Streaming with sentence-level checks enables mid-generation abort, preventing partial violations from being published. Dynamic blocklist updates address emerging terms during live events. This combines generation-time filtering, semantic policy evaluation, and adaptive term detection.

---

**Q7. Correct: A**

Foundry's built-in trace logging with provenance metadata automatically captures tool invocations, data flows, and decision chains without custom code. Agent oversight modes provide native support for human-in-the-loop approval workflows based on conditions. Streaming to Azure Monitor provides enterprise-grade retention and compliance reporting. This is the Azure-native, minimal-code approach.

---

**Q8. Correct: B**

GPT-4o natively handles text, vision, and audio in a single model, eliminating orchestration complexity. PTUs provide reserved capacity for baseline 50K/day load with guaranteed low latency and predictable costs. Standard deployments handle spike traffic with pay-per-token pricing only when needed. This hybrid approach optimizes cost (PTU discount on baseline) while meeting 5-second SLA during spikes.

---

**Q9. Correct: B**

1000-token chunks are too coarse for precise medical facts like dosages—critical information gets diluted. 256-token chunks with overlap ensure dosage details appear in focused contexts. Top-k=3 may miss relevant documents (0.65 similarity is low). Increasing to 10 and adding reranking ensures the LLM sees the most relevant grounded context, directly addressing the root cause of hallucinations from poor retrieval.

---

**Q10. Correct: D**

This implements true keyless authentication for all three services using managed identity. 'Cognitive Services OpenAI User' grants appropriate OpenAI access, SQL user mapped to managed identity enables native AAD auth to SQL, and 'Storage Blob Data Contributor' scoped to just the container provides least-privilege blob write access. No secrets, no keys, no rotation needed.

---

**Q11. Correct: D**

The key signal is safety filter blocks jumping from 2% to 18% while retrieval similarity remains high (0.82). The agent is retrieving old regulations, generating confident responses, but users know the information is outdated and challenge the responses. When users push back, the agent may generate conflicting or uncertain content that trips safety filters. Knowledge base staleness in regulated industries directly causes quality degradation. Financial regulations change frequently—4 weeks is enough for significant policy updates.

---

**Q12. Correct: B**

GitHub Actions with environment protection rules provides native two-reviewer requirement enforcement before Production deployment. Environment secrets safely manage endpoint configs per environment. Bicep parameter files enable infrastructure-as-code with audit trails through Git history. Branch protection on main ensures all changes are reviewed. GitHub environment protection rules integrate approval workflow directly into deployment pipeline with full audit logs. This is the most modern, secure, and automated approach.

---

**Q13. Correct: A**

A Validator Agent acts as an orchestration safeguard between agents, explicitly evaluating intermediate outputs (retrieval results) before they propagate downstream. This is the recommended pattern for multi-agent workflows where one agent's output quality directly impacts another's performance. The validator uses a separate evaluation model to score relevance, filtering low-quality results before the Response Agent consumes them.

---

**Q14. Correct: D**

This combines three proven anti-hallucination strategies: (1) Phi-3 small models are specifically trained to be more factual and less prone to fabrication than larger models on knowledge-sparse queries, (2) RAG with SEC filings provides grounded, authoritative data that the model retrieves rather than invents, and (3) model reflection adds a self-verification step. For companies with limited public data, RAG ensures the model only summarizes what exists in verified sources.

---

**Q15. Correct: B**

This combines two critical safeguards for agent function-calling: (1) Explicit prompt guidance reduces model confusion about when to invoke functions—models often over-call functions without clear instructions, and (2) semiautonomous approval flows are the recommended pattern in Foundry for sensitive operations like database writes, giving users control before execution. This directly addresses the compliance concern while maintaining agent efficiency for read operations.

---

**Q16. Correct: C**

AI Search boosting profiles with recency scoring is the native, performant solution for time-sensitive RAG scenarios. Exponential decay weighting gives progressively lower scores to older documents while still allowing them if they're highly relevant. Semantic ranking (L2 ranking) further improves relevance by understanding query intent. This is built into the search layer, requires no additional latency, and solves the problem at the retrieval source rather than post-processing.

---

**Q17. Correct: A**

Tool-augmented flows are the recommended pattern for multi-step reasoning where each step requires different data or processing. The agent can dynamically decide when to search statutes vs. retrieve cases vs. analyze patterns, adapting to query complexity. Conversation memory maintains context across tool calls without rigid sequential constraints. This gives the model agency to explore reasoning paths while keeping steps grounded in retrieved data, directly addressing the depth problem in steps 3-4 where analysis requires iterative exploration.

---

**Q18. Correct: C**

Hybrid LLM plus rules-engine designs are the optimal pattern when some decisions have deterministic criteria and others need reasoning. Known failure patterns (e.g., temperature > X for Y minutes) should use rules for 100% precision and low latency. LLMs handle edge cases where context matters. This dramatically reduces false positives (the core problem) by removing LLM uncertainty from clear-cut cases, while keeping LLM flexibility for ambiguous scenarios. Operator feedback enables continuous evaluation of both components.

---

**Q19. Correct: D**

AI-103 emphasizes Foundry's native orchestration capabilities for multi-agent solutions. Semiautonomous approval flows are specifically designed for regulated scenarios requiring human oversight, and the exam tests knowledge of persisting conversation context (not just metadata) for audit compliance. Cosmos DB with immutable logs is the recommended pattern for regulatory audit trails in Foundry architectures.

---

**Q20. Correct: C**

AI-103 tests optimization strategies balancing quality and latency. GPT-4o-mini delivers 60-80% latency reduction versus GPT-4o while maintaining strong instruction-following for structured tasks like product descriptions. The hybrid LLM-plus-rules-engine pattern is explicitly covered in the exam as best practice for scenarios requiring guaranteed factual accuracy on specific fields. Few-shot examples improve output structure consistency, and the rules-engine post-processor provides deterministic validation that catches fabricated specs the LLM might generate.

---

**Q21. Correct: C**

AI-103 heavily emphasizes tool schema design as the primary determinant of agent tool-calling reliability. The exam tests that well-documented schemas with clear when-to-use guidance and parameter examples dramatically improve function-calling accuracy. Many agent failures stem from insufficient max_tokens, causing truncated tool-call JSON. The exam distinguishes this from prompt engineering (adding to system prompt) or architectural changes (switching models), which are secondary factors.

---

**Q22. Correct: D**

AI-103 extensively covers Foundry's evaluation capabilities as the recommended approach for systematic A/B testing of prompts and models. The exam tests knowledge that Foundry's evaluation pipeline integrates LLM-as-judge for relevance assessment, built-in grounding evaluators for fabrication detection, and trace analytics for token consumption—all three required metrics. The A/B evaluation feature is specifically designed for comparing variants against labeled datasets at scale, which maps to the 10,000-interaction requirement.

---

**Q23. Correct: D**

AI-103 tests RAG optimization patterns where retrieval quality directly impacts generation precision. The exam emphasizes that when the LLM uses most retrieved documents (6.3 of 10), the issue is low-quality retrieval, not generation. Reducing retrieved documents focuses the LLM on highest-quality sources. Raising the semantic ranker threshold filters marginal matches. Cross-encoder reranking is the gold-standard Foundry pattern for improving retrieval precision, explicitly covered in RAG optimization sections—it rescores documents based on query-document interaction, catching relevance nuances vector search misses.

---

**Q24. Correct: C**

AI-103 extensively covers autonomous vs. semiautonomous agent workflows as a core exam topic. The exam tests that Foundry's semiautonomous workflows with approval gates are purpose-built for exactly this scenario: actions requiring human oversight based on business impact. Confidence scoring for tool-call decisions is a native Foundry capability that maps to the 95% threshold requirement. Routing traces to Azure Monitor with custom dimensions enables the full traceability and compliance analysis the question requires. This architecture uses Foundry's intended features rather than external orchestration.

---

**Q25. Correct: D**

Semiautonomous workflows in Azure AI Foundry allow agents to execute multiple steps autonomously but pause at designated approval gates for human intervention. Configuring an approval gate after income verification with conversation tracking preserves the agent's state and context, allowing seamless resumption after approval. All three APIs remain as agent tools with proper schemas for function calling, maintaining the native agent architecture. This approach directly uses Foundry's built-in capabilities for hybrid autonomous/human-in-the-loop workflows.

---

**Q26. Correct: D**

Azure AI Foundry's built-in groundedness evaluation specifically measures whether generated responses are supported by the retrieved context, using a GPT-4 judge model to perform nuanced comparison. This is purpose-built for detecting fabrication/hallucination in RAG applications. The evaluation pipeline can process all 500 queries programmatically, produces quantitative scores, and provides detailed breakdowns identifying which responses lack grounding. Groundedness is one of the core RAG evaluation metrics Foundry provides alongside relevance and coherence, designed exactly for this fabrication detection scenario.

---

**Q27. Correct: A**

The key insight is that Phi-3 validation takes 60% of latency despite minimal token usage, suggesting the validation logic itself may be unnecessarily complex for an LLM. A hybrid rules-engine design handles the majority of validation cases (80%) through fast, deterministic code that executes in milliseconds, only invoking Phi-3 for genuinely complex cases where rules can't decide. This directly targets the latency bottleneck. Moving validation after code generation prevents wasted code generation on inputs that would fail validation. Caching Phi-3 results by constraint pattern further reduces repeated validation overhead. This approach demonstrates the exam's emphasis on hybrid LLM-plus-rules architectures for optimization.

---

**Q28. Correct: C**

Azure AI Foundry image generation services (2026) support watermark embedding during generation as a responsible AI control. Azure AI Content Understanding pro-mode pipelines can analyze images for synthetic media characteristics including watermark presence, providing the required verification workflow. This is the native, integrated approach for generation + responsible AI validation.

---

**Q29. Correct: D**

Azure AI Foundry video editing services (emphasized in 2026 AI-103) provide native support for mask-based editing (object removal) and prompt-driven modifications (background replacement). Video segmentation enables frame-range targeting. This is the minimal, purpose-built solution using the platform's video editing workflows without requiring separate frame extraction or custom orchestration.

---

**Q30. Correct: C**

Azure AI Content Understanding provides specialized captioning capabilities optimized for detailed descriptions (including charts and diagrams). Azure Content Safety's multimodal content filters (2026 emphasis) specifically address indirect prompt injection attacks via embedded text in images, detecting jailbreak patterns and malicious instructions hidden in visual content. This pairing directly addresses both accessibility and responsible AI security requirements.

---

**Q31. Correct: B**

Azure AI Content Understanding (2026 emphasis) includes prohibited-symbol flagging as a responsible AI capability, specifically designed for detecting restricted visual content in real-time video. Azure AI Foundry video editing provides mask-based workflows that can automatically blur detected regions. This combination uses purpose-built services for the exact use case (prohibited content + video editing) and can meet latency requirements with proper resource allocation.

---

**Q32. Correct: A**

Azure AI Foundry image generation (2026) integrates responsible AI controls including prohibited-symbol flagging (for brand logos) and automatic watermarking. Azure AI Content Safety multimodal filters provide comprehensive detection for unsafe visual content, patient-identifiable features (faces, tattoos, ID badges), and policy violations. This is the most complete, integrated responsible AI stack for controlled image generation with built-in safeguards.

---

**Q33. Correct: C**

Azure AI Content Safety multimodal content filter (2026 emphasis) is specifically designed to detect indirect prompt injection attacks via embedded text in images. It analyzes both visual and textual modalities together to identify adversarial patterns, including jailbreak attempts disguised in images. This is the purpose-built service for this exact threat vector and can handle 5,000 images/hour with proper resource allocation while maintaining high accuracy.

---

**Q34. Correct: B**

GPT-4o with structured output (response_format parameter enforcing JSON schema) provides the most efficient solution: it handles PII detection, redaction, and structured formatting in a single model call, meets sub-second latency requirements, integrates natively as a Foundry Tool, and includes built-in content safety that can be configured for PII detection. The 2026 AI-103 exam emphasizes generative AI approaches over traditional NLP pipelines for text analysis tasks.

---

**Q35. Correct: C**

Azure AI Speech speech translation provides real-time multilingual-to-English translation with conversation context preservation. GPT-4o agents in Foundry can natively handle sentiment analysis through instructions and have built-in Content Safety integration for automated violation logging. This minimizes custom code while meeting all requirements. The exam tests understanding of service integration in Foundry agent architectures.

---

**Q36. Correct: B**

GPT-4o with response_format (structured output) and temperature=0.0 provides deterministic structure enforcement. Grounding with citations ensures extracted figures reference actual transcript text, minimizing hallucination. JSON-to-XML conversion is trivial post-processing. The 2026 exam emphasizes structured output capabilities and grounding techniques for high-accuracy extraction tasks.

---

**Q37. Correct: A**

Azure AI Speech continuous recognition provides low-latency real-time transcription. GPT-4o excels at nuanced emotion classification from text through prompting (no training needed) and can detect frustration/anger beyond simple positive/negative sentiment. This approach balances accuracy, latency, and implementation simplicity for production use. The exam tests understanding of speech-to-text integration with LLM-based analysis.

---

**Q38. Correct: C**

GPT-4o handles abstractive summarization with word-count constraints and structured extraction in a single model call (via response_format). Built-in content filters support custom blocklists for PHI terms. Azure AI Foundry batch endpoints provide the throughput needed (10,000 docs in 24hrs) with managed scaling. This approach minimizes pipeline complexity while meeting all requirements. The exam emphasizes end-to-end Foundry solutions for document processing.

---

**Q39. Correct: B**

Azure AI Speech multi-language detection with auto-detect handles intra-utterance code-switching by recognizing both languages in a single transcription stream. GPT-4o easily understands mixed-language text and can detect primary language through conversation patterns without custom code. This leverages built-in capabilities with minimal development. The exam tests understanding of modern speech recognition features for multilingual scenarios.

---

**Q40. Correct: C**

Content Understanding (part of Azure AI Foundry) provides production-grade semantic chunking and clean grounded representations specifically designed for RAG scenarios. Document Intelligence layout model extracts tables with structure preserved and handles handwritten text. Azure AI Search custom skills orchestrate the enrichment pipeline, connecting Document Intelligence → Content Understanding → vector indexing in a single managed flow without custom chunking code.

---

**Q41. Correct: A**

Vector profiles (containing HNSW algorithm config) are mandatory for vector search fields. Custom skills in the indexer skillset enable calling the Azure ML endpoint during enrichment. Semantic ranker requires L2 (Standard) tier or higher and is invoked via queryType=semantic. Hybrid queries combine 'vector' and 'search' parameters in a single request—both ranking signals merge automatically.

---

**Q42. Correct: A**

Custom extraction models (formerly custom neural models) are purpose-built for form field extraction with labeled training. They directly recognize field labels, extract associated values, and identify selection marks (checkboxes) with field-level confidence. 50+ samples provide sufficient training for high accuracy on consistent form layouts. Output structured JSON maps directly to agent working memory schemas, and training time is measured in hours, not weeks.

---

**Q43. Correct: D**

Azure Video Indexer natively handles speaker diarization (speaker labels), keyframe extraction, on-screen text OCR (video OCR), and generates time-coded insights—all in one service. Outputs JSON with timestamps linking transcripts to frames. Azure AI Search custom skills can enrich this data, creating a unified searchable index. Agents query once and get correlated text + visual citations with exact timestamps. Handles 2-hour 4K videos within service limits.

---

**Q44. Correct: A**

Azure AI Language custom text classification provides trained classification models with per-text-record pricing (typically $0.001–0.002 per record) vs GPT-4 token-based pricing ($0.03–0.06 per 1K tokens for multiple clause extractions). Integrated as an Azure AI Search skill, it processes during indexing without per-query LLM calls. Classification models handle clause categorization at 98%+ accuracy when properly trained. Cost reduction >60% is achievable: ~$0.002 per doc vs ~$0.015–0.03 GPT-4 cost. Latency drops to <1 second per document.

---

**Q45. Correct: B**

Hybrid search in Azure AI Search combines BM25 text search with vector similarity in a single query. Text fields (product description) use standard text search. Vector field stores CLIP image embeddings (1024-dim) with HNSW for efficient approximate nearest neighbor search. Filters apply before retrieval for performance. RRF (default in hybrid queries) automatically merges text and vector ranking signals into a unified score. This is the native, production-ready pattern for multi-modal RAG.

---

