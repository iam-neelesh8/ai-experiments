# 🏛️ RAG Architectures

RAG isn't one thing — it's a spectrum from "embed and search" to fully agentic,
self-correcting retrieval systems. Here's the evolution, each with a diagram and
**where it's used**.

```mermaid
flowchart LR
    N[Naive RAG] --> A[Advanced RAG] --> M[Modular RAG] --> AG[Agentic RAG]
    A --> G[Graph RAG]
    classDef n fill:#1f6feb22,stroke:#1f6feb,color:#c9d1d9;
    class N,A,M,AG,G n;
```

| Architecture | Core idea | Complexity | Best for |
|--------------|-----------|-----------|----------|
| [Naive RAG](#1-naive-rag) | retrieve → stuff → generate | ⭐ | Prototypes, simple Q&A |
| [Advanced RAG](#2-advanced-rag) | pre- & post-retrieval optimization | ⭐⭐ | Most production systems |
| [Modular RAG](#3-modular-rag) | swappable, reconfigurable components | ⭐⭐⭐ | Complex, evolving platforms |
| [Agentic RAG](#4-agentic-rag) | an agent decides *how/what/when* to retrieve | ⭐⭐⭐⭐ | Multi-step, multi-source research |
| [Graph RAG](#5-graph-rag) | retrieve over a knowledge graph | ⭐⭐⭐⭐ | Connected data, "how are X and Y related" |

---

## 1. Naive RAG

The original pattern: embed, retrieve top-k, concatenate, generate.

```mermaid
flowchart LR
    Q([Query]) --> E[Embed] --> S[Top-k search] --> P[Prompt + chunks] --> LLM[(LLM)] --> A([Answer])
```

- ✅ Dead simple, fast to build.
- ❌ No query cleanup, no reranking, no checks. Fragile on real data.
- **Used for:** demos, MVPs, small clean corpora.

## 2. Advanced RAG

Add optimization **before** retrieval (query rewriting, [HyDE](../retrieval.md), routing)
and **after** retrieval ([reranking](../reranking.md), compression, filtering).

```mermaid
flowchart TD
    Q([Query]) --> PRE[Pre-retrieval:<br/>rewrite · route · HyDE]
    PRE --> HYB[Hybrid retrieval]
    HYB --> POST[Post-retrieval:<br/>rerank · compress · dedupe]
    POST --> LLM[(LLM)] --> A([Answer + citations])
```

- ✅ The big quality jump over naive; still a linear pipeline.
- **Used for:** the majority of serious production RAG (support bots, doc chat).

## 3. Modular RAG

Treat each capability (retrieve, rerank, memory, routing, fusion) as a **swappable
module** you can rewire — including loops and branches, not just a straight line.

```mermaid
flowchart LR
    Q([Query]) --> RT{Router}
    RT -->|FAQ| MOD1[Vector module]
    RT -->|numbers| MOD2[SQL / DB module]
    RT -->|web| MOD3[Web search module]
    MOD1 & MOD2 & MOD3 --> FUSE[Fusion] --> LLM[(LLM)]
```

- ✅ Flexible, reusable, testable pieces; route to the right data source.
- **Used for:** platforms serving many query types / data sources.

## 4. Agentic RAG

An **[agent](../../agents/)** sits on top and *decides*: Do I even need to retrieve?
Which source? Is this result good enough, or should I search again? It can **loop**
and **self-correct**.

```mermaid
flowchart TD
    Q([Query]) --> AG{{Agent}}
    AG -->|need info?| DEC{Decide source}
    DEC --> V[Vector store]
    DEC --> W[Web search]
    DEC --> DB[(Database)]
    V & W & DB --> J{Good enough?}
    J -- no, refine --> AG
    J -- yes --> LLM[(LLM)] --> A([Answer])
```

Includes patterns like **Self-RAG** (model critiques its own retrieval/answer) and
**Corrective RAG / CRAG** (grade retrieved docs; fall back to web search if they're weak).

- ✅ Handles multi-step, multi-source, "research"-style questions; recovers from bad retrieval.
- ❌ Slower, costlier, harder to make reliable. See the [agentic loop](../../agentic-loop/).
- **Used for:** research assistants, complex enterprise Q&A, deep-research tools.

## 5. Graph RAG

Retrieve over a **[knowledge graph](https://en.wikipedia.org/wiki/Knowledge_graph)**
(entities + relationships) instead of (or alongside) flat text chunks. Great when the
answer depends on **connections** across documents.

```mermaid
flowchart LR
    Q([Query]) --> EX[Extract entities]
    EX --> KG[(Knowledge graph)]
    KG --> SUB[Relevant subgraph +<br/>community summaries]
    SUB --> LLM[(LLM)] --> A([Answer])
```

- ✅ Answers "how are X and Y related?" and global "what are the main themes?" questions that flat RAG struggles with; multi-hop reasoning.
- ❌ Building & maintaining the graph is expensive.
- **Used for:** interconnected corpora — research literature, org knowledge, investigations, compliance.

---

## 🧭 Choosing

```mermaid
flowchart TD
    S{Start} --> Q1{Simple Q&A over<br/>clean docs?}
    Q1 -- yes --> NAIVE[Naive → Advanced]
    Q1 -- no --> Q2{Many sources /<br/>query types?}
    Q2 -- yes --> MOD[Modular]
    Q2 -- no --> Q3{Multi-step research<br/>or self-correction?}
    Q3 -- yes --> AGG[Agentic]
    Q3 -- no --> Q4{Answer depends on<br/>relationships?}
    Q4 -- yes --> GRAPH[Graph RAG]
    Q4 -- no --> ADV[Advanced RAG]
```

**Rule of thumb:** start at **Advanced RAG**. Only reach for Agentic/Graph when you can point at a concrete failure that a simpler pipeline can't fix.

➡️ Back to [RAG overview](../README.md).
