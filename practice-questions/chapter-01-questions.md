## Chapter 1: Planning and Managing Azure AI Solutions — Practice Questions

**Q1.** A financial services company needs to build a chatbot that answers compliance questions from a 50,000-page document repository. The solution must minimize latency for frequently asked questions while keeping costs under $500/month. The company expects 100,000 queries per month with 95% being repetitive questions. Which deployment approach BEST meets these requirements?

A) Deploy GPT-4o with provisioned throughput of 100 PTUs and implement caching layer
B) Deploy Llama 3.1 405B on real-time endpoint with vector search only
C) Deploy GPT-4o mini serverless with semantic caching and Azure AI Search hybrid retrieval
D) Deploy GPT-4o serverless with provisioned throughput reserved capacity

<details>
<summary>Answer</summary>

**Correct: C**

GPT-4o mini serverless is cost-effective at ~$0.15 per 1M input tokens vs GPT-4o at $2.50 per 1M. With 95% repetitive queries, semantic caching reduces actual LLM calls to ~5,000/month. Hybrid retrieval (vector + keyword) improves accuracy for compliance documents. Total cost: ~$75-150/month well under budget.

**Why the others are wrong:**
- 100 PTUs cost ~$30,000/month — far exceeds $500 budget. PTUs are for sustained high throughput (>100 requests/sec), not this scenario.
- Llama 3.1 405B on real-time endpoint costs ~$5-8/hour compute plus inference costs. Monthly: $3,600-5,800. Vector-only search misses exact compliance term matches that keyword search catches.
- Invalid configuration — provisioned throughput and serverless are mutually exclusive deployment types. Cannot combine both for same model.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** An e-commerce platform processes product images to generate descriptions, extract text from receipts, and answer customer questions about orders. The solution requires processing 10,000 images daily and handling 50,000 text queries. Which model selection strategy is MOST cost-effective?

A) Use GPT-4o for all tasks since it handles both vision and text natively
B) Use GPT-4o for vision tasks, deploy Phi-3 small language model for text queries via serverless endpoint
C) Deploy Llama 3.2 Vision 90B for images and Llama 3.2 3B for text on shared real-time endpoint
D) Use Azure Computer Vision for OCR, GPT-4o mini for descriptions, and GPT-4o for customer questions

<details>
<summary>Answer</summary>

**Correct: B**

GPT-4o excels at vision tasks (image→text descriptions, complex OCR). Phi-3-mini/medium handles straightforward text queries at 1/10th the cost of GPT-4o (~$0.10 vs $1.00 per 1M tokens). Serverless deployment means no idle compute costs. Customer questions likely pattern-match to FAQ-style responses where small models perform well.

**Why the others are wrong:**
- Using GPT-4o for all 50,000 text queries costs ~$125/day for text alone. Using Phi-3 reduces text costs to ~$12/day — 10x savings. GPT-4o should be reserved for complex reasoning tasks.
- Llama 3.2 Vision 90B requires expensive real-time endpoint (~$8/hour = $5,760/month minimum). Shared endpoint cannot optimize per-task — both models must stay loaded. Self-hosted Llama costs exceed managed service benefits.
- Over-engineered: Computer Vision API duplicates GPT-4o vision capabilities. Three separate services increase integration complexity and latency. GPT-4o already handles OCR in vision mode.

</details>

---

**Q3.** A research team deploys a RAG application using Azure AI Foundry with a knowledge base of 2 million technical documents. Users report that queries about recent papers (last 6 months) return outdated results, while historical queries work well. Vector index rebuild occurs nightly. Which retrieval configuration change will MOST effectively address this issue?

A) Increase vector index dimensions from 1536 to 3072 and rebuild index immediately
B) Change from serverless to provisioned throughput deployment to increase query speed
C) Switch from vector search to hybrid search combining vector similarity with keyword BM25 and apply recency boosting to semantic ranker
D) Add metadata filters to vector search to exclude documents older than 6 months

<details>
<summary>Answer</summary>

**Correct: C**

Hybrid search (vector + BM25) catches exact terminology matches common in recent papers (new concepts, acronyms). Semantic ranker can apply recency boosting using document metadata timestamps, elevating newer papers in results. Vector-only search may embed new terms similarly to existing concepts, causing semantic collision. Exam tests understanding that hybrid > pure vector for temporal relevance.

