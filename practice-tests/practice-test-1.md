# Practice Test 1: AI-103: Azure AI Apps & Agents Developer Certification

- **Questions:** 45
- **Time limit:** 90 minutes
- **Passing score:** 70%%

> Answers are in `answer-key-1.md`. Track results in `score-tracker.md`.

---

**Q1.** A company is building a customer support chatbot that must answer questions using their product documentation (500 PDF files, 2GB total). The bot needs to cite specific document sections in responses and handle 10,000 daily conversations with sub-2-second response times. The solution must minimize hallucination and provide verifiable source attribution. Which Azure AI Foundry approach BEST meets these requirements?

A) Deploy GPT-4o directly with all 500 PDFs embedded in the system prompt using prompt engineering techniques
B) Use Azure AI Search with vector indexing for the PDFs, then deploy a Foundry agent with retrieval augmented generation (RAG) and citation grounding
C) Fine-tune a small language model (SLM) on the PDF content and deploy it as a standalone Foundry model endpoint
D) Create a Foundry tool that calls Azure OpenAI GPT-3.5 Turbo for each query and manually append document links to responses

---

**Q2.** An AI developer is deploying a Foundry agent that uses three custom tools: one queries a SQL database, one calls an external weather API, and one sends approval emails. The agent will be used by 50 internal employees. Security policy requires that the agent cannot access the SQL database or send emails without explicit human approval for each action, but weather queries should execute automatically. Which Foundry agent configuration satisfies this requirement?

A) Set the agent to 'copilot' oversight mode globally, then configure tool-specific access policies with auto-execute enabled for the weather tool
B) Configure the agent with 'autonomous' oversight mode and implement approval workflows only for the SQL and email tools
C) Use managed identity for SQL authentication, keyless credentials for the email service, and set all tools to require manual confirmation
D) Deploy the agent with 'approval' oversight mode and create a custom tool wrapper that bypasses approval for weather queries

---

**Q3.** A Foundry-based image analysis application processes user-uploaded photos using GPT-4o with vision capabilities. During testing, the system occasionally returns responses for images containing violence or graphic content that should be blocked. The development team needs to prevent the model from processing such images entirely. What is the MOST effective way to implement this safety control in Azure AI Foundry?

A) Enable Azure AI Content Safety integration with severity threshold filters on image inputs before they reach the GPT-4o model
B) Configure GPT-4o's built-in safety system message to refuse processing violent images in the model deployment
C) Implement a custom Python function that analyzes image metadata and blocks files larger than 5MB
D) Add a post-processing evaluator that detects harmful content in GPT-4o's text responses and redacts them

---

**Q4.** An organization runs 12 Foundry agents across development, staging, and production environments. Each agent uses different GPT-4o deployment configurations. The infrastructure team needs to enforce that all agents in production use private networking (no public internet access) and authenticate using managed identity instead of API keys. Which combination of Azure resources and configurations achieves this requirement?

A) Create a single Azure AI Foundry hub with managed identity enabled, deploy all agents to the same hub, and configure a private endpoint with virtual network integration for production workspaces
B) Deploy each agent to separate Azure AI Services resources, enable system-assigned managed identity on each resource, and configure NSG rules to block outbound internet traffic
C) Use Azure Key Vault to store managed identity credentials, configure each agent's deployment to reference Key Vault secrets, and enable Azure Private Link on the Key Vault instance
D) Create an Azure Virtual Network with a subnet for AI services, assign a user-assigned managed identity to the subscription, and configure route tables to force-tunnel all traffic through Azure Firewall

---

**Q5.** A Foundry application using GPT-4o Turbo is experiencing performance degradation during peak hours (9 AM-5 PM EST). The deployment is configured with 50K tokens-per-minute (TPM) quota. Monitoring shows the application makes 200 requests/hour during peak times with average request size of 1,500 input tokens and 800 output tokens. Rate limit errors (429) occur frequently. Assuming the current quota is fully allocated, what is the MINIMUM TPM quota increase needed to eliminate rate limiting during peak hours?

A) Increase TPM quota to 77K (add 27K to current 50K)
B) Increase TPM quota to 100K (double current allocation)
C) Increase TPM quota to 150K (triple current allocation)
D) Increase TPM quota to 65K (add 15K to current 50K)

---

