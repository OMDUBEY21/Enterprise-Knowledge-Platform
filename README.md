# Enterprise Knowledge Platform
An AI-powered enterprise knowledge platform that organizes company documents into a searchable knowledge base, enabling users to explore information and ask questions with contextual, cited answers from internal and trusted external sources.

 # Application Screenshots
 ### 1. Knowledge Platform — Home Dashboard
 <img width="1518" height="971" alt="01" src="https://github.com/user-attachments/assets/b2063eb7-80f6-4923-a687-619b443c7239" />

 ### 2. Document Upload & Ingestion
 <img width="1404" height="927" alt="02" src="https://github.com/user-attachments/assets/8118aa30-7c87-48e4-a549-6cedad35d875" />

 ### 3. Document Metadata Configuration
 <img width="1189" height="909" alt="03" src="https://github.com/user-attachments/assets/317a3554-5114-4ea5-accb-740e1b13fe6c" />

 ### 4. Real-Time Document Processing Pipeline
 <img width="1472" height="975" alt="04" src="https://github.com/user-attachments/assets/481705ec-7c88-4670-96ae-f7fa69ee2c6a" />



 
-----

 # Overview
The **Enterprise Knowledge Platform** is an end-to-end Retrieval-Augmented Generation (RAG) application designed to make enterprise documents easier to ingest, search, understand, and query.

Users can upload company documents with business metadata such as company, department, country, document type, author, version, and confidentiality. The platform processes those documents into structured, semantically meaningful chunks, generates embeddings, stores them in a vector database, and exposes both pure retrieval and LLM-powered question answering.

# The system combines:

- **Dense Semantic Retrieval** – Retrieves documents based on semantic similarity using vector embeddings.
- **BM25 Lexical Retrieval** – Performs keyword-based retrieval to capture exact terminology and domain-specific matches.
- **Reciprocal Rank Fusion (RRF)** – Combines semantic and lexical retrieval results into a unified ranking.
- **Cross-Encoder Reranking** – Re-ranks retrieved candidates using a cross-encoder for improved relevance.
- **Metadata-Aware Retrieval** – Uses document metadata and business context to improve retrieval precision.
- **Rule-Based & LLM-Based Query Understanding** – Interprets user queries using deterministic rules and LLM reasoning.
- **Confidence Scoring & Filter Relaxation** – Dynamically evaluates retrieval confidence and relaxes filters when necessary.
- **Citation-Grounded Answer Generation** – Generates answers grounded in retrieved sources with supporting citations.
- **Company-Scoped Web Search Fallback** – Falls back to company-restricted web search when internal knowledge is insufficient.
- **Ingestion & Retrieval Metrics** – Tracks document ingestion and retrieval performance for system evaluation and optimization.
- **Retrieval Debugging & Observability** – Detailed visibility into retrieval, ranking, filtering, scoring, and query-processing behavior.
- **Knowledge Workspace & Evaluation Dashboard** – Browser-based interface for knowledge management, querying, evaluation, and performance monitoring.

# System Flow

The platform follows a sequential knowledge-processing and question-answering workflow.

                    ┌─────────────────────────┐
                    │     Enterprise Docs     │
                    │ PDF / DOCX / TXT / MD   │
                    │ HTML / HTM              │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │   Document Ingestion    │
                    │ Parse / Clean / Metadata│
                    │ Normalize / Enrich      │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │    Chunking Pipeline    │
                    │ Parent / Child Chunks   │
                    │ Semantic Processing     │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │   Embedding Generation  │
                    │       BAAI/bge-m3       │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │      Knowledge Base     │
                    │   Qdrant + SQLite       │
                    └────────────┬────────────┘
                                 │
                                 │
                    ┌────────────▼────────────┐
                    │      User Question      │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │   Query Understanding   │
                    │ Rules + LLM Processing  │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │    Hybrid Retrieval     │
                    │ Dense + BM25 Retrieval  │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │    RRF Rank Fusion      │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │ Cross-Encoder Reranking │
                    └────────────┬────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │Confidence Classification│
                    └────────────┬────────────┘
                                 │
                  ┌──────────────┴──────────────┐
                  │                             │
                  ▼                             ▼
           High / Medium                  Low / No Results
                  │                             │
                  ▼                             ▼
        ┌─────────────────┐          ┌─────────────────────┐
        │ LLM Generation  │          │ Company-Scoped Web  │
        │ + Citations     │          │ Search Fallback     │
        └────────┬────────┘          └──────────┬──────────┘
                 │                              │
                 └──────────────┬───────────────┘
                                ▼
                     ┌───────────────────────┐
                     │   Final AI Response   │
                     │ Grounded + Cited      │
                     └───────────────────────┘

# Key Features

### 1. 📄 Enterprise Document Ingestion

Supports ingestion of multiple enterprise document formats:

- **PDF**
- **DOCX**
- **TXT**
- **Markdown**
- **HTML / HTM**

The ingestion pipeline processes documents through a structured workflow:

**Parse → Duplicate Check → Clean → Metadata → Normalize → Content Metadata → Chunk → Embed → Store**

It includes:

- **Multi-format document parsing** for common enterprise knowledge sources.
- **Content-hash duplicate detection** to prevent redundant document ingestion.
- **Filename + company duplicate detection** for additional duplicate protection.
- **Company-name normalization** for consistent company identification.
- **Conservative department, document-type, and country normalization** to avoid incorrect category merging.
- **Boilerplate, watermark, and repeated-line removal** for cleaner knowledge extraction.
- **OCR-needed detection** for low-text PDFs.
- **LLM-generated summaries, topics, keywords, and entities** for content enrichment.
- **Content metadata extraction** to improve document discoverability and retrieval.
- **Semantic chunking** to create retrieval-optimized knowledge units.
- **Embedding generation** for dense vector retrieval.
- **Persistent vector and document storage** for downstream RAG and hybrid retrieval.
- **Ingestion-stage performance metrics** for pipeline monitoring and optimization.

 ### 2. 🧩 Parent-Child Semantic Chunking

Documents are converted into a parent-child chunk structure to balance retrieval precision with contextual completeness.

The chunking pipeline uses:

1. Sentence-aware recursive splitting
2. Configurable token-based chunk sizing
3. Sentence-granular overlap
4. Embedding-based semantic merging
5. Parent-child relationships for contextual retrieval

Default configuration:

| Parameter | Default |
|---|---:|
| Target chunk size | 700 tokens |
| Chunk overlap | 120 tokens |
| Semantic merge threshold | 0.82 |
| Parent soft cap | 6000 characters |

This approach allows retrieval to match smaller child chunks while retaining the broader parent section for answer generation.

### 3. 🌐 Company-Scoped Web Search Fallback

When internal enterprise knowledge is insufficient, the platform can augment retrieval using external web sources:

- **Company-Aware Search Planning** – Generates search queries and identifies likely company domains.
- **Company-Scoped Web Search** – Restricts external search to relevant company context.
- **Raw Content Enrichment** – Enriches selected web results with additional content.
- **LLM-Based Result Filtering** – Selects results based on relevance and trustworthiness.
- **Internal + Web Evidence** – Combines enterprise knowledge with validated external evidence.
- **Web-Augmented Answer Generation** – Produces answers using both internal and external sources when required.

### 4. 📊 Evaluation, Metrics & Observability

The platform includes a browser-based evaluation and monitoring experience:

- 🔹 **Evaluation Metrics Dashboard** – Provides visibility into ingestion and retrieval performance.
- 🔹 **Pipeline KPIs** – Tracks key ingestion and retrieval indicators.
- 🔹 **Ingestion Stage Performance** – Measures individual ingestion pipeline stages.
- 🔹 **Retrieval Strategy Comparison** – Enables analysis of different retrieval approaches.
- 🔹 **Retrieval Funnel & Chunk Analysis** – Provides visibility into candidate selection and chunk-level behavior.
- 🔹 **RAG Scoring** – Exposes retrieval and answer-quality related metrics.
- 🔹 **Latency Timeline** – Tracks processing and retrieval latency.
- 🔹 **Failure Insights** – Helps identify pipeline and retrieval failures.
- 🔹 **Historical Runs** – Maintains historical ingestion and retrieval metrics.
- 🔹 **Detailed Retrieval Debugging** – Exposes per-request retrieval traces including query understanding, company resolution, metadata filters, hybrid scoring, reranking, confidence classification, and retry events.

# Ingestion Pipeline

The **Ingestion Pipeline** transforms raw enterprise documents into structured, metadata-rich, semantically searchable knowledge that can be consumed by the retrieval layer.

## Architecture / Flow

```text
┌──────────────────────────────────────────────────────────────┐
│                    ENTERPRISE DOCUMENTS                      │
│            PDF / DOCX / TXT / Markdown / HTML                │
└──────────────────────────────┬───────────────────────────────┘
                               │
                               ▼
┌──────────────────────────────────────────────────────────────┐
│                     DOCUMENT PARSING                         │
│                                                              │
│              Extract Text + Sections                         │
└──────────────────────────────┬───────────────────────────────┘
                               │
                               ▼
┌──────────────────────────────────────────────────────────────┐
│                    DUPLICATE DETECTION                       │
│                                                              │
│        Filename + Company Matching + SHA-256 Hash            │
└──────────────────────────────┬───────────────────────────────┘
                               │
                               ▼
┌──────────────────────────────────────────────────────────────┐
│                       TEXT CLEANING                          │
│                                                              │
│     Boilerplate / Watermarks / Repeated Lines / Noise        │
└──────────────────────────────┬───────────────────────────────┘
                               │
                               ▼
┌──────────────────────────────────────────────────────────────┐
│                    BUSINESS METADATA                         │
│                                                              │
│  Company / Department / Country / Industry / Type /          │
│                    Author / Version                          │
└──────────────────────────────┬───────────────────────────────┘
                               │
                               ▼
┌──────────────────────────────────────────────────────────────┐
│                  METADATA NORMALIZATION                      │
│                                                              │
│       Company Fuzzy Matching + Canonical Business Fields     │
└──────────────────────────────┬───────────────────────────────┘
                               │
                               ▼
┌──────────────────────────────────────────────────────────────┐
│                    LLM CONTENT ENRICHMENT                    │
│                                                              │
│       Groq → Summary / Topics / Keywords / Entities          │
└──────────────────────────────┬───────────────────────────────┘
                               │
                               ▼
┌──────────────────────────────────────────────────────────────┐
│                     SEMANTIC CHUNKING                        │
│                                                              │
│   Recursive Split + Semantic Merge + Parent/Child Chunks     │
└──────────────────────────────┬───────────────────────────────┘
                               │
                               ▼
┌──────────────────────────────────────────────────────────────┐
│                   EMBEDDING GENERATION                       │
│                                                              │
│              BAAI/bge-m3 → 1024-D Vectors                    │
└──────────────────────────────┬───────────────────────────────┘
                               │
                               ▼
┌──────────────────────────────────────────────────────────────┐
│                     KNOWLEDGE STORAGE                        │
│                                                              │
│        Qdrant → Vectors + Chunks + Metadata                  │
│        SQLite → Documents + Metrics + Metadata               │
└──────────────────────────────┬───────────────────────────────┘
                               │
                               ▼
┌──────────────────────────────────────────────────────────────┐
│                     RETRIEVAL-READY KB                       │
│                                                              │
│                 Dense + BM25 Search                          │
└──────────────────────────────────────────────────────────────┘
```

