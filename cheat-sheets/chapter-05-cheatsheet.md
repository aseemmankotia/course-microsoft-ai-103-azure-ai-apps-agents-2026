# Chapter 5: Optimizing and Operationalizing Generative Solutions

## Model Parameters Quick Reference

| Parameter | Range | Low Value Effect | High Value Effect | Exam Use Cases |
|-----------|-------|------------------|-------------------|----------------|
| **temperature** | 0.0 - 2.0 | Deterministic, factual, repeatable | Creative, diverse, risky | Legal/Medical: 0-0.3, Creative: 0.7-1.0 |
| **top_p** | 0.0 - 1.0 | Focused (top 10% tokens) | Broad (90%+ tokens) | Use OR temperature, not both extreme |
| **frequency_penalty** | -2.0 - 2.0 | More repetition | Discourages repeated tokens | Code: 1.0-1.5 for diverse variable names |
| **presence_penalty** | -2.0 - 2.0 | Stay on topic | Explore diverse topics | Multi-topic content: 0.5-1.0 |
| **max_tokens** | 1 - 128000 | Short responses | Long responses | Cost control: set minimum viable |

## Cost Optimization

**Azure OpenAI GPT-4 Pricing (2026)**
- Prompt tokens: $0.03 per 1K tokens
- Completion tokens: $0.06 per 1K tokens (2x prompt cost)

**Cost Reduction Strategies**
1. **Prompt caching**: 80%+ savings on repeated prompt prefixes
2. **Semantic caching**: 40-60% savings by reusing similar query responses
3. **Reduce completion length**: Lower max_tokens where feasible
4. **Use smaller models**: GPT-3.5-Turbo for simple tasks (90% cost reduction)

## Latency Breakdown (RAG Example)

| Phase | Typical % | Optimization |
|-------|-----------|-------------|
| Retrieval | 20% | Index optimization, fewer documents |
| Prompt processing | 30% | Reduce document count, shorter prompts |
| Generation | 50% | Smaller context, lower max_tokens |

**Exam Gotcha**: Reducing documents from 10→5 cuts BOTH prompt processing AND generation time (smaller context)

## Hybrid Architecture Patterns

### Pattern 1: LLM + Rules Engine
**When**: High-accuracy requirements (99%+), deterministic validation needed  
**How**: LLM generates → Rules engine validates against database/protocols → Confidence score assigned  
**Example**: Medical diagnosis validation, legal clause extraction

### Pattern 2: Multi-Model Orchestration
**When**: Cost/speed optimization with quality maintenance  
**How**: Small model classifies → Large model generates (only when needed)  
**Example**: BERT for intent classification (fast, cheap) → GPT-4 for response generation (high quality)

### Pattern 3: Fallback Logic
**When**: Reliability critical, LLM confidence varies  
**How**: Set confidence threshold (0.7-0.9) → Below threshold: trigger rules engine or human review  
**Example**: Financial fraud detection with manual review fallback

## Prompt Engineering Essentials

**Role-Based Prompting**  
`"You are a [specific expert role]. [Task with constraints]."`  
Example: `"You are a Python security expert. Review this code for SQL injection vulnerabilities."`

**Delimiters**  
Use triple quotes, XML tags, or ### to separate sections:  
`"""User input: {input}""" Analyze the above input for...`

**Output Format Specification**  
`"Respond in JSON format: {"summary": "...", "sentiment": "positive|negative|neutral"}"`

**Negative Prompting**  
`"Do not include opinions, speculation, or information not in the source text."`

**Chain-of-Thought**  
`"Let's think step by step:"` or `"Show your reasoning before the final answer."`  
⚠️ Increases token cost but improves accuracy on complex reasoning

## Tracing & Observability

**Token Analytics to Track**
- Prompt tokens vs completion tokens per request
- Token usage per workflow step (retrieval, summarization, generation)
- Cost attribution by user, feature, or model

**Latency Breakdown to Monitor**
- Retrieval time (vector search, database queries)
- Prompt processing time (input token encoding)
- Generation time (completion token generation)
- Post-processing time (validation, formatting)

**Quality Metrics**
- Retrieval relevance scores (top-k accuracy)
- Generation quality (BLEU, ROUGE for summarization)
- Confidence scores from hybrid validation layers

## Common Exam Traps

1. **Temperature confusion**: Temperature does NOT reduce token cost, only affects randomness
2. **Multi-model = always better**: Exam tests when NOT to use (simple tasks don't need GPT-4)
3. **Caching types**: Prompt caching ≠ semantic caching (different mechanisms, different savings)
4. **Penalty parameters**: frequency_penalty for repetition, presence_penalty for topic diversity (not interchangeable)
5. **Latency optimization**: Reducing documents helps prompt processing AND generation, not just retrieval