**Q6.** An AI engineering team is deploying a Foundry agent that generates SQL queries from natural language. The agent uses a GPT-4o model with a custom tool that executes SQL against a production database. Responsible AI requirements mandate that all SQL executions must be logged with full provenance (user prompt, generated query, execution timestamp, and result set) for audit compliance. Which Azure AI Foundry feature BEST satisfies this requirement with minimal custom implementation?

A) Enable Application Insights integration for the Foundry project and configure custom telemetry events in the SQL tool's execution function
B) Implement a custom middleware layer that intercepts agent responses and writes provenance metadata to Azure Blob Storage
C) Configure trace logging on the agent deployment with automatic prompt, tool call, and response capture to Azure Monitor Logs
D) Use Azure SQL Database audit logs to capture query executions and correlate them with agent conversation IDs stored in Cosmos DB

---

**Q7.** A company is building a customer support agent that must handle text queries, analyze uploaded images of damaged products, and generate voice responses. The agent needs to access a product catalog, create support tickets, and escalate complex issues to humans. Which Azure AI Foundry capability provides the MOST efficient way to integrate these multimodal inputs and tool-calling requirements?

A) Use Azure AI Foundry agent runtime with a multimodal model (GPT-4o) and register product catalog search, ticketing API, and escalation workflow as Foundry Tools
B) Deploy separate Azure OpenAI GPT-4o, Azure AI Vision, and Azure Speech models, then orchestrate them using custom Python code with manual prompt chaining
C) Create an Azure Machine Learning pipeline that routes requests to specialized models based on input type, with Azure Functions handling tool execution
D) Implement Azure Logic Apps to detect input modality and call appropriate Cognitive Services APIs, storing conversation state in Cosmos DB

---

**Q8.** An AI application using Azure AI Foundry experiences token rate limit errors during peak hours (429 responses). The application uses a GPT-4 deployment with 50K tokens-per-minute (TPM) quota. Monitoring shows average usage of 35K TPM but spikes to 80K TPM for 2-3 minutes during peaks. Which solution provides the MOST cost-effective resolution while maintaining performance?

A) Increase the deployment quota to 100K TPM to accommodate peak usage
B) Deploy a second GPT-4 instance and use Azure Front Door to load balance requests across both deployments
C) Switch to GPT-4o-mini for all requests, which has higher default TPM limits
D) Implement exponential backoff retry logic with jitter in the application code and enable dynamic quota management to temporarily scale during detected peaks

---

**Q9.** A financial services application must ensure that an Azure AI Foundry agent cannot access external APIs or generate responses containing personal financial data without explicit approval. The agent uses registered Foundry Tools for account lookup and transaction history. Which combination of controls BEST enforces these requirements?

A) Enable agent oversight mode set to 'approval-required', configure content safety filters to block PII patterns, and restrict tool access policies to require human confirmation for all external tool calls
B) Use Azure Private Link to block internet access, enable the 'Protected Material' content filter, and implement Azure RBAC to prevent the agent service principal from calling external endpoints
C) Set up Azure API Management policies to intercept all agent requests, scan responses for financial data using regex, and enable Azure Content Safety text moderation on outputs
D) Deploy the agent in a VNet with NSG rules blocking outbound traffic, use prompt shields to detect jailbreak attempts, and log all tool calls to Azure Monitor for manual review

---

**Q10.** An Azure AI Foundry application uses vector search to ground LLM responses with company documents stored in Azure AI Search. Users report that responses sometimes contain outdated product specifications despite recent document updates in the source blob storage. The search index is configured with a scheduled indexer that runs every 6 hours. Which change will MOST effectively ensure responses reflect current data?

A) Reduce the indexer schedule to run every 15 minutes to minimize indexing lag
B) Implement a cache invalidation strategy that clears the LLM's context window whenever documents are updated
C) Configure the blob storage data source with change tracking enabled and set the indexer to run on a real-time trigger when documents are modified
D) Switch from vector search to hybrid search (vector + keyword) to improve retrieval of recent documents

---

**Q11.** A development team must deploy an Azure AI Foundry agent to production with automated testing of prompt quality, grounding accuracy, and safety compliance before each release. The deployment pipeline is built in Azure DevOps. Which Azure AI Foundry feature provides the MOST comprehensive way to validate these requirements in the CI/CD pipeline?

A) Azure Load Testing to simulate user traffic against the agent endpoint and measure response times and error rates
B) Azure Monitor Application Insights custom metrics tracking prompt token usage, response length, and HTTP status codes
C) Azure AI Foundry Evaluators configured to run automated assessments for groundedness, relevance, and safety metrics, with results fed into DevOps pipeline gates
D) Azure Policy compliance scans to verify that the agent deployment meets organizational security standards

