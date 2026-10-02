## Chapter 4: Building Agents with Microsoft Foundry — Practice Questions

**Q1.** A company is building an AI agent that needs to query a product database, check inventory levels via REST API, and generate customer responses. The agent must use real-time data for each query. Which tool integration approach BEST meets these requirements?

A) Embed product data in the agent's system prompt and use RAG for inventory lookups
B) Define function calling schemas for both database queries and API calls, registering them as agent tools
C) Use Azure AI Search to index both product data and inventory, querying via semantic search
D) Store all data in conversation memory and update it periodically via scheduled jobs

<details>
<summary>Answer</summary>

**Correct: B**

Function calling with tool schemas allows the agent to dynamically invoke external APIs and databases in real-time. Each tool is defined with JSON schema specifying parameters, enabling the agent to construct proper API calls based on conversation context.

**Why the others are wrong:**
- System prompts are static and cannot provide real-time data; RAG retrieves documents but doesn't execute API calls
- Azure AI Search works for indexed content but cannot execute REST API calls or database queries that require authentication/dynamic parameters
- Conversation memory stores chat history, not real-time external data; scheduled updates create stale data issues

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** An agent handles customer support requests. After 50 turns of conversation, the agent begins losing context of earlier issues mentioned. Token limits prevent including full conversation history. Which memory pattern should be implemented?

A) Vector memory with embeddings of conversation chunks for semantic retrieval
B) Entity memory tracking only customer name and ticket ID
C) Summary memory that condenses conversation history into progressive summaries
D) Sliding window memory keeping only the last 10 message pairs

<details>
<summary>Answer</summary>

**Correct: C**

Summary memory maintains context across long conversations by progressively condensing older messages into summaries while keeping recent messages verbatim. This balances token efficiency with context preservation, critical for multi-turn support scenarios.

**Why the others are wrong:**
- Vector memory adds complexity and retrieval latency; works better for knowledge bases than conversation flow
- Entity memory only tracks structured data, losing conversation nuance and issue details needed for coherent responses
- Sliding window loses critical early context (original issue, customer details) that may be referenced later in the conversation

</details>

---

**Q3.** A multi-agent system has a coordinator agent that routes requests to specialist agents (Legal, Finance, HR). A request requires both Legal review AND Finance approval before responding. Which orchestration pattern is MOST appropriate?

A) Sequential handoff where coordinator sends to Legal first, then Legal's output goes to Finance, then back to coordinator
B) Parallel execution where coordinator sends request to both agents simultaneously and merges responses
C) Hierarchical pattern where Legal agent spawns Finance sub-agent and returns combined result
D) Autonomous mode where coordinator randomly selects one specialist and trusts its judgment

<details>
<summary>Answer</summary>

**Correct: A**

Sequential handoff ensures proper workflow: Legal reviews first (outputs decision), Finance reviews Legal's decision (outputs approval), coordinator receives final verdict. This maintains dependencies and approval chains. Exam heavily tests sequential vs parallel distinction.

**Why the others are wrong:**
- Parallel execution loses dependency — Finance needs Legal's output to make approval decision; merging independent responses creates incorrect results
- Hierarchical spawning creates unnecessary complexity and unclear ownership; Legal agent shouldn't control Finance agent execution
- Random selection violates the requirement that BOTH agents must process the request

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** An agent uses function calling to execute database DELETE operations. The organization requires human approval before any deletion. Which safeguard implementation is CORRECT?

A) Set tool schema 'require_approval': true and configure approval workflow in agent settings
B) Use autonomous mode but log all deletions to Azure Monitor for post-execution review
C) Add a validation function that checks deletion parameters and auto-approves if valid
D) Implement semiautonomous oversight mode where DELETE functions trigger approval request before execution

<details>
<summary>Answer</summary>

**Correct: D**

Semiautonomous mode allows the agent to prepare the function call but requires human approval before execution. This is the standard pattern for high-risk operations. The agent pauses, presents the planned action, waits for approval, then executes or cancels.

**Why the others are wrong:**
- Tool schemas don't have a built-in 'require_approval' field; approval logic is controlled by agent mode settings, not schema properties
- Autonomous mode with logging is post-execution review — deletion already happened, violating the 'before deletion' requirement
- Auto-approval defeats the purpose of human oversight; validation is technical correctness, not business approval authority

</details>

---

**Q5.** A company needs an agent to answer questions using internal documents (500MB), external web search, and SQL database queries. Document content changes weekly. Which architecture BEST fits these requirements?

