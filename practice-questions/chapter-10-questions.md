## Chapter 10: Full Exam Simulation and Test-Taking Strategy — Practice Questions

**Q1.** A company is building a customer service chatbot that needs to access customer order history from a SQL database, recommend products based on past purchases, and handle multi-turn conversations with context. The solution must minimize development time and automatically handle conversation state. Which Azure AI solution BEST meets these requirements?

A) Deploy Azure OpenAI GPT-4 with custom code to query the database and implement conversation history tracking using Azure Cosmos DB
B) Use Azure AI Agent Service with a data source connection to Azure SQL Database and enable built-in conversation history
C) Create an Azure Machine Learning pipeline that fine-tunes a model on customer data and deploys to Azure Container Instances
D) Implement Azure AI Search with hybrid search over product catalog and use Azure Bot Service for conversation management

<details>
<summary>Answer</summary>

**Correct: B**

Azure AI Agent Service provides built-in conversation state management, native data source connectors (including SQL), and orchestration of multi-turn conversations. This minimizes development time compared to building custom conversation tracking and database integration code.

**Why the others are wrong:**
- Option A requires custom code for conversation state management and database queries, significantly increasing development time and maintenance overhead
- Option C is over-engineered for this scenario - fine-tuning a model is unnecessary when agents can query data sources directly, and adds weeks of development time
- Option D uses AI Search for product discovery but lacks the multi-turn conversation orchestration and SQL database integration that agents provide natively

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** During an exam simulation, you have 45 minutes remaining and 20 questions left. You encounter a complex case study with 5 related questions that you estimate will take 12 minutes to complete. What is the BEST time management strategy?

A) Complete the case study immediately to ensure you answer all related questions while the context is fresh
B) Skip the case study entirely, complete all remaining single questions first, then return to the case study if time permits
C) Read the case study scenario, answer the easiest 2-3 questions, flag the difficult ones, and return after completing other questions
D) Spend only 6 minutes on the case study (half your estimate), answer what you can, and move on regardless of completion

<details>
<summary>Answer</summary>

**Correct: C**

With 45 minutes for 20 questions (2.25 min/question average), you can afford some time on the case study but not the full 12 minutes. Reading the scenario once, answering the straightforward questions (3-4 minutes), and flagging difficult ones allows you to capture points while maintaining pace. You'll have context for returning later if time permits.

**Why the others are wrong:**
- Option A consumes 12 of your 45 remaining minutes (27%) for only 5 of 20 questions (25%), putting you behind pace and risking incomplete coverage of easier questions
- Option B wastes the time spent reading the scenario and risks running out of time before attempting 5 potentially scorable questions
- Option D arbitrarily limits time without strategic question selection - you might skip easy questions while spending time on impossible ones

</details>

---

**Q3.** A healthcare application uses Azure OpenAI to summarize patient medical records. During testing, the model occasionally fabricates medication names that don't appear in the source documents. Regulatory requirements mandate that all AI-generated summaries must be traceable to source content. Which solution BEST addresses this requirement?

A) Implement Azure AI Content Safety content filters to block fabricated medical terms from model outputs
B) Use prompt engineering to include 'Do not fabricate information' in the system message
C) Enable grounding with citations in the Azure OpenAI deployment and configure the application to display source references for all generated content
D) Deploy Azure AI Search with semantic ranking and use vector search results instead of generative AI

<details>
<summary>Answer</summary>

**Correct: C**

Grounding with citations ensures the model generates content based on provided source documents and returns references to specific source locations. This directly addresses fabrication (hallucination) by tying generated text to verifiable sources, meeting the regulatory traceability requirement.

**Why the others are wrong:**
- Content Safety filters detect harmful content (hate speech, violence, etc.) but cannot detect factual fabrications or ensure traceability to source documents
- Prompt engineering alone is insufficient - models can still hallucinate despite instructions, and this approach provides no verification mechanism
- Switching to pure search eliminates the summarization capability entirely, defeating the purpose of using generative AI for the use case

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** You are reviewing Azure OpenAI deployment options for a chatbot that will handle 100,000 requests per day with unpredictable traffic spikes during business hours. Average token usage is 1,500 tokens per request. The application requires consistent response times under 3 seconds. Which deployment configuration BEST meets these requirements?

A) Provisioned deployment with 200,000 provisioned throughput units (PTUs)
B) Standard deployment with autoscaling enabled and 100K tokens-per-minute (TPM) quota
C) Standard deployment with 500K TPM quota and retry logic for rate limit errors
D) Provisioned deployment with 50,000 PTUs and overflow routing to a Standard deployment

<details>
<summary>Answer</summary>

**Correct: A**

100K requests/day with unpredictable spikes and a 3-second SLA requires guaranteed throughput. Provisioned deployments provide reserved capacity with predictable latency. 100K requests × 1,500 tokens = 150M tokens/day ÷ 1,440 minutes ≈ 104K tokens/minute average, but spikes require ~200K PTU capacity for headroom.

**Why the others are wrong:**
- Standard deployments use shared infrastructure and can experience variable latency during high load, making the 3-second SLA unreliable during traffic spikes
- Higher quota doesn't guarantee capacity availability on Standard - you still share infrastructure and may experience throttling or latency spikes
- 50K PTUs is insufficient for the calculated throughput needs (would cover only ~25% of capacity), and overflow to Standard defeats the latency consistency requirement