## Pipeline Stages

### 1. Document Upload & Parsing

The platform accepts **PDF, DOCX, TXT, Markdown, and HTML/HTM** documents. Format-specific parsers convert uploaded files into structured sections containing extracted text and available page information.

The parser also flags low-text PDFs that may require OCR. The current implementation detects this condition and perform a separate OCR workflow.

### 2. Duplicate Detection

Duplicate validation occurs before expensive downstream processing.

The implementation uses:

- **Filename + company validation**
- **SHA-256 content hashing**

The content hash is calculated from normalized document text so superficial whitespace differences do not create a new logical document.

```text
Document → Extracted Text → Normalize → SHA-256
                                      ↓
                              Existing Document?
```

### 3. Text Cleaning

Parsed sections are cleaned before metadata enrichment and chunking.

The cleaner removes document noise such as:

- Boilerplate
- Watermarks
- Repeated lines
- Page-number artifacts
- Extraction noise

### 4. Business Metadata Extraction

Documents are associated with structured enterprise metadata:

| Metadata | Purpose |
|---|---|
| Company | Enterprise ownership/context |
| Department | Organizational context |
| Industry | Business domain |
| Country | Geographic context |
| Document Type | Document classification |
| Author | Document ownership |
| Version | Version tracking |
| Confidentiality | Classification context |

Company resolution is particularly important because company context is also used by the retrieval layer.

### 6. LLM-Based Content Enrichment

The pipeline performs an ingestion-time **Groq LLM** call to derive content-level metadata:

- **Summary**
- **Topics**
- **Keywords**
- **Entities**
- **Section headings**

```text
Document Content
      │
      ▼
   Groq LLM
      │
      ▼
Structured Content Metadata
```

This enrichment is available to downstream retrieval, particularly lexical retrieval and result context.

### 7. Parent-Child Semantic Chunking

Chunking converts long document sections into retrieval-oriented units.

The implementation combines:

1. Sentence-aware recursive splitting
2. Token-based chunk sizing
3. Sentence-granular overlap
4. Embedding-based semantic merging
5. Parent-child chunk relationships

Default configuration:

| Parameter | Default |
|---|---:|
| Target chunk size | 700 tokens |
| Chunk overlap | 120 tokens |
| Semantic merge threshold | 0.82 |
| Parent soft cap | 6000 characters |

Adjacent pieces with cosine similarity of at least **0.82** can be merged, subject to the configured size ceiling.

Each section produces a parent chunk and child chunks:

```text
Parent Section
      │
      ├── Child Chunk 1
      ├── Child Chunk 2
      └── Child Chunk N
```

Children provide precise retrieval units, while parents preserve broader context for later answer generation.

### 8. Dense Embedding Generation

Every generated chunk is converted into a dense vector using:

```text
BAAI/bge-m3
```

Configured embedding dimension:

```text
1024
```

Embeddings are generated in batches.

```text
Chunk Text → BAAI/bge-m3 → 1024-D Dense Vector
```

The same embedding model/path is used for query embeddings during retrieval.

> **Implementation note:** `bge-m3` supports additional sparse and ColBERT-style representations, but this application uses its **dense embedding output**. Lexical retrieval is implemented separately using BM25.

### 9. Vectorization & Storage

Generated chunks and embeddings are persisted into **Qdrant**.

Default collection:

```text
ecip_chunks
```

Each Qdrant point contains the dense vector, chunk text, business metadata, content metadata, and parent-child information.

Document-level information and operational metrics are stored in **SQLite**.

```text
                 Knowledge Base
                       │
             ┌─────────┴─────────┐
             ▼                   ▼
          Qdrant              SQLite
             │                   │
     Vectors + Chunks      Documents + Metrics
     Metadata Payloads
```

### 10. Ingestion Metrics & Observability

The server records actual stage timings for:

- Parse
- Clean
- Chunk
- Embed
- Store
- Total ingestion

Additional information includes total sections, total chunks, parent/child counts, OCR-needed status, warnings, and completion status.

Metrics are persisted in SQLite and surfaced through the Evaluation Metrics experience.

## Ingestion Data Lifecycle

