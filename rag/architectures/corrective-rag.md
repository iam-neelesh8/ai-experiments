# 🩹 Corrective RAG (CRAG)

> **One-liner:** Corrective RAG adds a **retrieval grader** that checks whether the retrieved
> chunks are actually good — and if they're **not**, it *corrects course* (e.g. falls back
> to a web search) instead of answering from bad context.

```mermaid
flowchart TD
    Q([Query]) --> R[Retrieve chunks]
    R --> GRADE{Retrieval grader:<br/>chunks good?}
    GRADE -- Correct --> USE[Use chunks]
    GRADE -- Ambiguous --> BOTH[Use chunks + web search]
    GRADE -- Incorrect --> WEB[Discard → web search]
    USE & BOTH & WEB --> GEN[(Generate)]
    GEN --> A([Answer])
```

---

## 🧠 The key idea: grade, then act

A lightweight **evaluator** scores retrieved documents into one of three buckets, each with a
different corrective action:

| Grade | Meaning | Action |
|-------|---------|--------|
| **Correct** | Chunks are relevant | Use them (after refining) |
| **Incorrect** | Chunks are off | Discard → **fall back to web search** |
| **Ambiguous** | Not sure | Use **both** local chunks and web |

```mermaid
flowchart LR
    D[Retrieved docs] --> E[Retrieval evaluator]
    E --> C[✅ Correct]
    E --> I[❌ Incorrect → web]
    E --> A[🤔 Ambiguous → both]
```

---

## 🔧 Knowledge refinement

For "correct" docs, CRAG often **decompose-then-filter**: break chunks into smaller
"knowledge strips," keep the relevant strips, drop the noise — so only high-signal context
reaches the model.

```mermaid
flowchart LR
    CHUNK[Chunk] --> STRIPS[Split into strips]
    STRIPS --> KEEP[Keep relevant strips]
    KEEP --> CLEAN[Clean, focused context]
```

---

## ✨ Why it helps

```mermaid
flowchart LR
    PROB["Problem: retrieval sometimes<br/>returns irrelevant/wrong chunks"] --> BAD["Naive RAG answers from<br/>bad context → wrong answer"]
    CRAG["CRAG detects bad retrieval<br/>→ web fallback + refinement"] --> GOOD["✅ Recovers instead of<br/>confidently failing"]
```

It directly attacks the **"right doc wasn't retrieved"** failure mode → fewer [hallucinations](../../hallucination/).

---

## ⚖️ CRAG vs Self-RAG

| | [Self-RAG](./self-rag.md) | Corrective RAG |
|---|---------------------------|----------------|
| Core mechanism | Model's own **reflection tokens** | External **retrieval grader** |
| Needs special training? | Yes (trained tokens) | Lighter — grader can be a small model |
| Signature move | Decide *when* to retrieve + self-critique | *Grade* retrieval, **web-search fallback** |
| Both aim to… | Filter bad context, reduce hallucination | Same goal, different route |

Both are **[Agentic RAG](./README.md#4-agentic-rag)** patterns — they add a decision/critique
loop on top of plain retrieval.

➡️ Compare with **[Self-RAG](./self-rag.md)** · back to **[architectures](./README.md)**.
