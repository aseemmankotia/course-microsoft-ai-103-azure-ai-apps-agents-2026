# Chapter 2: Monitoring, Securing, and Responsible AI Controls

## Quota & Performance

| Metric | Description | Exam Tip |
|--------|-------------|----------|
| TPM (Tokens Per Minute) | Sustained throughput quota | Per-minute limit |
| Burst Limit | ~20% of TPM per 10 seconds | Causes 429 errors during spikes even if avg TPM low |
| Model Drift | Latency/quality changes over time | Monitor identical prompts for performance trends |
| 429 Error | Rate limit exceeded | Check burst limit first, not just per-minute quota |

## Safety Layers (When to Use Which)

| Feature | Use Case | What It Blocks |
|---------|----------|----------------|
| **Content Filters** | Harmful content detection | Violence, hate, sexual, self-harm (severity 0-6) |
| **Prompt Shields** | Prompt injection attacks | Direct attacks (user), indirect attacks (docs/tools) |
| **Custom Blocklists** | Known bad terms/patterns | Static keyword lists (exact/fuzzy match) |

### Content Filter Severity Levels
- **0-1:** Very strict (most false positives)
- **2:** Default (balanced)
- **4-5:** Permissive (fewer false positives, e.g. coding terms)
- **6:** Only extreme violations

**Rule:** Increase level number to allow more content through

## RAG Evaluation Metrics

| Metric | Measures | Detects |
|--------|----------|----------|
| **Groundedness** | Answer supported by docs | Fabrication/hallucination |
| **Relevance** | Docs match query | Poor retrieval |
| **Provenance** | Source attribution | Which doc → which answer part |

**Exam Focus:** Fabrication = groundedness, not relevance

## Agent Oversight

| Mode | Behavior | Use When |
|------|----------|----------|
| **Autonomous** | Executes all tools without approval | Fully trusted operations |
| **Semiautonomous** | Proposes actions, waits for approval | High-stakes operations ($ thresholds, sensitive data) |

### Tool-Access Controls
- **Pre-execution hooks:** Run code before tool executes (e.g. logging, validation)
- **Post-execution validation:** Check results before returning to user
- **Rate limits:** Per-tool throttling

**Example:** "Must log before DB query" → pre-execution hook, not approval workflow

## Security

| Method | What It Secures | Exam Tip |
|--------|-----------------|----------|
| **Private Endpoints** | Network access (VNet) | Doesn't authenticate — combine with auth method |
| **Managed Identity** | Keyless API authentication | "No keys in code" = managed identity, not Key Vault |
| **RBAC Roles** | Cognitive Services OpenAI User | For inference; separate role for management |

### Key Vault vs Managed Identity
- **Key Vault:** Still uses keys (just stored securely)
- **Managed Identity:** No keys at all — Azure AD authentication

**Exam Trap:** "No keys" means managed identity, not Key Vault

## Monitoring Safety Events

```bash
# Log types to monitor
- Content filter violations (by category/severity)
- Prompt injection attempts (blocked by shields)
- Grounding failures (fabricated responses)
- Tool execution failures
- Throttling errors (429)
```

## Commands

```bash
# Enable prompt shields
az cognitiveservices account deployment create \
  --prompt-shield-mode Enabled

# Configure content filters (severity per category)
az cognitiveservices account deployment update \
  --content-filter-config violence=4,hate=2,sexual=2,self-harm=2

# Assign managed identity role
az role assignment create \
  --role "Cognitive Services OpenAI User" \
  --assignee-object-id <managed-identity-id>
```

## Common Exam Scenarios

1. **Bursting errors despite low avg TPM** → Burst limit (10-sec), not per-minute quota
2. **Coding assistant flagging 'kill process'** → Raise violence severity (e.g. 2→4)
3. **Prevent revealing system prompt** → Prompt shields (indirect attacks)
4. **Detect fabrication in RAG** → Groundedness metric
5. **High-value transaction approval** → Semiautonomous mode with approval workflow
6. **No keys in code** → Managed identity, not Key Vault
7. **Must audit before DB access** → Tool pre-execution hook
8. **Latency increasing over weeks** → Model drift — redeploy