```text
Raw File
   │
   ▼
Parsed Sections
   │
   ▼
Cleaned Sections
   │
   ▼
Business + Content Metadata
   │
   ▼
Parent / Child Chunks
   │
   ▼
Dense Embeddings
   │
   ├───────────────┐
   ▼               ▼
 Qdrant          SQLite
   │               │
Vectors +       Documents +
Chunks +        Metadata +
Payloads        Metrics
   │
   └───────► Retrieval Layer
```
---

# Retrieval Pipeline

The **Retrieval Pipeline** converts a natural-language user query into a ranked, reranked, and contextually expanded set of relevant knowledge sections.

The platform exposes two retrieval surfaces:

```text
POST /search
    │
    └── Pure retrieval

POST /ask
    │
    └── LLM-assisted RAG
        Query understanding
        → Retrieval
        → Answer generation
```

Both use the same underlying retrieval engine:

## Architecture / Flow

```text
                         User Query
                             │
                             ▼
                 ┌─────────────────────┐
                 │  Query Understanding │
                 │                     │
                 │ Entity Inference    │
                 │ Metadata Inference  │
                 │ Query Expansion     │
                 │ LLM Understanding   │
                 │      (/ask)         │
                 └──────────┬──────────┘
                            │
                            ▼
                     Query Variants
                            │
                  ┌─────────┴─────────┐
                  │                   │
                  ▼                   ▼
           Dense Retrieval       BM25 Retrieval
              Qdrant             rank_bm25
                  │                   │
                  └─────────┬─────────┘
                            ▼
                    RRF Fusion (k=60)
                            │
                            ▼
                    Top 30 Candidates
                            │
                            ▼
                 Cross-Encoder Reranking
                 BAAI/bge-reranker-base
                            │
                            ▼
                  Confidence Evaluation
                            │
                 ┌──────────┴──────────┐
                 │                     │
                 ▼                     ▼
            Confident             Low / Empty
                 │                     │
                 │               Filter Relaxation
                 │                     │
                 │                    Retry
                 │                     │
                 └──────────┬──────────┘
                            ▼
                  Parent-Level Deduplication
                            │
                            ▼
                       Final Top-K
                            │
                            ▼
                    Parent Context
                            │
                            ▼
                    LLM-Ready Context
```

## Retrieval Stages

### 1. Query Intake

Users submit natural-language questions through  `/ask`.

Example:

```text
"What is the annual leave policy for employees?"
```

`/search` performs retrieval directly, while `/ask` additionally performs LLM-based query understanding before calling the same retrieval engine.

### 2. Query Understanding

The retrieval layer can identify useful enterprise context including:

- Company
- Department
- Document type
- Country
- Topics

For `/search`, processing is primarily deterministic.

For `/ask`, the LLM can additionally provide:

- Rewritten query
- Company hint
- Department hint
- Expanded queries
- Query intent

LLM-derived values are treated as **hints**, not unquestioned authoritative filters.

The general precedence is:

```text
Explicit Input
      >
Deterministic Inference
      >
LLM Hint
      >
No Filter
```

### 3. Deterministic Entity Inference

Known metadata values from the knowledge base are used to infer entities from free-text queries.

Example:

```text
Query:
"What is the leave policy at Saint Gobain?"

Resolved Company:
Saint Gobain
```

The shared matching primitive uses whole-word/phrase boundaries and prefers longer matching values.

### 4. Query Expansion

Two mechanisms are implemented.

#### Deterministic Expansion

Available for every retrieval request.

Example:

```text
"What is the CEO's name?"
```

can produce:

```text
"What is the CEO's name?"
"CEO's name"
```

The original query is always retained.

#### LLM-Based Expansion

Used by `/ask` to generate semantically related variants.

Example:

```text
"leave balance"
```

may produce variants such as:

```text
annual leave
vacation policy
PTO
leave entitlement
```

Variants are deduplicated before retrieval.

### 5. Query Embedding

Each query variant entering dense retrieval is embedded using:

```text
BAAI/bge-m3
```

Result:

```text
1024-dimensional query vector
```

```text
Query Variant → BAAI/bge-m3 → 1024-D Vector
```

Using the same embedding model for documents and queries places both representations in the same vector space.

### 6. Metadata Filtering

Dense Qdrant retrieval can use enterprise metadata to narrow the search space.

Supported filters include:

- Company
- Department
- Document type
- Topics

This combines semantic similarity with enterprise-specific context.

### 7. Dense Semantic Retrieval

The first retrieval branch performs **cosine-similarity vector search** against Qdrant.

```text
Query
  │
  ▼
Query Embedding
  │
  ▼
Qdrant Dense Search
  │
  ▼
Dense Candidates
```

Default dense candidate count:

```text
20
```

Dense retrieval helps when query terminology differs from source-document wording.

### 8. BM25 Lexical Retrieval

The second branch performs lexical search using:

```text
rank_bm25.BM25Okapi
```

The implementation maintains an in-process, per-company BM25 index.

The indexed representation uses child-chunk text together with extracted keywords and topics.

This provides exact lexical matching for terminology, acronyms, policy names, and domain-specific phrases.

> **Implementation note:** BM25 is implemented separately from Qdrant. It is not Qdrant's native sparse-vector retrieval in the current architecture.

### 9. Hybrid Search

For each query variant, the engine executes:

```text
Dense Search + BM25 Search → RRF Fusion
```

