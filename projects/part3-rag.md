# 📖 Part III Projects — RAG

Build up a real "chat with your docs" system, one concept at a time.
🟢 conceptual · 🟡 needs code/API · 🔴 heavier.

➡️ Concepts: [RAG](../rag/) · [Chunking](../rag/chunking.md) · [Retrieval](../rag/retrieval.md) · [Reranking](../rag/reranking.md) · [RAG Eval](../rag/evaluation.md) · [Architectures](../rag/architectures/) · [Knowledge Graphs](../knowledge-graphs/)

---

## 🔹 [RAG](../rag/) — "Chat with One PDF" 🟡
- **Goal:** the minimal end-to-end RAG loop.
- **Build:** Load a PDF → chunk → embed → store → on a question, retrieve top-k, stuff into the prompt, answer **with the source quote**.
- **Learn:** the whole index → retrieve → generate pipeline.
- **Stretch:** make it say *"I don't know"* when no chunk is relevant.

## 🔹 [Chunking](../rag/chunking.md) — "Chunking Bake-Off" 🟡
- **Goal:** feel chunking's impact.
- **Build:** Index the same doc three ways — **fixed-size**, **fixed+overlap**, **recursive** — and compare which retrieves the right passage for 10 questions.
- **Learn:** chunk strategy is the #1 quality lever.
- **Stretch:** add **semantic chunking** and re-score.

## 🔹 [Retrieval](../rag/retrieval.md) — "Hybrid Search" 🟡
- **Goal:** beat pure vector search.
- **Build:** Add **BM25** keyword search next to dense search; fuse with **RRF**. Test on queries with exact IDs/codes *and* fuzzy questions.
- **Learn:** hybrid wins because keywords + meaning cover each other's gaps.
- **Stretch:** add **query rewriting** (resolve a follow-up like "and for the EU?").

## 🔹 [Reranking](../rag/reranking.md) — "Add a Reranker" 🟡
- **Goal:** get the best chunk to rank #1.
- **Build:** Retrieve top-20, then re-score with a **cross-encoder** reranker; keep top-5. Compare answer quality before/after.
- **Learn:** retrieve wide, rerank narrow; fights "lost in the middle."
- **Stretch:** add a score floor → abstain when even the top chunk is weak.

## 🔹 [RAG Evaluation](../rag/evaluation.md) — "20-Question Eval Set" 🟡
- **Goal:** stop tuning on vibes.
- **Build:** Write 20 questions + their correct source chunks. Measure **context recall** (did we retrieve the right chunk?) and use an **LLM judge** for **faithfulness**.
- **Learn:** separate retrieval vs generation failures.
- **Stretch:** wire up **RAGAS** and track scores as you change chunking.

## 🔹 [RAG Architectures](../rag/architectures/) — "Adaptive Retrieval" 🟡
- **Goal:** don't always retrieve.
- **Build:** Add a step where the model first decides *"do I need to look this up?"* — skip retrieval for chit-chat, do it for factual questions (mini [Self-RAG](../rag/architectures/self-rag.md)).
- **Learn:** agentic RAG = decisions around retrieval.
- **Stretch:** add a [CRAG](../rag/architectures/corrective-rag.md)-style grader that falls back to web search when chunks are weak.

## 🔹 [Knowledge Graphs](../knowledge-graphs/) — "Extract a Mini KG" 🟡
- **Goal:** capture relationships, not just text.
- **Build:** Ask an LLM to extract **(subject → relation → object)** triples from a few paragraphs; store them and answer a "how are X and Y connected?" question by traversing.
- **Learn:** graphs enable multi-hop questions vector search misses.
- **Stretch:** visualize the graph (e.g. with `networkx` / mermaid) and answer a 2-hop question.

---

✅ **Part III checkpoint:** you can build a production-ish "chat with your docs" bot that
retrieves well, reranks, cites sources, and is measured by an eval set.

➡️ Next: **[Part IV — Customizing](./part4-customizing.md)** · or try the **[beginner capstone](./capstones.md)**.
