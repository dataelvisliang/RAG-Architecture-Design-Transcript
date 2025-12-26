# RAG-Architecture-Design-Transcript

Ultimate Architecture Design: Async Dual-Path Retrieval & Tiered Reranking RAG System for Transcription Data.

## 🚀 Business Vision
Precise discovery of customer pain points from massive recording transcripts using a "Bottom-layer Index Dehydration + Top-layer Logic Scoring" architecture. The system achieves < 1.0s End-to-End latency with high factual accuracy.

## 🏗️ Architecture Overview

```mermaid
graph TD
    UserQuery["User Query"] --> LLM_Rewrite["LLM Rewrite / Expansion"]
    
    subgraph Retrieval_Stage["1. Async Dual-Path Retrieval"]
        direction TB
        TaskA["Task A: Keyword Path"]
        TaskB["Task B: Vector Path"]
        
        LLM_Rewrite --> TaskA
        LLM_Rewrite --> TaskB
        
        TaskA --> SOS["Snowflake SOS"]
        SOS --> BM25["Python BM25"]
        
        TaskB --> VectorSearch["Snowflake Vector Search"]
    end
    
    BM25 --> RRF["RRF Fusion & De-duplication"]
    VectorSearch --> RRF
    
    subgraph Rerank_Funnel["2. Rerank Funnel"]
        direction TB
        RRF --> Rerank1["Rerank 1: Child Chunk (Zerank-2)"]
        Rerank1 --> Rerank2["Rerank 2: Parent Chunk (Zerank-2)"]
    end
    
    Rerank2 --> FinalOutput["Final Insight Report"]
```

## 📄 Key Components
- **[RAG Architecture Design](RAG_Architecture_Design.md):** Detailed technical documentation.
- **Parent-Child Strategy:** Context-aware retrieval ensuring truthfulness.
- **Async Dual-Path:** Combining Keyword (Snowflake SOS) and Semantic (Vector) search.
- **Tiered Reranking:** Escalating from short summaries to 8k context for ultimate precision.

## ⏱️ Latency SLA
| Stage | Action | Latency |
| :--- | :--- | :--- |
| **Retrieval** | Async Rewrite + Recall | ~450ms |
| **Rerank 1** | Child Chunk Scoring | ~300ms |
| **Rerank 2** | Parent Chunk Final Audit | ~400ms |
| **Total** | **End-to-End** | **< 1.0s (Parallelized)** |