This creates the platform's **hybrid retrieval** strategy.

```text
                 Query Variant
                      │
          ┌───────────┴───────────┐
          ▼                       ▼
    Dense Retrieval         BM25 Retrieval
       Qdrant                rank_bm25
          │                       │
          └───────────┬───────────┘
                      ▼
                 RRF Fusion
```

The combination provides:

- **Semantic recall** from dense retrieval
- **Exact lexical matching** from BM25

### 10. Reciprocal Rank Fusion

Dense and BM25 result lists are combined using **Reciprocal Rank Fusion (RRF)**.

The implementation uses:

```text
RRF contribution = 1 / (k + rank)
k = 60
```

A chunk appearing in both result lists receives contributions from both rankings.

### 11. Candidate Selection

After fusion, candidates are sorted by accumulated RRF score.

The top:

```text
30 candidates
```

are passed to the reranker.

This creates a two-stage retrieval architecture:

```text
Stage 1: Recall
Dense + BM25
      ↓
RRF Fusion
      ↓
Top 30

Stage 2: Precision
Cross-Encoder
      ↓
Final Ranking
```

### 12. Cross-Encoder Reranking

The top 30 candidates are reranked using:

```text
BAAI/bge-reranker-base
```

The cross-encoder evaluates each:

```text
(Query, Candidate Chunk)
```

pair directly.

```text
Query + Candidate Chunk
          │
          ▼
   Cross-Encoder
          │
          ▼
   Relevance Score
```

The original query is used for reranking. Expanded variants influence candidate discovery but are not used as the reranking query.

### 13. Confidence Classification

The top reranked score is used for retrieval confidence.

| Reranked Score | Confidence |
|---:|---|
| `>= 0.80` | High |
| `>= 0.60` and `< 0.80` | Medium |
| `< 0.60` | Low |

```text
Top Reranked Score
        │
        ▼
Confidence Classifier
        │
   ┌────┼────┐
   ▼    ▼    ▼
 High Medium Low
```

### 14. Filter Relaxation & Retrieval Retry

If retrieval returns no useful evidence or produces low confidence, the engine can retry with relaxed inferred filters.

```text
Initial Retrieval
      │
      ▼
Low Confidence / No Results
      │
      ▼
Relax Inferred Filters
      │
      ▼
Retry Retrieval
      │
      ▼
RRF + Reranking
      │
      ▼
Final Confidence
```

Explicit user constraints remain trusted; inferred/LLM-derived constraints can be relaxed.

### 15. Evidence-Based Company Inference

For `/ask`, the system can infer a company from retrieved evidence when no company has already been resolved.

The top five reranked results are examined for company agreement.

Current threshold:

```text
60% agreement
```

A company-scoped retry is only accepted if it performs at least as well as the original unscoped retrieval.

### 16. Parent-Child Dereferencing

After reranking, the system performs parent-level deduplication.

If several children from the same parent section are retrieved:

```text
Parent A
 ├── Child A1
 ├── Child A2
 └── Child A3

Parent B
 ├── Child B1
 └── Child B2
```

only the highest-scoring child for each parent section is retained.

This reduces redundant context.

### 17. Top-K Context Selection

After parent-level deduplication, results are truncated to the requested `top_k`.

Default:

```text
top_k = 5
```

The final result set therefore represents the most relevant unique sections.

### 18. Context Preparation

Each retrieval result retains:

- The **matched child text** that produced the retrieval match
- The **parent section text** providing broader context

```text
Query
  │
  ▼
Child Match
  │
  ▼
Parent Section
  │
  ▼
Contextual Retrieval Result
```

This balances retrieval precision with contextual completeness.

### 19. LLM-Ready Context

For `/ask`, final results are prepared as source-aware context blocks.

Context can include:

- Document name
- Heading
- Page number
- Matched snippet
- Parent text
- Retrieval score
- Source type
- Summary
- Topics
- Entities

Conceptually:

```text
[1] INTERNAL — Document A — Section X
    Parent section context...

[2] INTERNAL — Document B — Section Y
    Parent section context...

[3] INTERNAL — Document C — Section Z
    Parent section context...
```

## Retrieval Data Lifecycle

```text
User Query
    │
    ▼
Query Understanding
    │
    ├── Entity Inference
    ├── Metadata Context
    └── Query Expansion
    │
    ▼
Query Variants
    │
    ├─────────────────────┐
    ▼                     ▼
Query Embeddings       BM25 Queries
    │                     │
    ▼                     ▼
Qdrant Dense Search    BM25 Search
    │                     │
    └──────────┬──────────┘
               ▼
           RRF Fusion
               │
               ▼
        Top 30 Candidates
               │
               ▼
      Cross-Encoder Reranking
               │
               ▼
       Confidence Evaluation
               │
          ┌────┴────┐
          │         │
          ▼         ▼
      Confident   Retry
                    │
             Relax Inferred
                Filters
                    │
             Re-run Retrieval
                    │
          └────┬────┘
               ▼
       Parent Deduplication
               │
               ▼
             Top-K
               │
               ▼
      Parent Context Retrieval
               │
               ▼
        LLM-Ready Context
```

---

# Answer Generation

After retrieval, the platform can use selected evidence to generate a grounded response.

The generation layer supports:

- Direct answer generation from internal knowledge
- Web-augmented answer generation
- Whole-document summarization

## Confidence-Gated Generation

```text
Retrieved Results
      │
      ▼
Confidence Gate
      │
 ┌────┴────┐
 ▼         ▼
High/Med   Low
 │         │
 ▼         ▼
LLM      Evidence
Answer   Caveat
```

When retrieval confidence is **low**, normal answer-generation LLM synthesis is skipped and the system returns a controlled evidence caveat instead.

## Citation-Grounded Generation

Citations are derived from actual retrieved results rather than being produced by an independent citation-generation step.

Source information can include:

- Document name
- Heading
- Page number
- Chunk information
- Source type

This structurally connects generated answers to retrieved evidence.

---

# Company-Scoped Web Search Fallback

When internal knowledge is insufficient, the `/ask` workflow can use a company-scoped external web-search fallback.

The workflow is bounded rather than an open-ended agent loop:

```text
Low / Insufficient Internal Confidence
                 │
                 ▼
          Company Available?
                 │
                 ▼
         Search Planning
                 │
                 ▼
           Tavily Search
                 │
                 ▼
        Raw Content Enrichment
                 │
                 ▼
        LLM Result Filtering
                 │
                 ▼
       External Evidence Set
                 │
                 ▼
      Web-Augmented Generation
```

The workflow can:

- Generate a search query
- Identify likely company domains
- Execute company-aware search
- Enrich selected results with raw content
- Filter results for relevance/trustworthiness
- Combine internal and external evidence

---

# LLM Architecture

The platform uses **Groq** for LLM operations.

Default model:

```text
llama-3.3-70b-versatile
```

LLM usage is divided into structured and natural-language tasks.

```text
Groq
 │
 ├── Structured Tasks
 │     ├── Query Understanding
 │     ├── Content Metadata Extraction
 │     ├── Search Planning
 │     └── Web Result Filtering
 │
 └── Generation Tasks
       ├── Answer Generation
       └── Document Summarization
```

The application uses bounded LLM interactions rather than maintaining a long conversational message history inside the backend pipeline.

---

# Storage Architecture

## SQLite

SQLite stores structured application information such as:

- Document records
- Ingestion metrics
- Retrieval metrics

Default database:

```text
data/ecip.db
```

## Qdrant

Qdrant stores the searchable vector representation.

Default collection:

```text
ecip_chunks
```

Qdrant points contain:

- Dense embeddings
- Chunk text
- Business metadata
- Content metadata
- Parent-child relationships

The current design uses one Qdrant collection and stores company context as payload metadata.

---

# Evaluation, Metrics & Observability

The platform includes a browser-based Evaluation Metrics experience for inspecting ingestion and retrieval behavior.

It provides visibility into:

- Pipeline KPIs
- Ingestion stage performance
- Retrieval strategy behavior
- Retrieval funnel
- Chunk-level analysis
- RAG-related scoring
- Latency
- Failure insights
- Historical runs
- Retrieval debugging

## Ingestion Observability

Server-side timing is recorded for:

```text
Parse → Clean → Chunk → Embed → Store → Total
```

## Retrieval Observability

Detailed retrieval diagnostics can expose:

```text
Query Understanding
        ↓
Entity / Company Resolution
        ↓
Metadata Filters
        ↓
Query Variants
        ↓
Dense Retrieval
        ↓
BM25 Retrieval
        ↓
RRF Fusion
        ↓
Reranking
        ↓
Confidence
        ↓
Retry / Relaxation
        ↓
Final Results
```

---

# API Overview

## Document APIs

| Method | Endpoint | Purpose |
|---|---|---|
| `POST` | `/documents/check-company` | Check possible company-name matches |
| `POST` | `/documents/upload` | Upload and ingest a document |
| `GET` | `/documents/count` | Return current chunk count |
| `GET` | `/documents` | List ingested documents |
| `DELETE` | `/documents/{document_id}` | Delete a document |
| `DELETE` | `/documents` | Reset the knowledge base |

## Retrieval APIs

| Method | Endpoint | Purpose |
|---|---|---|
| `POST` | `/search` | Execute pure hybrid retrieval |
| `POST` | `/ask` | Execute LLM-powered RAG question answering |

## System & Evaluation APIs

| Method | Endpoint | Purpose |
|---|---|---|
| `GET` | `/health` | Basic health response |
| `GET` | `/api/info` | Application information |
| `GET` | `/api/system-status` | System status/configuration |
| `GET` | `/api/metrics/dashboard` | Evaluation dashboard data |
| `GET` | `/api/metrics/documents` | Document-level metrics |
| `GET` | `/api/metrics/historical-runs` | Historical metrics |
| `GET` | `/api/metrics/export` | Export metrics |
| `GET` | `/api/evaluation` | Evaluation dashboard fragment |

---

# Technology Stack

## Backend

- **Python**
- **FastAPI**
- **Uvicorn**
- **Pydantic**
- **SQLAlchemy**
- **Alembic**

## AI / LLM

- **Groq**
- **Llama 3.3 70B**
- **BAAI/bge-m3**
- **BAAI/bge-reranker-base**

## Retrieval

- **Qdrant**
- **BM25 / rank_bm25**
- **Reciprocal Rank Fusion (RRF)**
- **Cross-Encoder Reranking**
- **Metadata Filtering**
- **Dense Semantic Search**

