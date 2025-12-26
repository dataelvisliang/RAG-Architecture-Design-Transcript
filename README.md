# RAG-Architecture-Design-Transcript

Ultimate Architecture Design: Async Dual-Path Retrieval & Tiered Reranking RAG System for Transcription Data.

## 🚀 Business Vision
Precise discovery of customer pain points from massive recording transcripts using a "Bottom-layer Index Dehydration + Top-layer Logic Scoring" architecture. The system achieves < 1.0s End-to-End latency with high factual accuracy.

## 🏗️ Architecture Overview

```mermaid
graph TD
    %% Node Definitions
    UserQuery["User Query"]:::input
    LLM_Rewrite["LLM Rewrite / Expansion"]:::process
    
    subgraph Retrieval_Stage["1. Async Dual-Path Retrieval"]
        direction TB
        TaskA["Task A: Keyword Path"]:::subtask
        TaskB["Task B: Vector Path"]:::subtask
        
        SOS["Snowflake SOS"]:::internal
        BM25["Python BM25"]:::internal
        VectorSearch["Snowflake Vector Search"]:::internal
    end

    LLM_Rewrite --> TaskA
    LLM_Rewrite --> TaskB
    
    BM25 --> RRF["RRF Fusion & De-duplication"]:::process
    VectorSearch --> RRF
    
    subgraph Rerank_Funnel["2. Rerank Funnel"]
        direction TB
        Rerank1["Rerank 1: Child Chunk (Zerank-2)"]:::rerank
        Rerank2["Rerank 2: Parent Chunk (Zerank-2)"]:::rerank
        
        RRF --> Rerank1 --> Rerank2
    end
    
    Rerank2 --> FinalOutput["Final Insight Report"]:::output

    %% Class Definitions for Premium Look
    classDef input fill:#EDF2FF,stroke:#4C6EF5,stroke-width:2px,color:#1971C2,font-size:13px;
    classDef process fill:#F8F9FA,stroke:#495057,stroke-width:2px,color:#212529,font-size:13px;
    classDef subtask fill:#E3FAFC,stroke:#0C8599,stroke-width:2px,color:#0B7285,font-size:12px;
    classDef internal fill:#FFF,stroke:#ADB5BD,stroke-width:1px,color:#495057,font-size:12px,stroke-dasharray: 5 5;
    classDef rerank fill:#FFF4E6,stroke:#FD7E14,stroke-width:2px,color:#D9480F,font-size:13px;
    classDef output fill:#EBFBEE,stroke:#40C057,stroke-width:2px,color:#2B8A3E,font-size:14px,font-weight:bold;

    %% Subgraph Styling
    style Retrieval_Stage fill:#f8f9fa,stroke:#dee2e6,stroke-width:1px,color:#868e96
    style Rerank_Funnel fill:#fff9db,stroke:#fab005,stroke-width:1px,color:#f08c00
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