---

**Q12.** An Azure AI Foundry application must access Azure Blob Storage to retrieve documents for grounding without managing storage account keys or connection strings in application configuration. The application runs in Azure Container Apps. Which authentication configuration BEST meets this requirement following Azure security best practices?

A) Store the storage account connection string in Azure Key Vault and configure the Container App to retrieve it using a managed identity with Key Vault access
B) Create a service principal with a client secret, assign it storage permissions, and store the client ID and secret in Container Apps environment variables
C) Use Azure Active Directory authentication with a user-assigned managed identity and configure SAS token generation with read-only permissions
D) Enable system-assigned managed identity on the Container App and grant it the 'Storage Blob Data Reader' role on the storage account, using keyless DefaultAzureCredential in application code

---

**Q13.** A company is building a customer support application using Azure AI Foundry. The application must answer questions using the company's internal documentation stored in Azure Blob Storage, cite specific documents in responses, and handle follow-up questions while maintaining conversation context. Which Azure AI Foundry implementation approach BEST meets these requirements?

A) Deploy a GPT-4 model endpoint and use the Completions API with the entire document set concatenated into each prompt
B) Fine-tune a small language model on the company documentation and deploy it as a dedicated endpoint in Foundry
C) Implement a RAG pattern using Azure AI Search as the vector store, retrieve relevant chunks via semantic search, and pass them as grounding context to the LLM with conversation history
D) Create a Foundry agent with a custom tool that reads files directly from Blob Storage and returns full documents to the LLM for each query

---

**Q14.** You are implementing a multi-agent orchestration solution in Azure AI Foundry where a coordinator agent delegates tasks to three specialist agents: one for SQL queries, one for data visualization, and one for report generation. The coordinator must track which specialist completed each subtask and handle failures by reassigning work. Which Foundry agent design pattern should you implement?

A) Configure all four agents with identical tool schemas and use conversation memory to track which agent handled each message
B) Implement the coordinator agent with custom tools that invoke each specialist agent's endpoint, parse responses, and maintain orchestration state in the coordinator's conversation context
C) Deploy each specialist as a separate Foundry project and use Azure Logic Apps to orchestrate the workflow between projects
D) Use a single agent with all tools (SQL, visualization, report generation) and rely on the LLM to reason about task sequencing

---

**Q15.** An Azure AI Foundry application uses a deployed GPT-4 model to generate product descriptions. Evaluations show that 12% of outputs contain fabricated technical specifications not present in the source data. The application uses a RAG pattern with product specs retrieved from Azure AI Search. Which combination of techniques will MOST effectively reduce fabrication rate?

A) Increase the temperature parameter from 0.7 to 1.2 to generate more diverse outputs and reduce repetitive fabrications
B) Switch from GPT-4 to a fine-tuned small language model trained specifically on product descriptions
C) Implement model self-critique by having the LLM generate a response, then prompt it to verify claims against the retrieved context, and regenerate if discrepancies are found
D) Add a post-processing step that uses Azure Content Safety API to flag outputs containing fabricated content

---

**Q16.** You are deploying a code generation agent in Azure AI Foundry that uses a Codex-family model. The agent must generate Python functions, execute them in a sandbox to verify correctness, and iterate until tests pass. The sandbox execution must be isolated and timeout after 30 seconds. Which Foundry agent tool implementation should you use for the sandbox execution?

A) Use the built-in Code Interpreter tool in Foundry agents, configure execution timeout to 30 seconds in the tool schema
B) Define a custom tool with an Azure Function backend that spins up an Azure Container Instance with a 30-second timeout to execute the code
C) Implement a custom tool that calls Azure Batch to execute the Python code in an isolated compute node
D) Define a tool that uses the Azure OpenAI Service function calling feature to execute Python code directly in the model's runtime

---

**Q17.** A financial services company is building an Azure AI Foundry agent that recommends investment portfolios. Compliance requires that all agent recommendations be reviewed by a human advisor before being shown to customers. The agent uses a GPT-4 model and has access to market data tools. Which Foundry workflow pattern should you implement to meet the compliance requirement?

A) Configure the agent with autonomous workflow mode and add a post-processing approval step in the application layer that queues recommendations for human review
B) Use Foundry's model evaluation feature to automatically score recommendation quality and only present outputs with quality scores above 0.9 to customers
C) Implement a semiautonomous workflow where the agent generates recommendations but requires explicit human approval via an approval flow before final output is released to customers
D) Deploy two agents: one autonomous agent to generate recommendations and a second agent configured with a human-in-the-loop tool to review the first agent's output

