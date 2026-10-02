## Chapter 8: Text Analysis and Speech Solutions — Practice Questions

**Q1.** A healthcare company needs to extract patient information (name, date of birth, medications, diagnoses) from unstructured clinical notes and store them in a structured database. The solution must ensure consistent field types and validate that dates are in ISO format. Which approach BEST meets these requirements?

A) Use Azure AI Language Named Entity Recognition (NER) with custom entity types and post-process the results with validation rules
B) Use Azure AI Document Intelligence to extract text, then apply regex patterns to identify and validate each field
C) Use Azure Text Analytics sentiment analysis to identify medical terms, then manually structure the output
D) Use Azure OpenAI with JSON schema enforcement in the response format, specifying required fields and data types in the schema

<details>
<summary>Answer</summary>

**Correct: D**

Azure OpenAI with JSON schema enforcement (response_format with json_schema) guarantees structured output matching your exact schema, including field validation and type enforcement. This is the modern approach for extracting structured data from unstructured text — the LLM understands context better than traditional NER and the schema ensures consistency.

**Why the others are wrong:**
- Traditional NER doesn't guarantee structured output format or field validation — you'd need significant post-processing and it may miss context-dependent entities
- Document Intelligence is designed for forms and documents with visual layout, not unstructured clinical notes. Regex patterns are brittle and won't handle variation in natural language
- Sentiment analysis detects emotional tone, not entity extraction. This is the wrong tool entirely for structured data extraction

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** An AI agent needs to handle customer calls in real-time: listen to the customer's speech, process the request, and respond with synthesized speech. The agent must support custom pronunciation for product names and maintain conversational flow with minimal latency. Which Azure services combination is MOST appropriate?

A) Azure AI Speech (speech-to-text) → Azure OpenAI (processing) → Azure AI Speech (text-to-speech with custom neural voice)
B) Azure AI Language Understanding (LUIS) → Azure Bot Service → Azure Media Services for audio
C) Azure Cognitive Search for audio indexing → Azure Functions → Azure Communication Services
D) Azure Video Indexer for speech extraction → Azure Logic Apps → Azure Text Analytics

<details>
<summary>Answer</summary>

**Correct: A**

Azure AI Speech provides both real-time speech-to-text and text-to-speech with custom neural voice support (for product name pronunciation). Azure OpenAI handles the conversational reasoning. This is the standard speech-agent modality pattern — input through STT, process with LLM, output through TTS.

**Why the others are wrong:**
- LUIS is being deprecated and doesn't provide speech synthesis. Azure Media Services is for media streaming, not real-time conversational TTS
- Cognitive Search indexes content for search, it doesn't provide real-time speech processing. Communication Services handles calling infrastructure, not speech-to-text/text-to-speech
- Video Indexer is for batch processing of recorded videos, not real-time conversation. Logic Apps adds unnecessary latency for real-time speech flows

</details>

---

**Q3.** A legal firm needs to translate 50,000 legal contracts from English to Spanish while preserving exact legal terminology and clause structure. The contracts contain domain-specific terms that must be translated consistently. Azure Translator provides a custom translation model feature. Which approach provides the MOST accurate translations?

A) Use Azure OpenAI GPT-4 with prompts instructing it to translate while preserving legal terminology, processing each contract separately
B) Use Azure AI Language detection followed by Azure Translator standard translation without customization
C) Use Azure OpenAI with few-shot examples of legal translations embedded in the system prompt for each contract
D) Use Azure Translator custom models trained on parallel legal documents, with a terminology dictionary for specific legal terms

<details>
<summary>Answer</summary>

**Correct: D**

Azure Translator custom models are specifically designed for domain-specific translation at scale. Training on parallel legal documents and adding a terminology dictionary ensures consistent translation of legal terms across all 50,000 contracts. This is more cost-effective and consistent than LLM translation for high-volume, domain-specific work.

**Why the others are wrong:**
- GPT-4 can translate well but lacks the terminology consistency guarantee and is significantly more expensive for 50,000 contracts. Each translation is independent without enforced term consistency
- Standard Azure Translator without customization will mistranslate domain-specific legal terms — it's trained on general content, not legal documents
- Few-shot prompting doesn't scale to 50,000 documents and won't guarantee terminology consistency. Each API call is stateless and may translate the same term differently

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** A customer service application analyzes chat transcripts to detect conversations that contain threats, harassment, or requests for illegal activities. The system must flag these for human review while summarizing the nature of the safety concern. Which Azure service provides the MOST direct solution?

A) Azure Text Analytics sentiment analysis with custom threshold rules for negative sentiment
B) Azure AI Content Safety API to detect harmful content categories, combined with Azure OpenAI for summarization
C) Azure OpenAI with prompt engineering to detect safety issues and generate summaries in a single call
D) Azure AI Language custom text classification trained on labeled safety violation examples

<details>
<summary>Answer</summary>

**Correct: B**

Azure AI Content Safety is specifically built for detecting threats, harassment, sexual content, violence, and self-harm in text. It provides severity scores and categories. Combining it with Azure OpenAI for summarization gives you specialized safety detection plus context-aware summarization — best of both worlds.

**Why the others are wrong:**
- Sentiment analysis detects positive/negative/neutral tone, not safety violations. Negative sentiment doesn't mean harassment or threats
- While GPT-4 can detect some safety issues, it's not as specialized or reliable as Content Safety API for this specific task. You'd also miss the structured severity scoring that Content Safety provides
- Custom classification requires significant labeled training data and ongoing maintenance. Content Safety API is pre-trained on comprehensive safety categories

</details>

---

**Q5.** A financial services company needs to extract specific entities (account numbers, transaction amounts, dates, institution names) from customer emails and ensure the output contains all required fields before processing. Missing fields should trigger an error. Which implementation approach ensures field completeness?

