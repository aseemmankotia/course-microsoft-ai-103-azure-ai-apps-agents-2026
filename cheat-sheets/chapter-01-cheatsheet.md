# AI-103 Chapter 1: Planning and Managing Azure AI Solutions - Cheat Sheet

## Model Selection Decision Matrix

| Requirement | Recommended Model | Cost (per 1M tokens) | Notes |
|-------------|------------------|---------------------|-------|
| Vision + text, complex reasoning | GPT-4o | $2.50 input / $10 output | Multimodal, best accuracy |
| High volume simple queries | GPT-4o mini | $0.15 input / $0.60 output | 15x cheaper than GPT-4o |
| Edge deployment, on-device | Phi-3 mini/medium | $0.10 | 3.8B-14B params, no vision |
| Code generation | GPT-4o or specialized code models | Varies | Check Foundry catalog |
| Long context (>128K tokens) | GPT-4o (128K context) | Standard rates | Context window matters |

## Deployment Options Comparison

| Feature | Serverless | Provisioned Throughput (PTU) |
|---------|-----------|-----------------------------|
| **Pricing** | Pay per token | Fixed monthly (~$300-600/PTU) |
| **Use When** | Variable load, <1M tokens/day | Sustained >100 req/min, predictable |
| **Quota Type** | TPM (tokens/min) + RPM (req/min) | No limits, fixed capacity |
| **Scaling** | Auto-scales within quota | Fixed, no auto-scale |
| **Latency** | Variable (multi-tenant) | Consistent (dedicated) |
| **Break-even** | N/A | ~1M tokens/day minimum |

## Retrieval Methods for RAG

| Method | Best For | Configuration | Exam Gotcha |
|--------|----------|---------------|-------------|
| **Vector search** | Conceptual queries | Embedding model + index | Misses exact term matches |
| **Keyword (BM25)** | Technical terms, acronyms | Text analyzer config | Misses synonyms |
| **Hybrid** | Production RAG systems | Vector + keyword + RRF | Most tested pattern |
| **Semantic ranker** | Temporal relevance, quality | L1 (free, top 50) or L2 (paid, top 2500) | Adds 50-300ms latency |

**Exam Rule**: Temporal queries + technical docs = Hybrid + Semantic ranker

## Authentication Patterns

| Pattern | Use Case | Implementation | Security Level |
|---------|----------|----------------|----------------|
| **Managed Identity (System)** | Single Azure resource auth | `DefaultAzureCredential()` | Keyless (best) |
| **Managed Identity (User)** | Shared identity across resources | Assign MI to resources | Keyless (best) |
| **API Key** | Development only | `AzureKeyCredential(key)` | Low (avoid prod) |
| **SAS Token** | Temporary delegated access | Time-limited token | Medium |

**Exam Tip**: Managed identity = default answer for keyless auth questions

## Azure AI Foundry Infrastructure

```
Hub (shared layer)
├── Compute resources
├── Managed VNet + private endpoints
├── Quotas and billing
└── Projects (isolation layer)
    ├── Development → separate deployments, experiments
    ├── Staging → validation, integration testing  
    └── Production → live endpoints
```

**Network Isolation**: Managed VNet at hub level, NSG rules per project  
**Gotcha**: Cannot create private endpoint per project — it's hub-scoped

## Agent Workflow Components (Required)

1. **Memory Service**: Cosmos DB or Redis for conversation context
2. **Tool Integration**: Function calling config for external APIs
3. **Orchestration**: Prompt flow for multi-step tasks
4. **Knowledge Store**: Vector DB or Azure AI Search for RAG

**Pattern**: Agent queries memory → calls tools via function calling → updates memory

## Cost Optimization Strategies

| Scenario | Strategy | Savings |
|----------|----------|----------|
| Predictable high load | Right-size PTUs (don't over-provision) | 20-40% |
| Peak + off-peak pattern | PTU for baseline + serverless overflow | 15-25% |
| High volume simple tasks | Use mini/small models vs flagship | 10-15x |
| Repetitive queries | Semantic caching layer | 60-90% |
| Token usage | max_tokens limits, prompt compression | 20-40% |

## Quota and Rate Limits (Common Values)

| Service | Quota Type | Typical Limit | Exam Note |
|---------|-----------|---------------|------------|
| Azure OpenAI serverless | TPM | 10K-240K (region/model dependent) | TPM != guaranteed throughput |
| Azure OpenAI serverless | RPM | 300-6000 | Hits RPM after TPM |
| PTU | No TPM/RPM limits | Based on PTU count | Fixed capacity |
| Azure AI Search | QPS (queries/sec) | 3-200 depending on tier | Shared vs dedicated |

## CI/CD Deployment Patterns

**Approval Gate Pattern**:
1. Model registry stores metrics from training
2. Pipeline checks `accuracy >= 0.90` from metadata
3. Gate approves/rejects deployment
4. Blue/green maintains N-1 version for rollback

**Exam Tip**: Don't re-run validation in gate — query registry metadata

## Key Exam Formulas

- **PTU Break-even**: Monthly PTU cost ÷ 30 days ÷ cost per 1M tokens = daily tokens needed
- **Token Budget**: Budget/day ÷ cost per 1M tokens = max tokens/day
- **TPM to Requests**: TPM ÷ avg tokens per request = max requests/minute

**Example**: 50 PTUs at $400/PTU = $20K/month. With GPT-4o at $2.50/1M input tokens:  
Break-even = $20,000 ÷ 30 days ÷ $2.50 = 267M tokens/day = 185K tokens/min
