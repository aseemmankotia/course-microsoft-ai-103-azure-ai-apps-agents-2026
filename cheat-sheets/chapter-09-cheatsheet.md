# Retrieval and Information Extraction Pipelines Cheat Sheet

## RAG Ingestion Flow

| Step | Action | Key Considerations |
|------|--------|-------------------|
| 1. Document Loading | Load PDFs, DOCX, HTML, images | Handle multimodal content, preserve metadata |
| 2. Chunking | Split into 512-1024 token segments | Use semantic chunking (paragraphs/sections), 10-20% overlap |
| 3. Embedding Generation | Generate vectors (1536-dim with text-embedding-3-small) | Batch API calls, handle rate limits |
| 4. Index Population | Upload to Azure AI Search with vector + text fields | Configure HNSW algorithm, set filterable fields |

## Retrieval Strategies

| Strategy | Use When | Pros | Cons |
|----------|----------|------|------|
| **Vector Search** (cosine similarity) | Pure semantic queries, exploratory search | Handles synonyms, paraphrases | Misses exact matches |
| **Hybrid Search** (vector + keyword) | Production RAG, mixed query types | Best of both worlds | Slightly slower than pure vector |
| **Semantic Ranking** | Highest quality needs, user-facing apps | Significantly improves relevance | Adds 50-200ms latency |

**Exam Tip**: Hybrid + semantic ranking = recommended production pattern.

## Azure AI Search Index Configuration

```json
{
  "fields": [
    {"name": "id", "type": "Edm.String", "key": true},
    {"name": "content", "type": "Edm.String", "searchable": true},
    {"name": "vector", "type": "Collection(Edm.Single)", "dimensions": 1536, "vectorSearchProfile": "hnsw-config"},
    {"name": "category", "type": "Edm.String", "filterable": true},
    {"name": "timestamp", "type": "Edm.DateTimeOffset", "filterable": true}
  ]
}
```

**Exam Tip**: Vector fields require `dimensions` and `vectorSearchProfile`. Text fields need `searchable: true` for keyword search.

## Built-in Enrichment Skills

| Skill | Input | Output | Use Case |
|-------|-------|--------|----------|
| **OCR** | Images, PDFs | Extracted text, reading order | Scanned documents |
| **Entity Recognition** | Text | People, places, organizations | Metadata extraction |
| **Key Phrase Extraction** | Text | Important phrases | Summarization, tagging |
| **Language Detection** | Text | Language code (en, es, etc.) | Multi-language corpora |
| **PII Detection** | Text | Detected PII with redaction | Compliance scenarios |

**Exam Tip**: Built-in skills run in-process (fast). Custom skills use Azure Functions (slower, adds HTTP latency).

## Chunking Strategies

| Strategy | Description | When to Use |
|----------|-------------|-------------|
| **Fixed Token** | Split every N tokens | Simple, predictable chunk sizes |
| **Semantic** | Split on paragraphs, sections | Preserve context, better retrieval quality |
| **Sliding Window** | Overlapping fixed chunks | Ensure no context lost at boundaries |
| **Recursive** | Split on delimiters (\n\n, \n, space) | Balance size and semantics |

**Production Recommendation**: Semantic chunking with 10-20% overlap + parent metadata.

## Document Extraction Services

| Service | Best For | Key Features |
|---------|----------|-------------|
| **Azure AI Document Intelligence** | Forms, invoices, contracts | Prebuilt models (invoice, receipt, contract, ID), custom training, 95%+ accuracy |
| **Azure Content Understanding** | Multimodal documents (text + images) | Unified model for layout + entities + tables, understands image semantics |
| **Azure AI Vision OCR** | Simple text extraction | Fast, low-cost, no layout analysis |

**Exam Tip**: Document Intelligence for structured forms. Content Understanding for research papers with meaningful charts/diagrams.

## Cost Optimization

1. **Skillset Caching**: Enable in indexer config — saves 50-90% on re-indexing
2. **OCR DPI**: Use 150 DPI (vs 300) for printed docs — 50% faster
3. **Batch Sizing**: Increase to 100-1000 docs per batch — reduces overhead
4. **Remove Redundant Skills**: Skip language detection if single language
5. **HNSW over Exhaustive**: 10-100x faster vector search with minimal quality loss

## Vector Search Limits

- **Max dimensions**: 3072 (Azure AI Search)
- **Recommended dimensions**: 1536 (text-embedding-3-small), 3072 (text-embedding-3-large)
- **Max k value**: 1000 (tier-dependent)
- **Production k**: 10-20 for RAG (only top 3-5 used by LLM)
- **HNSW recall**: 95-99% at 10-100x speed vs exhaustive

## Common Exam Traps

1. **Chunking**: Fixed token splits break context → use semantic chunking
2. **Vector-only search**: Misses exact matches → use hybrid search
3. **High k values**: k=50 wastes compute → reduce to 10 for RAG
4. **No caching**: Re-runs expensive skills → enable skillset caching
5. **Wrong service**: Document Intelligence for multimodal → use Content Understanding instead