</details>

---

**Q5.** An application uses Azure AI Document Intelligence to extract data from invoices. Users report that the system frequently misclassifies shipping invoices as purchase orders, resulting in incorrect data routing. The application must improve classification accuracy without retraining on new data. Which approach BEST addresses this issue?

A) Deploy both Invoice and Purchase Order prebuilt models, compare confidence scores, and route based on the higher-confidence result
B) Switch from the prebuilt invoice model to a custom neural model trained on the company's specific invoice formats
C) Use the prebuilt invoice model with confidence score thresholds and human-in-the-loop review for scores below 0.85
D) Implement the Layout model instead of the Invoice model to extract all text, then use Azure OpenAI to classify document types

<details>
<summary>Answer</summary>

**Correct: A**

Running specialized prebuilt models (Invoice and Purchase Order) in parallel and selecting based on confidence comparison leverages the fact that each model is optimized to recognize its specific document type. No retraining is required, and this approach directly addresses the misclassification issue by letting each model 'vote' on document type.

**Why the others are wrong:**
- Custom neural models require retraining on new data, which contradicts the requirement 'without retraining on new data'
- Confidence thresholds help detect low-quality extractions but don't solve the misclassification problem - a shipping invoice might be confidently misclassified as a purchase order
- Layout model extracts structure but has no classification capability - adding OpenAI increases complexity, cost, and latency without leveraging Document Intelligence's built-in classification

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** A RAG application uses Azure AI Search with vector search over 500,000 product descriptions. Users report that searches for 'laptop with long battery life' return gaming laptops with high-performance GPUs instead of business laptops optimized for battery life. The index uses text-embedding-ada-002 embeddings. Which solution BEST improves result relevance?

A) Increase the number of nearest neighbors (k) in the vector search query from 10 to 50
B) Re-embed all documents using text-embedding-3-large which has higher dimensionality
C) Add a filter to the vector search query to exclude products in the 'gaming' category
D) Switch to hybrid search combining vector search with keyword search using BM25 ranking

<details>
<summary>Answer</summary>

**Correct: D**

Vector search alone may miss keyword-specific nuances like 'battery life' which should strongly weight certain products. Hybrid search combines vector similarity with keyword matching (BM25), allowing exact matches on terms like 'battery life' to boost relevance while still using semantic understanding from vectors.

**Why the others are wrong:**
- Increasing k returns more results but doesn't change ranking quality - you'll get more gaming laptops mixed with business laptops, not better filtering
- text-embedding-3-large provides better semantic understanding but doesn't solve the keyword emphasis problem - 'battery life' as a concept might still match gaming laptops with powerful batteries
- Adding category filters requires prior knowledge of which categories to exclude and doesn't scale to queries where the category distinction isn't obvious

</details>

---

**Q7.** You are 60 minutes into the exam with 25 questions remaining. You encounter a question about Azure AI Vision spatial analysis that you are completely unfamiliar with. You've narrowed the answers to two options but cannot determine which is correct. What is the BEST strategy?

A) Spend up to 5 minutes researching the concept using the exam's reference materials to make an informed choice
B) Choose randomly between the two options, flag for review, and continue to maintain pace of 2 minutes per question
C) Choose the option that mentions newer technology (e.g., 'GPT-4 Vision' over 'Custom Vision'), flag the question, and move on
D) Skip the question entirely and return to it only if you finish all other questions with time remaining

<details>
<summary>Answer</summary>

**Correct: B**

With 25 questions in 60 minutes (2.4 min/question pace), you cannot afford 5+ minutes on one question. When you've already eliminated two options (50% correct guess), flag and move on to maintain pace. Flagging ensures you'll return if time permits, but a guess now captures potential points.

**Why the others are wrong:**
- The exam provides no reference materials to research - this option describes an impossible action
- Choosing based on 'newer technology' is unreliable - exam questions often test established services, and this heuristic fails when comparing services from different domains
- Skipping entirely wastes your elimination work (you're already at 50% odds) and risks forgetting to return - you might leave a 50/50 question blank that you could have guessed

</details>

---

**Q8.** A financial services application must authenticate to Azure OpenAI and Azure AI Search. Security policy prohibits storing secrets in application code or environment variables. The application runs on Azure Kubernetes Service (AKS). Which authentication solution BEST meets these requirements?

A) Store connection strings in Azure Key Vault and retrieve them at application startup using the application's service principal
B) Use Workload Identity to assign a managed identity to the AKS pod and authenticate to both services using that identity
C) Configure Azure AD authentication with a certificate stored in Key Vault and rotate it every 90 days
D) Generate API keys for both services and inject them into containers using Kubernetes secrets encrypted at rest

<details>
<summary>Answer</summary>

**Correct: B**

Workload Identity (managed identity for AKS pods) eliminates all secret management by using Azure AD token exchange. The pod receives an identity without any credentials stored in code, environment variables, or even Key Vault. Both Azure OpenAI and AI Search support managed identity authentication.

**Why the others are wrong:**
- Key Vault retrieval still requires a service principal credential to access Key Vault - you've moved the secret but not eliminated it
- Certificate-based authentication with rotation is complex and still involves secret management (the certificate itself) - this doesn't eliminate secrets, just changes the secret type
- API keys are explicitly secrets, and Kubernetes secrets are still stored values (even if encrypted at rest) - this violates the 'no secrets stored' requirement

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

