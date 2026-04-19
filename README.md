# 📚 Adaptive RAG System

> **Retrieval-Augmented Generation | Smart Caching | External Fallback | Hallucination Detection**

![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)
![Colab](https://img.shields.io/badge/Google%20Colab-Ready-orange.svg)
![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4.1--nano-green.svg)

## 🎯 What This System Does

| Your Action | System Response |
|-------------|-----------------|
| 📄 Upload PDFs | Extracts text, creates chunks, builds FAISS index |
| ❓ Ask a question | Searches documents + cache + external sources |
| ⚡ Same question again | Returns instant cached answer |
| 🌐 Question not in PDF | Fetches from Wikipedia or arXiv |
| ❌ Unrelated question | Politely refuses to answer |

**Bottom line:** Answers from your documents. Fetches from web when needed. Refuses off-topic questions.

## Features

### Document Processing
-  Multi-PDF upload with parent-child chunking (250/1500 chars)
-  File & chunk deduplication
-  FAISS vector database (IndexFlatIP)

### Query Processing
-  Query rewriter (typos, contractions, normalization)
-  Three-layer cache: Exact match + Type-aware + Semantic (FAISS)
-  Parallel processing (embedding + FAISS + keyword search)

### Smart Routing
-  3-tier confidence-based routing (<0.35 refuse, 0.35-0.60 external, 0.60-0.70 adaptive, >0.70 direct)
-  External fallback to Wikipedia + arXiv with cross-encoder relevance check

### Retrieval & Generation
-  Hybrid search (FAISS semantic + keyword BM25)
-  MMR (Maximum Marginal Relevance) for diverse chunks (λ=0.7)
-  Query type detection (definition, explain, compare, list, general)
-  Dynamic context size based on query type
-  GPT-4.1-nano answer generation (temperature=0 for deterministic)

### Validation & Learning
-  NLI hallucination detection (BART-MNLI)
-  Heuristic correctness check
-  Regeneration on validation failure (max 2 attempts)
-  Learning loop with JSONL logging

### User Interface
-  Gradio chat interface with conversation history
-  Confidence score display
-  Cache hit indicator
-  External source indicator
-  Sections used display

## 🛠️ Tech Stack

| Component | Tool/Model |
|-----------|------------|
| PDF Processing | `pdfplumber`, `langchain-text-splitters` |
| Embeddings | `all-MiniLM-L6-v2` |
| Vector DB | FAISS |
| LLM | GPT-4.1-nano (OpenAI API) |
| External Search | Wikipedia API, arXiv API |
| Relevance | Cross-encoder `ms-marco-MiniLM-L-6-v2` |
| Hallucination | `bart-large-mnli` |
| UI | Gradio |

## 🚀 How to Run in Colab

### Step 1: Open Google Colab

Go to [colab.research.google.com](https://colab.research.google.com)

### Step 2: Add OpenAI API key to Secrets

Click 🔑 Secrets (left sidebar) and add:

| Secret Name | Value |
|-------------|-------|
| `OPENAI_API_KEY` | Your OpenAI API key |
| `OPENAI_BASE_URL` | `https://api.openai.com/v1` |
| `OPENAI_MODEL` | `gpt-4.1-nano` |

### Step 3: Copy all code cells in order

- Phase 0: PDF Processing
- Phase 1: Query Processing
- Phase 2: Smart Routing
- Phase 3: Adaptive Processing
- Phase 3.5: External Retriever
- Phase 4: Answer Generation
- Phase 5: Answer Validation
- Phase 6: RAG Orchestrator + Gradio UI

### Step 4: Run all cells

Click `Runtime → Run all`

### Step 5: Use the Gradio interface

Click the Gradio link that appears after running the last cell

## 📊 Output Indicators

| Symbol | Meaning |
|--------|---------|
| ⚡ | Answer from cache (instant) |
| 🌐 | Includes external sources (Wikipedia/arXiv) |
| 📊 | Confidence score (0-1 scale) |
| 📄 | Number of document sections used |

## 🔧 Architecture Flow

```mermaid
flowchart LR
    A[📄 Upload PDF] --> B[Chunk & Embed]
    B --> C[FAISS Index]
    
    D[❓ User Query] --> E{⚡ Cache?}
    E -->|HIT| F[Return Answer]
    E -->|MISS| G[FAISS Search]
    
    G --> H{Score?}
    H -->|<0.35| I[❌ Refuse]
    H -->|0.35-0.60| J[🌐 Wikipedia/arXiv]
    H -->|0.60-0.70| K[🔄 Expand Query]
    H -->|>0.70| L[🤖 GPT]
    
    J --> L
    K --> L
    L --> M[✅ Validate]
    M -->|Pass| N[💾 Cache]
    M -->|Fail| K
    N --> F
    I --> F