---

**Q18.** You are optimizing an Azure AI Foundry application that uses a GPT-4 deployment for document summarization. Telemetry shows average latency of 8 seconds per request, with token analytics revealing input tokens average 6,000 and output tokens average 800. The application processes 10,000 documents daily. Which optimization will MOST effectively reduce latency while maintaining summary quality?

A) Implement prompt compression by summarizing input documents with a smaller model first, then using GPT-4 to refine the summary
B) Reduce max_tokens parameter from 2048 to 800 to match observed output length and reduce generation time
C) Enable streaming responses to return partial summaries incrementally and reduce perceived latency
D) Switch from GPT-4 to GPT-4 Turbo, which has faster inference speed and lower cost per token for the same quality

---

**Q19.** You are building a retrieval-augmented generation (RAG) application using Azure AI Foundry. The application must answer questions about internal product documentation stored in Azure Blob Storage. You need to implement the retrieval component with the lowest latency for queries while maintaining semantic search capabilities. Which solution should you implement?

A) Create an Azure AI Search index with vector search enabled, use the text-embedding-ada-002 model to generate embeddings for documents, and configure hybrid search combining vector and keyword search
B) Store documents in Azure Cosmos DB, implement full-text search using the built-in indexing, and use GPT-4 to generate responses directly from retrieved documents
C) Use Azure Blob Storage static website hosting with client-side JavaScript to search documents, then send matched content to Azure OpenAI for generation
D) Configure Azure AI Document Intelligence to extract text from documents on-demand during each query, then pass the extracted text to the language model

---

**Q20.** A company is deploying an agent-based solution in Azure AI Foundry that must call external REST APIs to retrieve real-time inventory data. The agent uses function calling to determine when to query inventory. During testing, the agent sometimes fabricates inventory numbers instead of calling the function. What should you configure to ensure the agent always calls the function when inventory questions are asked?

A) Increase the model temperature parameter to 1.5 to improve creativity in function selection
B) Set the function tool schema's 'strict' parameter to true and ensure the function description clearly states it must be called for inventory queries
C) Add a system message instructing the model to 'never make up inventory data' and increase the top_p parameter to 0.95
D) Switch from function calling to a prompt engineering approach with the inventory API endpoint hardcoded in the system prompt

---

**Q21.** You are evaluating a generative AI application built with Azure AI Foundry that summarizes customer support tickets. You need to measure whether the summaries contain factual information not present in the original tickets. Which evaluation metric should you use?

A) Groundedness score using Azure AI Foundry's built-in evaluation metrics
B) BLEU score comparing generated summaries against reference summaries
C) Perplexity measurement of the language model's output
D) Token count ratio between input tickets and output summaries

---

**Q22.** A development team is building a multi-agent orchestration solution where a coordinator agent delegates tasks to specialized agents for data analysis, report generation, and email composition. Each specialized agent must maintain conversation context only for its specific task, not the entire orchestration flow. How should you implement conversation memory for this architecture?

A) Use a single shared conversation context across all agents stored in Azure Cosmos DB to ensure all agents have access to the complete conversation history
B) Configure separate conversation contexts for each specialized agent, with the coordinator agent managing a separate context that tracks delegation decisions and overall workflow state
C) Implement conversation memory only in the coordinator agent and pass all necessary context as parameters in each delegation call to specialized agents
D) Disable conversation memory entirely and rely on the model's inherent context window to maintain state across all agents

---

**Q23.** Your organization is deploying a generative AI application that uses GPT-4 for content generation. During load testing, you observe that 95th percentile latency is 8 seconds, exceeding the 3-second requirement. The application generates marketing copy averaging 200 tokens. Token analytics show the prompt contains 3,500 tokens of context. What optimization should you implement first?

A) Switch from GPT-4 to GPT-3.5-turbo which has faster response times for similar generation tasks
B) Increase the max_tokens parameter from 200 to 500 to allow the model to generate content in a single response
C) Deploy multiple model instances behind a load balancer and implement request queuing to distribute load
D) Implement prompt compression techniques to reduce the context from 3,500 tokens to under 1,500 tokens, focusing on removing redundant examples and verbose instructions

---

