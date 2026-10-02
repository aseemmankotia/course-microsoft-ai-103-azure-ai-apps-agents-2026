## Chapter 9: Retrieval and Information Extraction Pipelines — Practice Questions

**Q1.** A company is building a RAG system to search 50,000 technical documents. Users need to find documents using natural language queries like 'How do I configure SSL certificates?' and also filter by document type and date. The solution must return semantically relevant results ranked by meaning, not just keyword matches. Which Azure AI Search configuration BEST meets these requirements?

A) Create a search index with text fields only, enable the standard Lucene analyzer, and use simple query syntax with keyword filters
B) Create a search index with vector fields for embeddings and text fields for metadata, enable semantic ranking, configure hybrid search with vector and keyword queries, and add filterable fields for document type and date
C) Create a search index with vector fields only, use cosine similarity search, and implement client-side filtering on returned results
D) Create a search index using the legacy cognitive skills pipeline with entity extraction, then query using full-text search with boosting

<details>
<summary>Answer</summary>

**Correct: B**

Hybrid search (vector + keyword) with semantic ranking provides the best retrieval quality for natural language queries while maintaining exact-match capabilities. Vector fields enable semantic similarity search, text fields support keyword matching and filtering, semantic ranking re-ranks results by meaning. Filterable fields on document type and date enable efficient filtering without scanning all results. This is the recommended pattern for production RAG systems in Azure AI Search.

**Why the others are wrong:**
- Text fields with keyword search alone cannot handle semantic similarity queries like 'How do I configure SSL certificates?' — it will miss documents with synonyms or paraphrases. No vector embeddings means no semantic understanding.
- Vector-only search lacks the precision of keyword matching for exact terms and technical phrases. Client-side filtering is inefficient and slow for 50,000 documents — filtering must happen at index query time using filterable fields for performance.
- The cognitive skills pipeline is for enrichment during indexing (OCR, entity extraction), not for retrieval. Full-text search with boosting doesn't provide semantic similarity — it's still keyword-based matching with weighted terms.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A legal firm needs to extract structured data from 10,000 scanned contracts including party names, dates, monetary amounts, and table data showing payment schedules. The contracts are multi-page PDFs with varying layouts. The extracted data must be used in downstream workflows. Which Azure service provides the MOST complete solution?

A) Azure AI Vision OCR API to extract text, then use regex patterns to find entities and tables
B) Azure AI Search enrichment pipeline with OCR skill, entity recognition skill, and custom skill for table extraction
C) Azure Content Understanding with multimodal document analyzer for layout and entity extraction, then custom parsing for tables
D) Azure AI Document Intelligence prebuilt contract model to extract layout, entities, tables, and key-value pairs in a single API call

<details>
<summary>Answer</summary>

**Correct: D**

Azure AI Document Intelligence has a prebuilt contract model specifically designed for legal contracts. It extracts layout (reading order, pages), entities (parties, dates, amounts), tables, and key-value pairs in a single API call. Returns structured JSON output ready for downstream workflows. This is purpose-built for this exact scenario and requires no training or custom code.

**Why the others are wrong:**
- OCR only extracts raw text without structure. Regex patterns are brittle and fail on varying layouts — they cannot reliably extract entities across different contract formats. Table extraction via regex is nearly impossible for complex multi-column tables.
- Azure AI Search enrichment is designed for building search indexes, not for structured data extraction workflows. The pipeline adds latency and complexity when you just need direct API extraction. Indexing 10,000 documents when you don't need search is inefficient and costly.
- Content Understanding is for multimodal scenarios (images + text) and general document analysis. For structured forms and contracts, Document Intelligence prebuilt models provide higher accuracy and built-in table extraction. Content Understanding would require more custom post-processing to extract specific contract fields.

</details>

---

**Q3.** A RAG system chunks documents into 512-token segments with 50-token overlap, generates embeddings using text-embedding-3-small (1536 dimensions), and stores them in Azure AI Search. Retrieval returns low-quality results — users report getting chunks from the middle of paragraphs without context. Which change would MOST improve retrieval quality?

A) Switch to semantic chunking based on document structure (paragraphs, sections) instead of fixed token counts, maintain overlap between chunks, and include parent document metadata in each chunk
B) Increase chunk size to 2048 tokens and remove overlap to reduce index storage costs
C) Reduce embedding dimensions to 768 using a smaller model to improve search speed
D) Increase overlap to 200 tokens and add more chunks per document to the index

<details>
<summary>Answer</summary>

**Correct: A**

Semantic chunking respects document structure — chunks align with natural boundaries (paragraphs, sections) rather than arbitrary token counts. This preserves context and ensures chunks contain complete thoughts. Adding parent document metadata (title, section heading) to each chunk provides context even when the chunk is retrieved alone. Overlap between semantic chunks ensures important information at boundaries isn't lost. This is the recommended production pattern for high-quality RAG.

