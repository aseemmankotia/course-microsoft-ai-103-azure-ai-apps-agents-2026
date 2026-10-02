# Chapter 3 Cheat Sheet: Building Generative Applications with Foundry

## RAG vs Fine-Tuning Decision Matrix
| Use Case | RAG | Fine-Tuning |
|----------|-----|-------------|
| Dynamic knowledge (frequent updates) | ✅ Best | ❌ Expensive retraining |
| Factual QA with citations | ✅ Best | ❌ Cannot cite sources |
| Writing style/tone | ❌ Limited | ✅ Best |
| Task-specific formatting | ❌ Prompt eng. sufficient | ✅ Best for complex |
| Update latency | Minutes (re-index) | Hours/days (retrain) |

## Model Parameters (Critical for Exam)
| Parameter | Range | Purpose | Exam Defaults |
|-----------|-------|---------|---------------|
| temperature | 0-2 | Randomness (0=deterministic, 2=chaotic) | 0.7 creative, 0.2 factual |
| top_p | 0-1 | Nucleus sampling (cumulative probability) | 0.95 OR use temp (not both) |
| max_tokens | 1-model_max | Response length limit | Model-specific (GPT-4: 4096-128K) |
| frequency_penalty | 0-2 | Penalize repetition by count | 0.5-1.0 typical, 2.0 = max |
| presence_penalty | 0-2 | Penalize if token appeared | 0.5-1.0 typical |

## Prompt Engineering Patterns
**Chain-of-Thought (CoT)**
```
System: "Solve step-by-step, showing your reasoning."
User: "Problem: [query]. Let's approach this systematically:"
```
Exam note: CoT = reasoning PROCESS visibility, not accuracy.

**Few-Shot**
```
System: "You are [role]."
User: "Example 1: Q: [q1] A: [a1]\nExample 2: Q: [q2] A: [a2]\nNow: [actual query]"
```
Exam note: 2-5 examples. Demonstrates format, not knowledge storage.

**Grounding (Anti-Fabrication)**
```
System: "Answer ONLY using provided context below. Cite chunk IDs. If info not in context, say 'I don't have that information.'"
User: "Context: [chunks]\n\nQuestion: [query]"
```
Exam note: Explicit constraints > temperature tuning for fabrication.

## Foundry SDK Core Patterns
**Chat Completion (Basic)**
```python
from azure.ai.projects import AIProjectClient
client = AIProjectClient.from_connection_string(conn_str)
response = client.inference.complete(
    messages=[{"role":"user","content":"Hello"}],
    model="gpt-4",
    temperature=0.7
)
print(response.choices[0].message.content)
```

**Streaming**
```python
response = client.inference.complete(messages=[...], stream=True)
for chunk in response:
    if chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="")
# Final chunk contains usage: prompt_tokens, completion_tokens, total_tokens
```
Exam note: stream=True enables streaming. Usage in final chunk.

**Function Calling (Tools)**
```python
tools = [{
    "type": "function",
    "function": {
        "name": "get_inventory",
        "description": "Check product stock",
        "parameters": {"type":"object", "properties":{"product_id":{"type":"string"}}}
    }
}]
response = client.inference.complete(messages=[...], tools=tools)
if response.choices[0].message.tool_calls:
    # Model wants to call function - app executes, adds result, calls again
```
Exam note: Model decides when, app executes. Best pattern for external data.

## Hybrid Retrieval (Azure AI Search + RAG)
1. **Vector search**: Embed query → find similar embeddings (semantic)
2. **Keyword search**: BM25 full-text (exact matches)
3. **Combine**: RRF (Reciprocal Rank Fusion) merges results
4. **Semantic ranker**: Re-ranks top K results

Exam note: Hybrid = best accuracy. Vector-only misses exact matches.

## Evaluation Metrics (LLM-as-Judge)
| Metric | Scale | Definition |
|--------|-------|------------|
| Relevance | 1-5 | Answer matches question intent |
| Coherence | 1-5 | Logical flow, no contradictions |
| Fluency | 1-5 | Grammar, readability |
| Fabrication | Yes/No | Unsupported claims detected |

**Best Practice**: GPT-4 judges + 10% human spot-checks.
Exam trap: BLEU/ROUGE inappropriate for open-ended chat (use for translation/summarization only).

## Common Exam Traps
1. **Temperature ≠ Factuality**: temperature=0 makes output deterministic, NOT more factual. Use grounding for accuracy.
2. **Fine-tuning ≠ Knowledge**: Fine-tuning teaches behavior/style. Use RAG for dynamic facts.
3. **Context window limits**: GPT-4-turbo = 128K tokens max. Can't fit all knowledge. Use RAG + retrieval.
4. **Penalty max = 2.0**: frequency_penalty/presence_penalty max out at 2.0. Typically use 0.5-1.0.
5. **Multi-answer questions**: Exam uses single-answer only (A/B/C/D). One correct option.
6. **Streaming token counts**: Available in FINAL chunk only, not per-chunk.

## Tracing & Observability
Foundry SDK auto-captures: prompts, completions, token counts, latency, parameters.
Enable: Built-in (no config needed). Query via Azure ML workspace.
Exam note: Use built-in tracing before custom logging solutions.