## Document Processing

- **PyMuPDF**
- **python-docx**
- **Markdown parsing**
- **HTML parsing**
- **TXT parsing**

## External Search

- **Tavily**
- **HTTPX**

## Frontend

- **HTML**
- **CSS**
- **Vanilla JavaScript**
- **Chart.js**

---

# Project Structure

```text
Enterprise-Knowledge-Platform/
│
├── alembic/
│   ├── env.py
│   └── versions/
│
├── app/
│   ├── main.py
│   ├── api/
│   │   ├── routes.py
│   │   ├── search_routes.py
│   │   ├── ask_routes.py
│   │   └── meta_routes.py
│   ├── core/
│   │   ├── config.py
│   │   ├── llm.py
│   │   ├── web_search.py
│   │   ├── vector_store.py
│   │   ├── text_matching.py
│   │   └── logger.py
│   ├── database/
│   │   ├── database.py
│   │   ├── models.py
│   │   └── crud.py
│   ├── generation/
│   │   ├── answer_generator.py
│   │   └── web_search_agent.py
│   ├── ingestion/
│   │   ├── pipeline.py
│   │   ├── parser.py
│   │   ├── cleaner.py
│   │   ├── metadata_extractor.py
│   │   ├── company_normalization.py
│   │   ├── normalization.py
│   │   ├── content_metadata.py
│   │   ├── chunker.py
│   │   ├── embedder.py
│   │   └── duplicates.py
│   ├── models/
│   │   └── schemas.py
│   ├── retrieval/
│   │   ├── retriever.py
│   │   ├── entity_extraction.py
│   │   ├── query_understanding.py
│   │   ├── query_understanding_llm.py
│   │   ├── intent_rules.py
│   │   ├── bm25_index.py
│   │   ├── reranker.py
│   │   └── debug_trace.py
│   └── utils/
│       └── file_manager.py
│
├── frontend/
│   ├── index.html
│   ├── evaluation_fragment.html
│   ├── search.html
│   ├── css/
│   │   └── style.css
│   └── js/
│       ├── app.js
│       ├── dashboard.js
│       └── search.js
│
├── tests/
├── docs/
├── data/
│   ├── uploads/
│   ├── processed/
│   ├── qdrant_storage/
│   └── ecip.db
│
├── logs/
├── .env.example
├── .gitignore
├── requirements.txt
└── README.md
```

---

# Getting Started

## Prerequisites

- Python 3.x
- Git
- Internet access for Groq and Tavily functionality
- Sufficient local resources for embedding and reranking models

The default Qdrant configuration uses local/embedded storage, so a separate Qdrant server is not required for the default setup.

## 1. Clone the Repository

```bash
git clone <YOUR_REPOSITORY_URL>
cd Enterprise-Knowledge-Platform
```

## 2. Create a Virtual Environment

### Windows

```bash
python -m venv .venv
.venv\Scripts\activate
```

### macOS / Linux

```bash
python3 -m venv .venv
source .venv/bin/activate
```

## 3. Install Dependencies

```bash
pip install -r requirements.txt
```

## 4. Configure Environment Variables

Create a `.env` file based on `.env.example`.

```env
GROQ_API_KEY=your_groq_api_key
TAVILY_API_KEY=your_tavily_api_key

QDRANT_LOCAL_PATH=data/qdrant_storage

# Optional remote Qdrant configuration
QDRANT_URL=
QDRANT_API_KEY=
```

## 5. Start the Application

```bash
uvicorn app.main:app --reload --port 8000
```

Open:

```text
http://localhost:8000
```

---

# End-to-End RAG Flow

```text
DOCUMENT INGESTION
        │
        ▼
Enterprise Documents
        │
        ▼
Parsing
        │
        ▼
Duplicate Detection
        │
        ▼
Cleaning
        │
        ▼
Business Metadata
        │
        ▼
Normalization
        │
        ▼
LLM Content Enrichment
        │
        ▼
Parent-Child Chunking
        │
        ▼
Dense Embeddings
        │
        ▼
Qdrant + SQLite
        │
        ▼
      USER QUERY
        │
        ▼
Query Understanding
        │
        ▼
Query Expansion
        │
   ┌────┴────┐
   ▼         ▼
 Dense      BM25
 Search     Search
   │         │
   └────┬────┘
        ▼
    RRF Fusion
        │
        ▼
 Top 30 Candidates
        │
        ▼
Cross-Encoder Rerank
        │
        ▼
Confidence Evaluation
        │
   ┌────┴────┐
   ▼         ▼
Confident   Low
   │         │
   │    Filter Relaxation
   │         │
   │       Retry
   │         │
   └────┬────┘
        ▼
Parent Deduplication
        │
        ▼
      Top-K
        │
        ▼
Parent Context
        │
        ▼
LLM-Ready Context
        │
        ▼
Answer Generation
        │
        ▼
Citation-Grounded Answer
        │
        │
 Insufficient Internal Evidence
        │
        ▼
Company-Scoped Web Search
        │
        ▼
Web-Augmented Answer
```

---

# Error Handling & Reliability

The application is designed so optional AI and observability capabilities can degrade without unnecessarily breaking the core workflow.

Examples include:

