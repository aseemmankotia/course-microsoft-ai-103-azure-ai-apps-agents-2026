# Text Analysis and Speech Solutions — Exam Cheat Sheet

## Structured Output Extraction

| Approach | Use Case | Guarantees |
|----------|----------|------------|
| **Azure OpenAI JSON Schema** | Unstructured text → structured DB | Field presence, type validation, schema compliance |
| **Traditional NER** | Simple entity extraction | Entity labels only, no structure |
| **Form Recognizer** | Forms/documents with layout | Layout-based extraction, not free-form text |

**Exam Key:** Use JSON schema enforcement (`response_format` with `json_schema`) for guaranteed structured output from unstructured text. Specify `required` fields in schema.

## Speech Agent Modality Pattern

```
Audio Input → Azure AI Speech (STT) → Azure OpenAI (Reasoning) → Azure AI Speech (TTS) → Audio Output
```

**Key Points:**
- Auto language detection: specify candidate languages
- Custom neural voice: for custom pronunciation (product names)
- Phrase lists: boost recognition of specific terms
- Batch transcription: cheaper for pre-recorded audio (10K+ files)
- Real-time: for live conversations, more expensive

## Translation Decision Matrix

| Scenario | Service | Why |
|----------|---------|-----|
| 50K legal docs, consistent terminology | **Azure Translator custom model** | Scale, cost, terminology dictionary |
| Context-aware, nuanced translation | **Azure OpenAI** | Understands context, creative translation |
| Real-time audio translation | **Azure AI Speech translation** | Direct speech-to-text in target language |
| Multilingual chat (20+ languages) | **Azure OpenAI (GPT-4)** | Native multilingual, no translation needed |

**Exam Trap:** Don't translate before/after GPT-4 for multilingual support — it handles 50+ languages natively.

## Content Safety vs Sentiment Analysis

| Service | Detects | Output |
|---------|---------|--------|
| **Azure AI Content Safety** | Hate, sexual, violence, self-harm | Severity scores 0-7 per category |
| **Text Analytics Sentiment** | Positive, negative, neutral tone | Sentiment label + confidence |

**Exam Key:** Safety violations ≠ negative sentiment. Use Content Safety for threats/harassment, not sentiment analysis.

## Custom Speech Models

**Customization Types:**
1. **Acoustic model:** Train on audio samples + transcripts (domain audio)
2. **Language model:** Phrase lists + text corpus (specialized vocabulary)

**When to Use:**
- Medical/legal terminology
- Non-standard accents or audio conditions
- Product names or jargon

**Quick Fix:** Phrase lists (no training required) boost recognition of specific terms.

## Key Service Limits

| Service | Limit | Exam Relevance |
|---------|-------|----------------|
| Azure AI Speech batch | 1000 concurrent jobs | Use for high-volume transcription |
| Azure Translator custom | 10 custom models per subscription | Plan for multi-domain translation |
| Content Safety text | 10,000 characters per request | Chunk long documents |
| OpenAI JSON schema | Max 20KB schema size | Keep schemas focused |

## Common Exam Scenarios

**Scenario 1:** Extract structured data from unstructured text with required fields  
**Answer:** Azure OpenAI with JSON schema, specify `required` fields

**Scenario 2:** Voice agent in 20+ languages  
**Answer:** AI Speech (auto-detect) → GPT-4 (native multilingual) → AI Speech TTS

**Scenario 3:** 10K legal contracts, consistent terminology translation  
**Answer:** Azure Translator custom model with terminology dictionary

**Scenario 4:** Detect threats/harassment in chat  
**Answer:** Azure AI Content Safety, not sentiment analysis

**Scenario 5:** Medical terminology transcription accuracy  
**Answer:** Custom speech model with medical phrase list

**Scenario 6:** Compliance summarization from sales calls  
**Answer:** Batch transcription → OpenAI with structured output (entities + disclosure detection + sentiment)

## Command Syntax Examples

**Azure OpenAI JSON Schema (Python SDK):**
```python
response = client.chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": "Extract entities from: ..."}],
    response_format={
        "type": "json_schema",
        "json_schema": {
            "name": "entity_extraction",
            "schema": {
                "type": "object",
                "properties": {
                    "account_number": {"type": "string"},
                    "amount": {"type": "number"}
                },
                "required": ["account_number", "amount"]
            }
        }
    }
)
```

**Azure AI Speech with Phrase List:**
```python
phrase_list = speechsdk.PhraseListGrammar.from_recognizer(recognizer)
phrase_list.addPhrase("Azure OpenAI")
phrase_list.addPhrase("GPT-4")
```

**Exam Tip:** JSON schema with `required` fields = guaranteed field presence. Phrase lists = quick vocabulary boost without custom model training.
