# AI-103 Full Exam Simulation - Quick Reference

## Exam Format
| Element | Details |
|---------|--------|
| **Questions** | 40-60 (varies by exam version) |
| **Time** | 100-120 minutes |
| **Question Types** | Multiple choice, multiple select, drag-and-drop, case studies, hot area |
| **Passing Score** | ~700/1000 (70%) |
| **Case Studies** | 1-3 scenarios with 4-6 questions each |

## Time Management Strategy
| Scenario | Strategy |
|----------|----------|
| **Standard questions** | 1.5-2 min each, flag if >3 min |
| **Case studies** | 10-15 min per case, read once, answer easy questions first |
| **60 min left, 25 questions** | ~2.4 min/question pace - maintain strict timing |
| **Difficult question** | Eliminate 1-2 options, guess, flag for review |
| **Last 10 minutes** | Review flagged questions only if all answered |

## Domain Breakdown (Study Priority)
| Domain Weight | Focus Level | Topics |
|---------------|-------------|--------|
| **30-35%** | Deep study | Generative AI (Azure OpenAI), AI agents, RAG architecture |
| **20-25%** | Moderate | Document Intelligence, Computer Vision, Language services |
| **10-15%** | Light review | Content Safety, Speech, Custom Vision |

## Common Exam Traps
| Trap | Wrong Answer | Right Answer |
|------|--------------|-------------|
| **Auth** | API keys in env vars | Managed identity / Workload Identity |
| **OpenAI Deployment** | Standard for SLA requirement | Provisioned (guaranteed throughput/latency) |
| **Search** | Vector search for keyword query | Hybrid search (vector + BM25) |
| **Document AI** | Custom Vision for document extraction | Document Intelligence (prebuilt models) |
| **Fabrication** | Content Safety filters | Grounding with citations |

## Responsible AI Question Pattern
| Risk Mentioned | Correct Control |
|----------------|----------------|
| **Prompt injection / jailbreak** | Prompt shields |
| **Harmful output (hate, violence)** | Content Safety filters |
| **Fabricated facts** | Grounding with citations |
| **Bias in training data** | Fairness evaluation, diverse datasets |

## Service Selection Decision Tree
```
Conversation + Data Sources + State Management?
→ Azure AI Agent Service

Document data extraction (forms, invoices)?
→ Document Intelligence (NOT Custom Vision)

Text search with semantic understanding?
→ AI Search (hybrid if keywords matter, vector if purely semantic)

Generative text with guaranteed latency?
→ Azure OpenAI Provisioned (NOT Standard)

Image classification/object detection?
→ Computer Vision (prebuilt) OR Custom Vision (custom categories)
```

## Deployment Model Cheat Sheet
| Requirement | Azure OpenAI Deployment |
|-------------|------------------------|
| Pay per token, variable load | Standard |
| Consistent latency SLA | Provisioned |
| High volume (>100K tokens/min sustained) | Provisioned |
| Dev/test with low usage | Standard |

## Elimination Strategy
1. **Read question stem** - identify core requirement (cost, latency, security, etc.)
2. **Eliminate obviously wrong** - wrong service type, deprecated options
3. **Compare remaining on key dimension** - if 2 left, compare on cost vs complexity vs requirements
4. **Security questions** - managed identity > certificates > keys
5. **Architecture questions** - simpler solution wins if both meet requirements

## Case Study Approach
1. **Read scenario once** (2-3 min) - note requirements, constraints, existing infrastructure
2. **Answer easy questions first** (1-2 min each) - capture quick wins
3. **Flag complex questions** - move on, return later
4. **Don't re-read entire scenario** - refer to specific sections as needed
5. **Time budget** - 10-15 min total per case study, not per question

## Flag and Review Protocol
- **Flag immediately** if question >3 minutes
- **Don't leave blank** - guess among narrowed options before flagging
- **Review flagged only** if you finish all questions with 10+ min remaining
- **Change answer only** if you find clear evidence you were wrong

## Final 10 Minutes
- **Do NOT** review answered questions unless flagged
- **Do** ensure all questions have an answer selected
- **Do** review any flagged questions where you had 50/50 guess
- **Do NOT** second-guess answers you felt confident about
