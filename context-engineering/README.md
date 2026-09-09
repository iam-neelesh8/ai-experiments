# 🪟 Context Engineering

> **One-liner:** Context engineering is the discipline of deciding **everything that goes
> into the context window** — instructions, memory, retrieved data, tool outputs, history —
> and doing it within a **finite token budget**. It's the superset of
> [prompt engineering](../prompt-engineering/) for real, agentic systems.

> 📌 *"Prompt engineering" is what you write once. "Context engineering" is what you assemble,
> dynamically, on every single call.*

```mermaid
flowchart LR
    subgraph Window["🪟 Context window (finite!)"]
        SYS[System / instructions]
        MEM[Memory]
        RAG[Retrieved docs]
        TOOL[Tool outputs]
        HIST[Conversation history]
        Q[Current input]
    end
    Window --> LLM[(LLM)]
```

---

## 🎯 Why it exists: the window is finite

Every model has a **context limit**. You're always making a budget decision: *what earns
its place in the window right now?* More is **not** better — irrelevant context hurts.

```mermaid
flowchart TD
    ALL[Everything you *could* include] --> BUDGET{Token budget}
    BUDGET --> KEEP[✅ Relevant, high-signal]
    BUDGET --> DROP[❌ Cut / summarize / offload]
    KEEP --> LLM[(LLM)]
```

---

## ⚠️ The failure modes it fights

```mermaid
flowchart TD
    F1["🌫️ Context rot<br/>quality drops as the window fills"]
    F2["🎯 Lost in the middle<br/>models under-weight the middle of long contexts"]
    F3["💸 Cost & latency<br/>every token is paid for, every call"]
    F4["🧨 Distraction / conflict<br/>irrelevant or contradictory context misleads"]
    F5["☠️ Prompt injection<br/>malicious text sneaks in via docs/tools"]
```

The whole job is getting the **right** information — and *only* the right information — in front of the model at the right time.

---

## 🧰 Core techniques

```mermaid
flowchart LR
    W[Write<br/>save outside the window] 
    S[Select<br/>pull in only what's relevant]
    C[Compress<br/>summarize / trim]
    I[Isolate<br/>split across agents/calls]
```

| Technique | What it means | Example |
|-----------|---------------|---------|
| **Write** | Persist info *outside* the window | Scratchpads, [long-term memory](../agents/memory.md) |
| **Select** | Retrieve only what's relevant | [RAG](../rag/), memory retrieval, tool-result filtering |
| **Compress** | Shrink what you keep | Summarize old turns, trim tool outputs |
| **Isolate** | Partition context | [Multi-agent](../agents/architectures.md) — each agent its own clean context |

---

## 📦 What competes for the window

```mermaid
flowchart TD
    W[🪟 Window] --> A[System prompt & rules]
    W --> B[Few-shot examples]
    W --> C[Retrieved knowledge -RAG-]
    W --> D[Conversation history]
    W --> E[Long-term memories]
    W --> F[Tool definitions & outputs]
    W --> G[The actual user request]
    W --> H[Room for the response!]
```

> 🧮 Don't forget to leave headroom for the **output**. Filling the window to the brim leaves no room to answer.

---

## 🔁 Managing a long conversation

```mermaid
flowchart LR
    H[Growing history] --> CHK{Near limit?}
    CHK -- no --> PASS[Keep as-is]
    CHK -- yes --> SUM[Summarize old turns]
    SUM --> KEEP[Summary + recent turns]
    KEEP --> LLM[(LLM)]
```

Strategies: **sliding window** (last N turns), **summarization** (compress the old),
**retrieval** (fetch only relevant past turns), **offloading** (store details, recall on demand).

---

## 🗺️ Where context engineering lives

- **Agentic systems** — the make-or-break skill; agents generate tons of tool output that must be curated.
- **RAG** — deciding *how many* chunks, in what order, with what instructions.
- **Long conversations** — chat assistants that must stay coherent for hours.
- **Multi-agent** — giving each agent a focused, isolated context.

---

## ✅ Principles

```mermaid
flowchart TD
    P1[Relevance over volume<br/>less, but right]
    P2[Order matters<br/>put key info at the edges, not buried]
    P3[Structure it<br/>clear sections & delimiters]
    P4[Budget deliberately<br/>track tokens, leave output room]
    P5[Isolate untrusted content<br/>guard against injection]
```

**The mantra:** *the smallest set of high-signal tokens that fully specifies the task.*

➡️ It all comes together in the loop → **[agentic loop](../agentic-loop/)**.