**Why the others are wrong:**
- Higher dimensions don't solve temporal relevance — embeddings still represent semantic similarity regardless of dimensionality. Increases cost (storage, compute) without addressing root cause. Nightly rebuild already incorporates new documents.
- Deployment model (serverless vs PTU) affects latency and throughput, not retrieval accuracy or temporal relevance. Query speed is not the reported issue — result quality is.
- Metadata filtering solves the problem by brute force but eliminates 75% of knowledge base. Users may need historical context alongside recent findings. Doesn't leverage semantic ranking capabilities. Exam tests optimal solution, not workarounds.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** A development team configures an Azure AI Foundry hub with three projects: development, staging, and production. Each project deploys the same GPT-4o model but requires isolated network access. Staging must access on-premises data via VPN. Which infrastructure design satisfies these requirements with LEAST operational overhead?

A) Create three separate AI Foundry hubs with private endpoints per hub, configure VPN gateway per hub
B) Use single hub with three projects, enable managed virtual network for hub, configure private endpoint for staging project only
C) Create single hub with shared managed VNet, use network security groups to isolate projects, attach VPN to hub VNet
D) Deploy three AI Foundry hubs in different Azure regions, use global VNet peering and VPN on staging hub

<details>
<summary>Answer</summary>

**Correct: C**

Single hub reduces management overhead (one set of resources, billing, quotas). Managed VNet at hub level provides network isolation foundation. NSG rules isolate projects within shared VNet — exam tests understanding that projects inherit hub networking but can be segmented. VPN attaches once at hub level; routing rules direct staging traffic through VPN. Production/dev remain isolated via NSG.

**Why the others are wrong:**
- Three separate hubs multiply management overhead 3x: separate quotas, identity configs, cost tracking. VPN gateway per hub is expensive ($140/month each = $420 total). Violates 'LEAST operational overhead' requirement.
- Private endpoint applies per resource (hub), not per project. Cannot create project-only private endpoint — exam tests this architectural limitation. Managed VNet is hub-level setting, not project-level.
- Different regions unnecessary for network isolation and adds latency/data transfer costs. Global VNet peering + VPN overcomplicates design. Exam scenario doesn't require geo-distribution.

</details>

---

**Q5.** An AI agent application uses Azure OpenAI to process customer support tickets. The application authenticates using a service principal with client secrets stored in environment variables. Security audit flags this as high-risk. Which authentication pattern provides keyless access with LEAST code changes?

A) Store client secret in Azure Key Vault and reference vault URI in code
B) Migrate to DefaultAzureCredential with system-assigned managed identity, remove all secret references
C) Implement certificate-based authentication with certificate stored in Key Vault
D) Use Azure AD B2C with user-assigned managed identity per environment

<details>
<summary>Answer</summary>

**Correct: B**

DefaultAzureCredential automatically attempts managed identity auth first (when available), then falls back to other methods (useful for local dev). System-assigned MI provides keyless access — Azure handles credential rotation. Code change minimal: replace AzureKeyCredential(api_key) with DefaultAzureCredential(). Exam heavily tests managed identity as preferred pattern for Azure service-to-service auth.

**Why the others are wrong:**
- Still uses secrets (now in Key Vault vs env vars). Requires code to fetch secret from vault before creating credential — more code changes than option A. Not truly 'keyless' — secret still exists.
- Certificate-based auth still requires managing certificate lifecycle, storage, rotation. More complex than managed identity. Code changes significant: certificate loading, authentication flow different from secret-based.
- Azure AD B2C is for customer identity scenarios (B2C = business-to-consumer), not service authentication. User-assigned MI adds unnecessary complexity vs system-assigned for single application. Wrong identity service for this use case.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** A company runs a GPT-4o deployment with provisioned throughput of 50 PTUs. Usage patterns show peak load of 200 requests/minute for 2 hours daily (9-11 AM), and 20 requests/minute the remaining 22 hours. Monthly PTU cost is $15,000. Which cost optimization strategy provides BEST balance of performance and cost?

A) Keep 50 PTUs provisioned, implement request queuing during peak hours to smooth load
B) Switch entirely to serverless deployment with 10,000 TPM quota and burst allowance enabled
C) Reduce to 20 PTUs provisioned, deploy serverless GPT-4o as overflow endpoint with Azure Front Door load balancing
D) Use 10 PTUs provisioned 24/7, scale to 50 PTUs only during 9-11 AM window using deployment pipeline automation

<details>
<summary>Answer</summary>

**Correct: C**

20 PTUs handle off-peak load (20 req/min ≈ 20 PTU capacity). Cost: ~$6,000/month. During peak (200 req/min), overflow to serverless which handles ~180 req/min burst. Serverless cost: 2 hours × 60 min × 180 req × ~$0.01/req = ~$216/day × 30 = $6,480/month. Total: ~$12,480 vs $15,000 — saves $2,520/month (17%). Maintains peak performance via dual deployment pattern exam tests.