**Q24.** You are implementing an autonomous agent in Azure AI Foundry that processes expense reports. The agent must automatically approve expenses under $500 but require human approval for amounts of $500 or greater. The agent uses function calling to submit approvals. Which implementation ensures compliance with the approval requirement?

A) Add a system prompt instruction stating 'Do not approve expenses of $500 or more without human review' and rely on the model to enforce this rule
B) Set the temperature parameter to 0.0 to ensure deterministic behavior when processing expense amounts
C) Implement the approval logic entirely in the function code by checking the amount and automatically approving or rejecting without returning control to the agent
D) Configure the expense approval function with a parameter validation that returns a 'requires_human_approval' flag for amounts >= $500, and implement a workflow that routes flagged approvals to a human-in-the-loop queue before execution

---

**Q25.** A financial services company is building an Azure AI Foundry agent that processes customer loan applications. The agent must retrieve relevant regulatory guidelines from a knowledge base, calculate eligibility using a custom Python function, and then generate a personalized explanation letter. The agent should not proceed with letter generation if the eligibility calculation returns a denial. Which Azure AI Foundry agent design pattern BEST implements this workflow?

A) Create a single-agent solution with all three tools (knowledge retrieval, calculation function, and letter generation) registered, and rely on the LLM to autonomously decide the execution sequence based on the conversation context
B) Deploy three separate specialized agents in a multi-agent solution where each agent handles one capability, and use Azure AI Foundry's agent-to-agent communication to pass results sequentially through the pipeline
C) Use a semiautonomous workflow with human-in-the-loop approval gates after each tool execution, requiring manual confirmation before the agent proceeds to the next step in the loan processing sequence
D) Implement a multi-step orchestrated workflow where the agent calls the knowledge retrieval tool first, then conditionally calls the calculation function, and only invokes the letter generation tool when approval criteria are met, using tool result inspection between steps

---

**Q26.** An e-commerce company has deployed a RAG-based product recommendation agent using Azure AI Foundry with a vector store containing 50,000 product descriptions. Users report that the agent sometimes recommends discontinued products and occasionally fabricates product features that don't exist in the source data. The development team needs to improve answer quality while maintaining response latency under 3 seconds. Which combination of evaluation and optimization techniques should be implemented?

A) Increase the number of retrieved chunks from 5 to 20 to provide more context, implement chain-of-thought prompting to show reasoning steps, and add a post-processing rule engine that validates product availability against an inventory database
B) Switch to a smaller, faster model to reduce latency budget for safety checks, implement a self-critique loop where the agent generates multiple responses and selects the best one, and add relevance scoring to filter retrieved chunks
C) Implement fabrication detection evaluation using groundedness scoring against retrieved chunks, add metadata filters to exclude discontinued products from vector search, and enable model reflection where the LLM critiques its own response before returning it to the user
D) Enable hybrid search combining vector similarity with keyword matching, add a human-in-the-loop approval step for all product recommendations, and implement prompt engineering with explicit instructions to never mention discontinued products

---

**Q27.** A healthcare application uses Azure AI Foundry to deploy a GPT-4o model for generating patient care summaries from clinical notes. After analyzing traces and token analytics, the team observes that 60% of tokens are consumed by the system prompt, average response latency is 8 seconds, and the model occasionally includes irrelevant medical history from earlier in the conversation. The application must reduce costs by 40% while maintaining summary quality. What is the MOST effective optimization approach?

A) Implement conversation memory truncation to keep only the last 3 exchanges, compress the system prompt by removing examples and verbose instructions, and switch to a smaller model like GPT-4o-mini for summary generation
B) Keep the full system prompt for quality, implement retrieval-augmented generation to fetch only relevant portions of patient history instead of including full conversation context, and adjust temperature to 0.0 to reduce output token variability
C) Move to a hybrid design where a smaller model (GPT-4o-mini) handles initial summarization with a concise prompt, then use GPT-4o only for quality validation and refinement when the small model's output fails confidence thresholds
D) Optimize the system prompt using few-shot examples that demonstrate concise output, implement prompt caching to reuse the system prompt across requests, and configure max_tokens to limit response length to 200 tokens per summary

---

**Q28.** A company is building an e-commerce platform that needs to generate product lifestyle images from text descriptions. The solution must support inpainting to remove unwanted objects from reference images and apply brand-specific watermarks to all generated images. Which Azure AI service configuration BEST meets these requirements?