A) Use Azure AI Language NER with custom entity types, then check for null values in post-processing
B) Use regex patterns to extract entities, wrapping extraction in try-catch blocks to handle missing fields
C) Use Azure Form Recognizer custom models trained on email layouts with field extraction confidence thresholds
D) Use Azure OpenAI with required fields specified in the JSON schema response format, and validate the response contains all fields

<details>
<summary>Answer</summary>

**Correct: D**

Azure OpenAI JSON schema with 'required' fields specified forces the model to populate all specified fields. The schema validation happens at the API level, and you can validate the response object structure immediately. This is the most reliable way to ensure field completeness for unstructured text extraction.

**Why the others are wrong:**
- NER doesn't guarantee all entities are found — if an entity isn't in the text or isn't recognized, you get null/missing values. Post-processing catches this but doesn't solve the core problem
- Regex patterns fail on natural language variation and won't extract context-dependent entities (e.g., distinguishing transaction amounts from account balances). Try-catch handles errors but doesn't improve extraction
- Form Recognizer is for documents with consistent visual layout, not free-form emails. Email layouts vary too much for form-based extraction

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** An AI agent needs to process customer support calls in multiple languages. Customers speak in their native language, and the agent must respond in the same language. The agent uses Azure OpenAI for reasoning. Which architecture minimizes latency while supporting 20+ languages?

A) Azure AI Speech (speech-to-text with auto language detection) → Azure OpenAI multilingual model (GPT-4) → Azure AI Speech (text-to-speech in detected language)
B) Azure AI Speech (speech-to-text) → Azure Translator (translate to English) → Azure OpenAI (English only) → Azure Translator (translate back) → Azure AI Speech (text-to-speech)
C) Azure AI Language (language detection) → Azure AI Speech (speech-to-text) → separate Azure OpenAI calls per language → Azure AI Speech (text-to-speech)
D) Azure Video Indexer (speech extraction and translation) → Azure OpenAI → Azure Media Services (audio generation)

<details>
<summary>Answer</summary>

**Correct: A**

Modern Azure OpenAI models (GPT-4, GPT-4o) are multilingual — they understand and respond in 50+ languages natively. Azure AI Speech supports auto language detection and multilingual TTS. This direct flow (STT → LLM → TTS) avoids translation latency and preserves nuance. The LLM infers response language from input language.

**Why the others are wrong:**
- Double translation (input and output) adds significant latency and degrades quality through translation errors compounding. The LLM can handle multiple languages natively
- Separate OpenAI calls per language is unnecessary — GPT-4 handles all languages in one model. Language detection before STT adds a redundant step (STT can auto-detect)
- Video Indexer is for batch processing of recorded videos, not real-time calls. It adds massive latency and uses the wrong tools for live conversation

</details>

---

**Q7.** A compliance team needs to analyze 10,000 recorded sales calls to extract: mention of specific regulated products, confirmation that required disclosures were read, and tone of the conversation. The solution must generate a structured compliance summary for each call. Which approach is MOST efficient?

A) Azure Video Indexer for speech-to-text → Azure Text Analytics sentiment → manual review for disclosures
B) Azure AI Speech batch transcription → Azure OpenAI with structured JSON output for entity extraction, disclosure detection, and sentiment analysis
C) Azure AI Speech real-time transcription → Azure AI Language custom NER → Azure Text Analytics sentiment → combine results
D) Azure Communication Services call recording → Azure Cognitive Search with AI enrichment for entity extraction

<details>
<summary>Answer</summary>

**Correct: B**

Azure AI Speech batch transcription is designed for high-volume recorded audio (more cost-effective than real-time for 10,000 calls). Azure OpenAI with JSON schema can extract regulated products, detect disclosure phrases (via prompting with exact disclosure text), and analyze tone — all in one call with structured output. This is the modern pattern for compliance summarization.

**Why the others are wrong:**
- Video Indexer adds unnecessary overhead for audio-only files. Manual review for disclosures doesn't scale to 10,000 calls and defeats the automation purpose
- Real-time transcription is more expensive than batch for pre-recorded calls. Custom NER requires training data and doesn't handle context-dependent disclosure detection well. Combining three separate services adds complexity
- Cognitive Search with AI enrichment is designed for document search/discovery, not structured compliance analysis. It doesn't provide the same level of structured output and reasoning as an LLM

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** A medical transcription service needs to transcribe doctor-patient conversations with high accuracy for medical terminology (drug names, procedures, anatomical terms). The default speech-to-text model frequently misspells specialized terms. What is the MOST effective way to improve accuracy?

A) Create a custom speech model in Azure AI Speech trained on medical audio samples and provide a phrase list of medical terms
B) Use Azure OpenAI Whisper model with medical terminology in the prompt to guide transcription
C) Use standard Azure AI Speech with post-processing LLM calls to correct medical terms
D) Use Azure Video Indexer with custom language model for medical vocabulary

<details>
<summary>Answer</summary>

**Correct: A**

Azure AI Speech custom speech models allow training on domain-specific audio (medical conversations) and phrase lists boost recognition of specific medical terms. This directly improves transcription accuracy at the source. Custom speech is the recommended approach for specialized terminology and accents.

**Why the others are wrong:**
- Whisper is a capable speech model but Azure doesn't expose it as a customizable service with phrase lists or custom training. Prompting doesn't affect speech-to-text accuracy in the same way as acoustic/language model training
- Post-processing with LLM is a workaround, not a solution. It adds latency, cost, and potential for introducing new errors. Better to fix transcription accuracy at the source
- Video Indexer is for batch processing of video content, not real-time or audio-only medical transcription. It doesn't offer the same level of customization as custom speech models

</details>

---