**Why the others are wrong:**
- No cost reduction — still paying for 50 PTUs full time. Queuing degrades user experience during peak hours (delays). Exam tests understanding that queuing is latency trade-off, not cost optimization.
- Serverless quota (TPM = tokens per minute) of 10,000 ≈ 100 requests/min at typical lengths. Cannot handle 200 req/min peak even with burst — would hit rate limits causing failures. Burst allowance is temporary, not sustainable for 2-hour daily peak.
- PTU deployments cannot auto-scale — capacity is fixed once provisioned. Exam tests this limitation. Scaling PTUs requires creating new deployment, updating application endpoints, potentially minutes of switchover — not viable for daily 9 AM peak. Pipeline automation doesn't solve provisioning delay.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** A CI/CD pipeline promotes AI models from development to production using Azure AI Foundry. The pipeline must ensure that only models achieving >90% accuracy on validation set deploy to production, while maintaining model versioning and rollback capability. Which implementation approach aligns with these requirements?

A) Use Azure DevOps pipeline with custom script that queries model registry metrics, deploy via Azure CLI if threshold met, tag deployment with commit SHA
B) Create Azure ML pipeline that auto-registers models with metrics, use deployment approval gate checking accuracy metadata, maintain 2 previous versions as blue/green deployments
C) Implement GitHub Actions workflow that runs inference on validation set, compares to baseline model, creates new endpoint deployment with traffic splitting 10/90 for canary testing
D) Deploy all models to production with version tags, use Application Insights to monitor accuracy in production, rollback manually if metrics degrade

<details>
<summary>Answer</summary>

**Correct: B**

Azure ML model registry stores metadata including metrics from training runs. Deployment approval gate can programmatically check accuracy metadata before promotion — enforces >90% requirement automatically. Blue/green deployment pattern (2 previous versions) enables instant rollback by traffic switch, no redeployment needed. Exam tests understanding of approval gates + versioning strategy for production AI systems.

**Why the others are wrong:**
- Custom script approach works but fragile — accuracy calculation logic lives outside model registry, harder to audit. Tagging with commit SHA doesn't provide rollback mechanism — would need to redeploy previous commit. No systematic version retention policy.
- Canary testing (10/90 split) is deployment strategy, not validation gate — doesn't prevent deploying bad model, just limits blast radius. Comparing to baseline in pipeline doesn't guarantee >90% absolute accuracy, only relative improvement. Missing systematic rollback mechanism.
- Deploying all models then monitoring production violates requirement — should validate before production exposure. Manual rollback is error-prone and slow. Application Insights monitors production metrics, but validation should happen pre-deployment. Exam tests shift-left principle.

</details>

---

**Q8.** An agent workflow application integrates with Azure AI Foundry to execute multi-step tasks: (1) retrieve customer history from database, (2) analyze sentiment, (3) generate personalized response, (4) log interaction. The agent must maintain conversation context across multiple user turns and access real-time inventory data. Which architecture components are REQUIRED?

A) Memory service (Cosmos DB or Redis), function calling configuration for inventory API tool, prompt flow orchestration, conversation history management
B) Vector database for embeddings, Azure Functions for tool execution, agent runtime container, LangChain integration library
C) Azure OpenAI Assistants API with built-in memory, Azure API Management for inventory service gateway, Application Insights for logging
D) Semantic Kernel framework, Azure Storage Tables for conversation state, custom tool plugins for database and inventory, GPT-4o deployment

<details>
<summary>Answer</summary>

**Correct: A**

Memory service (Cosmos DB/Redis) provides persistent conversation context storage — exam tests understanding that agents need external state management. Function calling enables agent to invoke inventory API as tool during reasoning. Prompt flow orchestrates multi-step tasks (retrieve→analyze→generate→log). Conversation history management tracks turns for context. These are core Azure AI Foundry agent components.

**Why the others are wrong:**
- Vector database not required for workflow described — no RAG or semantic search mentioned. LangChain is third-party framework, not required component. Agent runtime container is deployment detail, not architectural requirement. Over-specifies implementation vs required capabilities.
- Assistants API is one implementation option but not 'required' — can build agents other ways in Foundry. APIM for inventory service is infrastructure pattern, not agent component. Doesn't address conversation context persistence or multi-step orchestration requirements.
- Semantic Kernel is Microsoft framework but alternative to Foundry prompt flow, not required together. Storage Tables viable for state but exam tests understanding of purpose-built memory services. Doesn't address orchestration requirement. Over-specifies framework choice.

</details>

---