A) Azure AI Foundry image generation with support for mask-based editing and watermark injection capabilities
B) Azure AI Vision with Custom Vision model trained on product images
C) Azure OpenAI Service GPT-4 Turbo with Vision for image analysis and generation
D) Azure AI Foundry image generation with DALL-E 3, using prompt engineering for inpainting and Azure Content Safety for watermarking

---

**Q29.** A healthcare application needs to generate alt-text descriptions for medical images to ensure accessibility compliance. The solution must ground descriptions in actual visual evidence present in the image and flag any unsafe medical content. Which implementation approach is correct?

A) Use Azure AI Vision's Image Analysis API with dense captions feature, then apply Azure Content Safety filters to the generated text
B) Configure Azure AI Foundry multimodal model for visual question answering with Content Safety integration for unsafe content detection
C) Use Azure Content Understanding pro-mode pipeline to extract visual characteristics, then generate descriptions with GPT-4 text model
D) Deploy GPT-4o multimodal model with specific prompts for medical image captioning and enable visual content filters for unsafe image detection

---

**Q30.** A video editing application allows users to modify marketing videos using text prompts. Users have reported that embedded text in uploaded videos is causing inappropriate content generation. What is the PRIMARY security concern and mitigation approach?

A) Indirect prompt injection via embedded text in video frames; apply multimodal content filters that analyze both visual and textual elements before generation
B) Prompt injection attacks through visual text; implement OCR scanning of video frames and filter detected text through Azure Content Safety before processing user prompts
C) Copyright infringement from brand logos; enable prohibited-symbol flagging in Azure Content Understanding to detect and block trademark violations
D) Malicious watermark removal; enforce watermark verification on all uploaded videos using Azure AI Vision watermark detection

---

**Q31.** A media company needs to analyze 10,000 hours of video content to identify all scenes containing specific objects and generate timestamped summaries. The solution must optimize for cost while maintaining accuracy. Which Azure Content Understanding configuration should be used?

A) Single-task Content Understanding pipeline with object detection model, processing full video at highest resolution
B) Pro-mode Content Understanding pipeline with component identification and region analysis, using adaptive frame sampling
C) Azure AI Vision Video Analysis with custom object detection model trained on company-specific objects
D) GPT-4o multimodal model processing video frames at 1-second intervals with object identification prompts

---

**Q32.** An application generates product images and must comply with brand usage rules that prohibit displaying competitor logos. The system should automatically reject generation requests that might violate these rules. What combination of services implements this requirement?

A) Azure AI Foundry image generation with custom prompt filters that block competitor brand names in text prompts
B) Azure Content Understanding prohibited-symbol flagging on generated images combined with pre-generation prompt analysis
C) Azure Content Safety visual content filters configured with custom prohibited symbol detection for competitor logos
D) GPT-4o for analyzing user prompts to detect competitor references, then Azure AI Vision Custom Vision to scan generated images

---

**Q33.** A real estate platform generates virtual staging images by editing photos of empty rooms. Users can describe desired furniture and styles, and the system performs prompt-driven modifications while preserving room architecture. Which editing workflow approach is correct for this scenario?

A) Use Azure AI Foundry video editing workflows adapted for sequential image frames representing furniture placement stages
B) Configure prompt-driven image modifications with reference image input, using generation controls to preserve background architecture while adding foreground elements
C) Apply Azure Content Understanding to identify room regions, then use mask-based editing to inpaint furniture in identified areas
D) Generate complete new room images with DALL-E 3 using detailed prompts describing both room and furniture, then blend with original using Azure AI Vision

---

**Q34.** A healthcare application needs to extract patient symptoms, medications, and diagnoses from clinical notes while ensuring protected health information (PHI) is detected and redacted. The solution must work with Azure AI Foundry agents and provide structured JSON output. Which approach BEST meets these requirements?

A) Use Azure AI Language's named entity recognition with the healthcare domain, then use a separate PII detection service, and manually combine results into JSON
B) Deploy Azure Text Analytics container on-premises with custom entity models and write custom code to convert results to JSON
C) Configure an Azure AI Foundry agent with a system prompt that includes entity extraction instructions, enable content safety filters for PII detection, and use structured output mode with a JSON schema defining symptom, medication, and diagnosis fields
D) Use Azure Translator API with entity preservation settings to extract entities during translation

---

**Q35.** A compliance team needs to analyze 50,000 customer support emails monthly to extract sentiment, detect regulatory concerns, and summarize key issues into 2-3 sentence summaries for each email. Processing must complete within 24 hours and stay under $500/month. Which solution provides the MOST cost-effective approach?