**Why the others are wrong:**
- Larger chunks (2048 tokens) dilute relevance — the embedding represents too much diverse content, reducing precision in retrieval. Removing overlap loses context at chunk boundaries. This makes the problem worse, not better.
- Smaller embeddings reduce semantic richness and retrieval quality. Search speed is rarely the bottleneck in RAG systems — quality is. 1536 dimensions with text-embedding-3-small is already efficient.
- More overlap without fixing the semantic chunking issue just creates more low-quality chunks. The core problem is chunks split in the middle of ideas — more overlap doesn't fix that. This increases index size and costs without improving quality.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** A company is building an Azure AI Search enrichment pipeline to process scanned invoices. The pipeline must extract text via OCR, detect language, extract key phrases, and identify entities (company names, amounts). Processing 100,000 invoices takes 40 hours and costs exceed budget. How can the pipeline be optimized for cost and speed?

A) Remove the language detection skill since all invoices are in English, and replace entity recognition with a custom skill using a smaller local model
B) Enable skillset caching to reuse enrichment results for unchanged documents, reduce the OCR skill DPI setting to 150 (from 300), and use batch indexing with larger batch sizes
C) Increase the indexer batch size to 1000 documents and run multiple indexers in parallel
D) Switch from built-in skills to Azure Functions custom skills for all enrichment steps to reduce per-transaction costs

<details>
<summary>Answer</summary>

**Correct: B**

Skillset caching stores enrichment results in Azure Storage — if a document hasn't changed, cached results are reused instead of re-running expensive skills (OCR, entity recognition). This dramatically reduces cost and time for re-indexing. Reducing OCR DPI from 300 to 150 is acceptable for printed invoices and reduces OCR processing time by ~50%. Larger batch sizes reduce overhead. These are the recommended cost optimization techniques in Azure AI Search documentation.

**Why the others are wrong:**
- If not all invoices are in English, removing language detection breaks downstream skills that depend on language. Even if all are English now, hardcoding this assumption is brittle. Replacing built-in entity recognition with a custom model adds complexity and may reduce accuracy — built-in skills use production-grade models.
- Batch size cannot exceed 1000 in Azure AI Search, and setting it to max doesn't necessarily improve speed — it can cause throttling and failures. Running multiple indexers in parallel on the same index causes conflicts and errors. This approach violates Azure AI Search limits and best practices.
- Custom skills via Azure Functions add latency (HTTP calls), complexity (managing Function deployments), and often cost MORE than built-in skills due to Function execution time and cold starts. Built-in skills are optimized and run in-process. This is the opposite of optimization.

</details>

---

**Q5.** An agent application uses Azure AI Search as a knowledge base. When users ask questions, the agent retrieves relevant chunks and passes them to GPT-4 for answer generation. Users report that answers sometimes contradict each other across sessions, and the agent cites outdated information. Which solution BEST addresses these issues?

A) Increase the number of retrieved chunks from 3 to 10 to provide more context to the LLM
B) Switch from hybrid search to vector-only search to improve semantic relevance
C) Implement a separate vector database (like Pinecone) alongside Azure AI Search for better embedding quality
D) Add a timestamp field to each document chunk, configure the search query to filter for documents updated within the last 30 days, and enable semantic ranking to prioritize recent content

<details>
<summary>Answer</summary>

**Correct: D**

Adding timestamps and filtering by recency ensures the agent uses current information — the root cause of citing outdated data. Semantic ranking prioritizes content by meaning within the time-filtered results, improving relevance. This is a targeted fix for the stated problem: outdated information and contradictions from old vs new content.

**Why the others are wrong:**
- Retrieving 10 chunks instead of 3 adds noise and contradictory information (especially if old and new versions of docs coexist), making the LLM output worse. More context ≠ better answers. This increases token costs and latency without addressing the outdated data problem.
- Vector-only search loses the precision of keyword matching for exact terms and filters. Hybrid search performs better for RAG because it combines semantic similarity with exact matching. Removing keyword search doesn't solve the outdated data issue.
- Azure AI Search supports vector embeddings natively — adding a separate vector database duplicates functionality and adds complexity. Embedding quality is determined by the model (text-embedding-3-small, etc.), not the database. This adds cost and integration overhead for no benefit.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** A company needs to process 5,000 multimodal documents containing images, charts, handwritten notes, and typed text. The documents are research reports where visual elements (charts, diagrams) are as important as text. The extracted data must include layout analysis, entities from both text and images, and table structures. Which service provides the MOST comprehensive extraction?

A) Azure AI Document Intelligence prebuilt layout model for structure, then Azure AI Vision for image analysis as a separate step
B) Azure AI Search enrichment pipeline with OCR skill, image analysis skill, and entity recognition skill chained together
C) Azure Content Understanding with the multimodal document analyzer, which processes text, images, layout, tables, and entities in a unified model
D) Azure AI Vision Florence model for image understanding, then Azure AI Document Intelligence for text and layout