- Duplicate documents are rejected before expensive processing.
- Missing company information can stop ingestion safely.
- Low-text PDFs are flagged for OCR.
- LLM content-metadata failures can degrade into warnings.
- Retrieval can retry with relaxed inferred filters.
- Low-confidence retrieval can prevent normal answer generation.
- Web-search failures do not replace the existing internal retrieval response.
- Metrics failures do not replace the primary ingestion/retrieval result.

# What This Project Demonstrates

## Generative AI

- LLM-powered query understanding
- Structured LLM extraction
- Grounded answer generation
- Document summarization
- Web-augmented generation

## Retrieval-Augmented Generation

- Enterprise document ingestion
- Text cleaning
- Semantic chunking
- Parent-child retrieval
- Dense embeddings
- Vector search
- Hybrid retrieval
- Reranking
- Context assembly
- Citation-grounded generation

## Information Retrieval

- Dense semantic retrieval
- BM25 lexical retrieval
- Query expansion
- Metadata filtering
- Reciprocal Rank Fusion
- Candidate generation
- Cross-encoder reranking
- Top-K retrieval
- Confidence scoring
- Retrieval retry and filter relaxation

## AI Engineering

- LLM integration
- Embedding model integration
- Retrieval pipeline design
- RAG failure handling
- Evaluation metrics
- Debug tracing
- Pipeline instrumentation
- Modular AI architecture

## Software Engineering

- FastAPI REST APIs
- SQLAlchemy persistence
- Alembic migrations
- Modular Python architecture
- Error handling
- Automated testing
- Logging
- Configuration management
- Frontend/backend integration

---

# Final Architecture Summary

```text
┌──────────────────────────────────────────────────────────────┐
│                 ENTERPRISE KNOWLEDGE PLATFORM                │
└──────────────────────────────────────────────────────────────┘

                         DOCUMENT SIDE
                              │
                              ▼
                 ┌─────────────────────────┐
                 │       INGESTION         │
                 │                         │
                 │ Parse                   │
                 │ Duplicate Detection     │
                 │ Clean                   │
                 │ Metadata                │
                 │ Normalize               │
                 │ LLM Enrichment          │
                 │ Chunk                   │
                 │ Embed                   │
                 │ Store                   │
                 └────────────┬────────────┘
                              │
                    ┌─────────┴─────────┐
                    ▼                   ▼
              ┌──────────┐        ┌──────────┐
              │  Qdrant  │        │  SQLite  │
              │          │        │          │
              │ Vectors  │        │ Documents│
              │ Chunks   │        │ Metrics  │
              │ Payloads │        │ Metadata │
              └────┬─────┘        └──────────┘
                   │
                   ▼
                         QUERY SIDE
                   │
                   ▼
             ┌───────────────┐
             │   User Query  │
             └───────┬───────┘
                     │
                     ▼
             Query Understanding
                     │
                     ▼
               Query Expansion
                     │
              ┌──────┴──────┐
              ▼             ▼
        Dense Search      BM25 Search
          Qdrant          rank_bm25
              │             │
              └──────┬──────┘
                     ▼
                 RRF Fusion
                     │
                     ▼
               Top 30 Candidates
                     │
                     ▼
              Cross-Encoder
                Reranking
                     │
                     ▼
             Confidence Gate
                     │
          ┌──────────┴──────────┐
          ▼                     ▼
     Confident              Low Confidence
          │                     │
          │               Filter Relaxation
          │                     │
          │                   Retry
          │                     │
          └──────────┬──────────┘
                     ▼
              Parent Deduplication
                     │
                     ▼
                   Top-K
                     │
                     ▼
             LLM-Ready Context
                     │
                     ▼
             Answer Generation
                     │
                     ▼
          Citation-Grounded Answer
                     │
                     │
              Insufficient Evidence
                     │
                     ▼
            Company-Scoped Web Search
                     │
                     ▼
              Web-Augmented Answer
```

---

# Key Takeaway

The platform follows a clear transformation from enterprise data to grounded AI responses:

```text
Raw Enterprise Documents
          ↓
Structured & Enriched Knowledge
          ↓
Parent-Child Semantic Chunks
          ↓
Dense + Lexical Retrieval
          ↓
RRF Candidate Fusion
          ↓
Cross-Encoder Reranking
          ↓
Confidence-Aware Evidence Selection
          ↓
LLM-Ready Context
          ↓
Grounded, Cited Answer
```

The core engineering principle is:

> **The LLM is not the retrieval system. The LLM operates on evidence produced by a dedicated ingestion, retrieval, ranking, and context-selection pipeline.**

This separation makes the architecture easier to evaluate, debug, optimize, and evolve.

---

# Project Summary

**Enterprise Knowledge Platform** is an end-to-end enterprise RAG application that transforms company documents into a structured, searchable knowledge base and enables users to retrieve relevant information and ask natural-language questions.

The project demonstrates practical implementation of:

**AI Engineering · Generative AI · RAG · LLM Applications · Dense Vector Search · BM25 · Hybrid Retrieval · Reciprocal Rank Fusion · Cross-Encoder Reranking · Parent-Child Retrieval · Metadata-Aware Search · Confidence-Aware Generation · Citation-Grounded Answers · FastAPI · Qdrant · Groq · Tavily · RAG Observability**