A) Chain three separate Foundry tools: sentiment analysis tool, then entity recognition for regulatory terms, then summarization tool, processing each email through all three sequentially
B) Use Azure AI Foundry batch processing with a GPT-4o-mini model, create a single prompt that extracts sentiment, identifies regulatory keywords, and generates summaries in one pass, output structured JSON with all three fields
C) Deploy three Azure AI Language services (sentiment analysis, entity recognition, summarization) and call each API separately for every email
D) Use GPT-4 with individual prompts for sentiment, compliance detection, and summarization, making three separate API calls per email

---

**Q36.** An Azure AI Foundry agent must process customer complaints and route them to appropriate departments. The agent uses speech-to-text to receive complaints, analyzes them for sentiment and urgency, then uses text-to-speech to confirm routing. Testing reveals that the agent misinterprets domain-specific product names in speech input, causing incorrect routing. Which solution BEST addresses this issue?

A) Increase the speech-to-text confidence threshold to reject low-confidence transcriptions
B) Create a custom speech model with phrase lists containing the product names and their phonetic variations, then configure the agent to use this custom model for speech-to-text
C) Add a post-processing step that uses entity recognition to correct product names after speech-to-text transcription
D) Switch to a different base speech model with better general accuracy

---

**Q37.** A financial services application must translate customer documents from 15 languages into English while preserving monetary amounts, account numbers, and regulatory terms exactly as written. The solution must integrate with Azure AI Foundry agents and support custom glossaries for financial terminology. What is the correct implementation?

A) Use the Foundry agent's built-in language model with a prompt instructing it to translate while preserving specific entities, providing examples of monetary formats
B) Implement Azure AI Language translation with entity preservation rules configured through the Azure portal
C) Configure Azure Translator with custom translation models and a glossary file, then call the Translator API from the Foundry agent workflow using the custom model endpoint
D) Create a Foundry tool that wraps Azure Translator, configure document translation with character set preservation

---

**Q38.** A content moderation agent analyzes user-generated social media posts for safety issues. The agent must detect hate speech, violence, self-harm content, and also identify if the text tone is aggressive versus constructive criticism. The solution processes 10 million posts per day. Which combination of services provides complete coverage?

A) Use only a GPT-4o model with a comprehensive safety and tone analysis prompt that covers all categories
B) Azure AI Language sentiment analysis for tone, plus custom text classification models for each safety category
C) Azure AI Content Safety for all safety categories plus tone detection, using the built-in tone analysis feature
D) Azure AI Content Safety API for hate speech, violence, and self-harm detection; add a generative prompt to the Foundry agent for tone analysis (aggressive vs. constructive)

---

**Q39.** An Azure AI Foundry agent handles customer support calls through speech interaction. After implementing speech-to-text and text-to-speech, the business requires the agent to detect caller frustration from voice characteristics (tone, pitch, speaking rate) and escalate to human agents when frustration is high. Which approach correctly implements this requirement?

A) Configure the speech-to-text service with emotion detection enabled, then use the emotion scores in the agent's logic to trigger escalation
B) Add a multimodal reasoning step where the agent processes the audio input directly with a model that supports audio modality, analyzing paralinguistic features for frustration indicators, then routing based on the analysis
C) Use Azure AI Language sentiment analysis on the transcribed text to detect negative sentiment as a proxy for frustration
D) Implement custom speech model training with labeled frustrated vs. calm speech samples

---

**Q40.** A financial services company needs to extract tables, handwritten signatures, and custom fields from 50,000 scanned loan documents stored in Azure Blob Storage. The extracted data must be indexed for semantic search and made available to an Azure AI agent for customer inquiries. Which solution BEST meets these requirements?

A) Use Azure AI Document Intelligence with a custom model, output to Azure AI Search with a skillset that includes OCR and key-value extraction, then connect the search index as a grounding source for the agent
B) Use Azure AI Vision OCR API to extract text, manually parse tables with custom code, store results in Cosmos DB, and configure the agent to query Cosmos DB directly
C) Deploy Azure AI Content Understanding to analyze documents with layout analysis enabled, index the markdown output in Azure AI Search with vector embeddings, and register the index as an agent tool
D) Use Azure Form Recognizer prebuilt invoice model for all documents, store extracted JSON in Azure Table Storage, and have the agent read from Table Storage using function calling

---