A) Multi-agent: coordinator routes to Document agent (RAG), Web agent (search), Data agent (SQL)
B) Single agent with three tools: Azure AI Search index (for docs), Bing Search API, SQL function calling
C) Embed all documents in system prompt, use web search tool, and SQL tool on single agent
D) Use knowledge store for documents, disable web search to reduce costs, rely only on internal data and SQL

<details>
<summary>Answer</summary>

**Correct: B**

Single agent with three distinct tools is most efficient for simple routing scenarios. Agent decides which tool(s) to use based on question. Azure AI Search handles document retrieval with easy weekly reindexing, Bing handles external search, SQL executes database queries. No need for multi-agent complexity here.

**Why the others are wrong:**
- Multi-agent adds orchestration overhead without benefit; this isn't a sequential workflow or specialist expertise scenario — just tool selection
- 500MB cannot fit in system prompt (token limits ~128K tokens ≈ 96MB text); static embedding prevents weekly updates
- Removing web search violates requirements; relying only on internal data means agent cannot answer questions requiring external information

</details>

---

**Q6.** An agent evaluation shows 92% task completion rate, but 40% of completed tasks used incorrect tools (e.g., calling web search instead of internal database). What is the PRIMARY issue to address?

A) Conversation coherence is low — improve conversation memory configuration
B) Agent lacks sufficient tools — add more function calling options to increase coverage
C) Tool usage accuracy is low — refine tool descriptions and provide better examples in system prompt
D) Task completion metric is unreliable — implement better success criteria in evaluation framework

<details>
<summary>Answer</summary>

**Correct: C**

Tool usage accuracy measures whether the agent selects the correct tool for each task. 40% incorrect tool selection indicates poorly defined tool schemas or unclear tool descriptions. The agent completes tasks but uses wrong methods. Fix: improve tool descriptions, add clear use-case examples, refine parameter schemas.

**Why the others are wrong:**
- Conversation coherence affects multi-turn dialogue quality; this is a tool selection problem, not a memory/context issue
- More tools would worsen the problem by adding confusion; the issue is selecting correctly from existing tools, not lack of options
- Task completion is actually high (92%); the metric correctly identifies completed vs failed tasks — the problem is how tasks were completed (wrong tools)

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** A customer service agent must never share internal pricing data (marked 'confidential' in knowledge base) with external users but can share it with employees. Which safeguard pattern should be implemented?

A) Use conversation boundaries to detect user type and filter tool responses before returning to user
B) Configure tool-access controls that check user role before allowing pricing data retrieval functions
C) Create two separate agents: external-facing (no pricing access) and internal-facing (full access)
D) Implement approval workflow where supervisor reviews any pricing-related responses before sending

<details>
<summary>Answer</summary>

**Correct: B**

Tool-access controls check permissions before executing retrieval functions. When agent attempts to call 'get_pricing_data', the control layer checks user role (external vs employee). External users get access denied; employees proceed. This is built-in security at the tool execution level.

**Why the others are wrong:**
- Conversation boundaries define what topics agent can discuss; they don't provide row-level security on data access — pricing data might still be retrieved then filtered (exposure risk)
- Two separate agents double maintenance burden and don't scale; role-based access control on one agent is standard security pattern
- Approval workflow adds human delay to every pricing question, even for employees; defeats real-time support goal and is operationally expensive

</details>

---

**Q8.** An agent orchestration has coordinator agent sending tasks to five specialist agents in parallel. One specialist agent fails due to API timeout. The coordinator must return partial results to the user. Which pattern is MOST appropriate?

A) Switch to sequential handoff pattern to detect failures earlier and prevent partial results
B) Use hierarchical pattern where each specialist spawns a backup agent on failure
C) Implement retry logic in coordinator with exponential backoff until all five agents succeed
D) Configure parallel execution with 'wait_for_all: false' and timeout handling that returns successful agent responses

<details>
<summary>Answer</summary>

**Correct: D**

Parallel execution with 'wait_for_all: false' allows coordinator to collect responses from successful agents and proceed without waiting for failed agent. Timeout handling catches the API timeout, logs it, and coordinator returns partial results with note about unavailable specialist. This matches real-world needs for graceful degradation.

**Why the others are wrong:**
- Sequential handoff wouldn't improve failure detection and would increase total latency; it's designed for dependent workflows, not parallel independent tasks
- Hierarchical spawning of backup agents adds extreme complexity; doesn't solve API timeout root cause and creates cascading resource usage
- Infinite retry blocks user response indefinitely; if API is down/slow, retrying won't help and user gets no answer at all

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

