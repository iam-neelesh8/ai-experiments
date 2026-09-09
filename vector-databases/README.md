# 🗄️ Vector Databases

> **One-liner:** A vector database stores [embeddings](../embeddings/) and finds the
> **nearest** ones to a query vector — fast — using **Approximate Nearest Neighbor (ANN)**
> indexes. It's the storage + search engine that makes [RAG](../rag/) and semantic search work at scale.

```mermaid
flowchart LR
    Q[Query vector] --> DB[(Vector DB)]
    DB --> IDX[ANN index]
    IDX --> K[Top-k nearest<br/>+ metadata]
```

---

## ⚡ Why not just compare all vectors?

Comparing a query against **millions** of vectors one by one (brute force / "flat") is
accurate but too slow. ANN indexes trade a **tiny** bit of accuracy for **massive** speed.

```mermaid
flowchart LR
    BF[Brute force<br/>exact, O-n-, slow] -->|scale up| PROB[Too slow for millions]
    PROB --> ANN[ANN index<br/>approximate, fast]
```

---

## 🧭 The main ANN index types

```mermaid
flowchart TD
    ANN[ANN indexes] --> HNSW[HNSW<br/>graph of neighbors]
    ANN --> IVF[IVF<br/>cluster then search a few]
    ANN --> PQ[PQ<br/>compress vectors]
    ANN --> FLAT[Flat<br/>exact brute force]
```

| Index | Idea | Trade-off |
|-------|------|-----------|
| **HNSW** | Navigable small-world graph; hop toward nearest | Great speed/recall; more memory |
| **IVF** | Partition into clusters; search only nearby clusters | Fast; needs training/tuning |
| **PQ** (Product Quantization) | Compress vectors to save memory | Smaller/faster; some accuracy loss |
| **Flat** | Exact search | Perfect recall; slow at scale |

Often combined (e.g. **IVF-PQ**, **HNSW-PQ**).

---

## 🎛️ Beyond similarity: what a vector DB adds

```mermaid
flowchart LR
    V[Vector search] --> F[Metadata filtering]
    V --> H[Hybrid -vector + keyword-]
    V --> S[Scaling / sharding]
    V --> C[CRUD + persistence]
    V --> M[Multi-tenancy / security]
```

- **[Metadata filtering](../rag/retrieval.md)** — "only search docs from 2024, region=EU."
- **[Hybrid search](../rag/retrieval.md)** — combine vector + keyword (BM25).
- **Scale & durability** — sharding, replication, updates, deletes.

---

## 🧰 The landscape

| Option | Flavor |
|--------|--------|
| **FAISS** | Library (not a DB); very fast, local, DIY |
| **Chroma** | Lightweight, great for prototyping |
| **Qdrant / Weaviate / Milvus** | Full-featured dedicated vector DBs |
| **pgvector** | Postgres extension — vectors beside relational data |
| **Elasticsearch / OpenSearch** | Search engines with vector support |
| **Pinecone** | Managed, serverless |

```mermaid
flowchart TD
    Q{Your situation} --> P[Prototype] --> CHROMA[Chroma / FAISS]
    Q --> PG[Already on Postgres] --> PGV[pgvector]
    Q --> SCALE[Large scale, dedicated] --> DED[Qdrant/Weaviate/Milvus/Pinecone]
```

---

## 🗺️ Where vector DBs are used

- **[RAG](../rag/)** retrieval (the headline use case)
- **Semantic search** over any corpus
- **[Agent memory](../agents/memory.md)** (long-term recall)
- **Recommendations, dedup, anomaly detection**

➡️ Powered by **[embeddings](../embeddings/)** · feeds **[RAG retrieval](../rag/retrieval.md)**.