**Q41.** You are building a RAG ingestion pipeline for a legal document repository containing PDFs with complex multi-column layouts, embedded images, and footer annotations. The pipeline must preserve document structure for accurate agent grounding. Which Azure AI Search skillset configuration should you implement?

A) OCR skill → Text Split skill → Azure OpenAI Embedding skill → Output to index with vector fields
B) Text Extraction skill → Entity Recognition skill → Key Phrase Extraction skill → Output to index with string fields only
C) Vision Multimodal Embedding skill → Direct output to vector index with no text processing
D) Document Layout skill → OCR skill → Custom skill for cleanup → Text Merge skill → Azure OpenAI Embedding skill → Output to index with vector and metadata fields

---

**Q42.** A healthcare provider needs to index 100TB of medical imaging studies (DICOM files) with associated radiology reports for a diagnostic support agent. The solution must support hybrid search combining visual similarity and clinical terminology, with results grounded in both image content and report text. What is the MOST efficient ingestion architecture?

A) Use Azure AI Vision Image Analysis 4.0 to generate embeddings for images, Azure AI Language for report embeddings, merge vectors using weighted concatenation, and index in Azure AI Search with hybrid retrieval enabled
B) Store DICOM files in Azure Blob Storage with hierarchical namespace, use Azure AI Search built-in image extraction skill and OCR skill for reports, enable vector search with separate image and text indexes linked by document ID
C) Use Azure AI Content Understanding multimodal analyzer for both DICOM images and PDF reports, configure hybrid search skillset with vector and keyword fields, enable integrated vectorization with GPT-4o embeddings, and register the unified index as agent grounding source
D) Deploy a custom Azure Machine Learning pipeline to extract DICOM metadata and thumbnails, use GPT-4o to generate multimodal embeddings combining image and report content, index unified embeddings in Azure AI Search with semantic ranker

---

**Q43.** You are implementing a video content extraction pipeline for a media company's archive of 10,000 interview videos. The extracted content must support an agent that answers questions about specific topics discussed, identifies speakers, and retrieves exact video timestamps. Which combination of services provides complete functionality?

A) Azure AI Speech batch transcription with speaker diarization, Azure AI Language topic modeling on transcripts, Azure AI Search with custom skills for timestamp indexing and hybrid search enabled
B) Azure AI Video Indexer for transcription and topic extraction, store results in Azure Cosmos DB, implement custom timestamp search with SQL queries
C) Azure Media Services for video storage, Azure OpenAI Whisper API for transcription, manual chunking by speaker, Azure Cognitive Search with keyword search only
D) Azure AI Vision Video Retrieval API for visual analysis, Azure AI Speech for audio transcription without diarization, combine outputs in Synapse Analytics for querying

---

**Q44.** A manufacturing company ingests technical manuals containing diagrams, tables, and multilingual text into an Azure AI Search index for agent grounding. After deployment, the agent frequently provides incorrect answers about diagram content. The search index uses OCR skill and text splitting with 500-token chunks. What is the MOST likely cause and solution?

A) OCR skill doesn't extract text from diagrams; add Azure AI Vision Custom Vision model trained on technical diagrams to extract labels and annotations, then merge outputs before chunking
B) 500-token chunks split diagram context across multiple chunks losing semantic coherence; reduce chunk size to 200 tokens with 50-token overlap to preserve diagram relationships
C) Multilingual text confuses the embedding model; add Language Detection skill and route each language to language-specific indexes for better retrieval quality
D) The pipeline lacks layout analysis to preserve spatial relationships in diagrams; add Document Layout skill before chunking and configure chunk strategy to respect diagram boundaries, or use Image Analysis skill to generate diagram descriptions that are chunked with surrounding text

---

**Q45.** You need to enrich a product catalog ingestion pipeline with custom domain knowledge before indexing for agent grounding. The enrichment must map product codes to detailed specifications stored in an internal SQL database, then generate embeddings that include both catalog text and specification data. How should you implement this in Azure AI Search?

A) Use a custom Web API skill that queries the SQL database with product codes, merges specification data into the document, then passes enriched content to downstream Azure OpenAI Embedding skill in the same skillset execution
B) Configure Azure AI Search to use SQL Database as a secondary data source, set up a scheduled indexer merge job, then run embedding generation as a separate batch process
C) Export SQL specifications to JSON files in Blob Storage, use FileReader skill to load specifications, manually join in application code before sending to search indexer
D) Implement a pre-processing Azure Function that enriches documents before upload to Blob Storage, configure the search indexer to read pre-enriched documents with embeddings already generated

---

