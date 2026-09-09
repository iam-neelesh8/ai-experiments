# 📏 Evaluating RAG

> **One-liner:** "It looks good" is not a metric. RAG has **two** things to measure —
> did you **retrieve** the right stuff, and did the model **generate** a faithful answer
> from it? You need both.

```mermaid
flowchart LR
    Q([Query]) --> R[Retrieval] --> G[Generation] --> A([Answer])
    R -. measure .-> RM[Retrieval metrics]
    G -. measure .-> GM[Generation metrics]
```

---

## 🎯 The RAG "triad"

```mermaid
flowchart TD
    Q[Question] --- C[Context<br/>retrieved chunks]
    C --- A[Answer]
    Q -. Answer Relevance<br/>does it address the question? .- A
    Q -. Context Relevance<br/>did we retrieve useful chunks? .- C
    C -. Faithfulness / Groundedness<br/>is the answer supported by the chunks? .- A
```

If all three hold, you have a trustworthy RAG system:
- **Context relevance** — are the retrieved chunks actually about the question?
- **Faithfulness (groundedness)** — is every claim in the answer supported by those chunks (no hallucination)?
- **Answer relevance** — does the answer actually respond to what was asked?

---

## 🔎 Retrieval metrics

You need a small **eval set**: questions paired with the chunks/documents that *should* be retrieved (the "ground truth").

| Metric | Question it answers |
|--------|---------------------|
| **Context Recall** | Of the chunks we *should* have found, how many did we? (Did we miss the answer?) |
| **Context Precision** | Of the chunks we returned, how many were relevant? (How much noise?) |
| **Hit Rate** | Was at least one correct chunk in the top-k? |
| **MRR** (Mean Reciprocal Rank) | How high up was the first correct chunk? |
| **nDCG** | Are the *most* relevant chunks ranked highest? |

```mermaid
flowchart LR
    subgraph TopK["Retrieved top-5"]
        d1[✅ relevant]
        d2[❌]
        d3[✅ relevant]
        d4[❌]
        d5[❌]
    end
    TopK --> P["Precision = 2/5"]
    TopK --> R["Recall = 2/3 relevant found"]
```

---

## 🧠 Generation metrics

Harder to measure because answers are free text. Common approaches:

| Approach | How |
|----------|-----|
| **LLM-as-a-judge** | A strong LLM scores faithfulness / relevance against the context. Scales well. |
| **Faithfulness check** | Break the answer into claims; verify each is supported by the retrieved context. |
| **Reference-based** | Compare to a gold answer (exact match / semantic similarity / ROUGE — weak for open-ended). |
| **Human review** | Gold standard, doesn't scale. Use for calibration. |

```mermaid
flowchart TD
    A[Generated answer] --> SP[Split into atomic claims]
    SP --> V{Each claim<br/>supported by context?}
    V -- all yes --> F[Faithful ✅]
    V -- some no --> H[Hallucination ❌]
```

---

## 🧪 Frameworks & tooling

| Tool | Focus |
|------|-------|
| **RAGAS** | Faithfulness, context precision/recall, answer relevance — purpose-built for RAG |
| **TruLens** | The RAG triad, feedback functions |
| **DeepEval / promptfoo** | Test-suite style eval, CI integration |
| **LangSmith / Phoenix (Arize)** | Tracing + eval on real traffic |

---

## 🔁 The eval loop (how you actually improve)

```mermaid
flowchart LR
    E[Eval set:<br/>Q + expected] --> RUN[Run pipeline]
    RUN --> SCORE[Score retrieval + generation]
    SCORE --> D{Where's the<br/>failure?}
    D -- bad chunks --> FIX1[Fix chunking / retrieval]
    D -- bad answer --> FIX2[Fix prompt / model]
    FIX1 & FIX2 --> RUN
```

**Debugging rule:** if the retrieved chunks *don't contain the answer*, it's a **retrieval** problem ([chunking](./chunking.md) / [retrieval](./retrieval.md)). If they *do* but the answer is still wrong, it's a **generation** problem (prompt / model / [context engineering](../context-engineering/)).

---

## ✅ A minimal eval to start today

1. Collect **20–50 real questions** with known correct sources.
2. Measure **context recall** (did the right chunk get retrieved at all?).
3. Use an **LLM judge** for **faithfulness** on the answers.
4. Change **one thing** (chunk size, add reranker, tweak prompt), re-run, compare.

That single loop will beat months of "vibes-based" tuning.

➡️ Back to [RAG overview](./README.md) · or explore [architectures](./architectures/).