<details>
<summary>Answer</summary>

**Correct: C**

Azure Content Understanding multimodal document analyzer is specifically designed for documents with mixed text and visual content. It uses a unified model to understand relationships between text, images, charts, and layout — extracting entities that span modalities (e.g., a chart with a text label). Single API call processes everything. This is the recommended service for multimodal document scenarios where visual elements carry semantic meaning.

**Why the others are wrong:**
- Using Document Intelligence and AI Vision separately requires manual correlation of text and image results — you lose the semantic relationships between modalities. For example, a chart labeled 'Q3 Revenue' requires understanding text + image together. Separate processing can't capture this.
- Azure AI Search enrichment is for building search indexes, not for direct extraction. Chaining skills adds latency. The OCR and image analysis skills process modalities separately — they don't understand multimodal relationships. Overkill for an extraction-only scenario.
- Azure AI Vision Florence is for general image understanding (captions, objects), not document-specific analysis like tables and forms. Document Intelligence doesn't process images semantically — just layout and text. This combination misses the integrated multimodal analysis that Content Understanding provides.

</details>

---

**Q7.** A RAG system indexes 20,000 product manuals in Azure AI Search. The index has vector fields (embeddings) and text fields (title, content). Users query using natural language. Retrieval latency is 800ms per query, exceeding the 200ms SLA. Vector search uses cosine similarity with k=50. Which optimization would MOST reduce latency while maintaining quality?

A) Enable HNSW (Hierarchical Navigable Small World) algorithm for vector search instead of exhaustive search, and reduce k from 50 to 10
B) Partition the index into multiple smaller indexes by product category and route queries to the relevant partition
C) Switch from cosine similarity to dot product similarity for faster computation
D) Cache the top 1000 most common queries and their results in Azure Redis Cache

<details>
<summary>Answer</summary>

**Correct: A**

HNSW is an approximate nearest neighbor (ANN) algorithm that dramatically speeds up vector search — it provides 95-99% recall with 10-100x faster query times than exhaustive search. Azure AI Search uses HNSW by default for vector fields, but you must configure it properly. Reducing k from 50 to 10 reduces computation — for RAG, top 10 results are usually sufficient (only top 3-5 are passed to the LLM anyway). This is the recommended optimization for production vector search.

**Why the others are wrong:**
- Partitioning adds routing complexity and overhead. If a query spans categories or the category is unknown, you must query multiple partitions, increasing latency. For 20,000 documents, partitioning is overkill — a single index with HNSW handles millions of vectors efficiently.
- Cosine similarity and dot product are mathematically equivalent for normalized vectors (which embeddings are). Switching doesn't improve speed — the bottleneck is the search algorithm (exhaustive vs HNSW), not the similarity metric.
- Caching helps for repeated queries but doesn't address the core latency problem for novel queries. With natural language queries, cache hit rate is low — users phrase questions differently. This adds complexity (cache invalidation, storage) without solving the 800ms latency issue.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** A healthcare application uses Azure AI Document Intelligence to extract data from patient intake forms. The forms have checkboxes, handwritten signatures, and typed fields. After extraction, the application must verify that required fields (patient name, date of birth, insurance ID) are present before processing. Which approach ensures the MOST reliable validation?

A) Use Document Intelligence prebuilt layout model, extract all text via OCR, then parse the text with regex to find required fields
B) Train a custom Document Intelligence model on sample intake forms, extract fields by name, and validate that required fields have non-null values
C) Use Document Intelligence prebuilt general document model to extract key-value pairs, then check for required keys in the response and validate field confidence scores are above 0.85
D) Use Azure AI Search enrichment pipeline with OCR skill and entity recognition skill, then validate entities match required field types

<details>
<summary>Answer</summary>

**Correct: B**

Custom models in Document Intelligence are trained on your specific form layouts — they learn field locations, labels, and types. Extraction by field name (e.g., fields['PatientName']) is reliable and structured. Validation checks for non-null values on required fields. Custom models provide the highest accuracy for fixed-format forms like intake forms. This is the recommended approach for production form processing in healthcare where accuracy is critical.

**Why the others are wrong:**
- Layout model returns unstructured text blocks — you must manually map text to fields using regex, which is brittle and unreliable for varying layouts. Handwritten text may not OCR perfectly, and regex can't reliably distinguish 'Patient Name' from 'Emergency Contact Name' without positional context.
- General document model extracts generic key-value pairs but isn't optimized for specific form layouts. For intake forms with fixed fields, a custom model trained on your forms provides much higher accuracy. Confidence scores help but don't replace domain-specific training.
- Azure AI Search is for building search indexes, not form validation. Entity recognition is for generic entities (person, date) but doesn't understand form-specific semantics like 'Insurance ID' vs 'Medical Record Number'. This is the wrong tool for structured form extraction.

</details>

---

