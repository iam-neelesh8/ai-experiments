# 🏗️ LLM System Design — Putting the Pieces Together

> **One-liner:** Individual concepts are Lego bricks. This page shows how they **click
> together** into real systems — the reference architectures an [FDE](../fde/) actually
> ships. Read this after you know the pieces; it's the "so how do I build the whole thing?"

```mermaid
flowchart LR
    U([User]) --> APP[Your app]
    APP --> ORCH[Orchestration layer]
    ORCH --> M[(LLM)]
    ORCH --> RET[Retrieval]
    ORCH --> TOOLS[Tools]
    ORCH --> GUARD[Guardrails]
    ORCH --> OBS[Observability]
```

---

## 🧭 The decision tree: what do I even build?

```mermaid
flowchart TD
    S{What does the task need?} --> Q1{Just better instructions?}
    Q1 -- yes --> PE[Prompted LLM call]
    Q1 -- no --> Q2{Needs private / fresh knowledge?}
    Q2 -- yes --> RAG[RAG system]
    Q2 -- no --> Q3{Needs multi-step actions / tools?}
    Q3 -- yes --> AG[Agent]
    Q3 -- no --> Q4{Needs consistent style / format / narrow skill?}
    Q4 -- yes --> FT[Fine-tuned model]
```

> Start at the **top** (cheapest, most reliable) and only move down when the task forces it.
> Most production systems are a **combination** — e.g. a fine-tuned model inside a RAG
> pipeline wrapped by an agent.

---

## 🏛️ Reference architecture 1 — Production RAG assistant

```mermaid
flowchart TD
    subgraph Offline["🏗️ Indexing -offline-"]
        DOC[Documents] --> CH[Chunk] --> EM[Embed] --> VDB[(Vector DB)]
    end
    subgraph Online["⚡ Serving -per query-"]
        Q([Query]) --> GIN[Input guardrails]
        GIN --> RW[Query rewrite]
        RW --> HY[Hybrid retrieve]
        VDB --> HY
        HY --> RR[Rerank]
        RR --> PR[Prompt + context]
        PR --> LLM[(LLM)]
        LLM --> GOUT[Output guardrails<br/>+ groundedness]
        GOUT --> A([Answer + citations])
    end
    A -.trace.-> OBS[(Observability)]
    A -.sample.-> EVAL[Evals]
```

**Bricks used:** [chunking](../rag/chunking.md) · [embeddings](../embeddings/) ·
[vector DB](../vector-databases/) · [retrieval](../rag/retrieval.md) ·
[reranking](../rag/reranking.md) · [guardrails](../guardrails/) ·
[evaluation](../rag/evaluation.md) · [observability](../observability/).

---

## 🤖 Reference architecture 2 — Tool-using agent

```mermaid
flowchart TD
    Q([Goal]) --> AG{{Agent loop}}
    AG --> PLAN[Plan / reason]
    PLAN --> ACT{Choose action}
    ACT -->|retrieve| RAG[RAG]
    ACT -->|act| TOOLS[Tools / APIs -MCP-]
    ACT -->|recall| MEM[Memory]
    RAG & TOOLS & MEM --> OBSV[Observe result]
    OBSV --> DONE{Goal met / limit?}
    DONE -- no --> AG
    DONE -- yes --> OUT([Result])
    ACT -.risky action.-> HUM[Human approval]
```

**Bricks used:** [agentic loop](../agentic-loop/) · [architectures](../agents/architectures.md) ·
[tools/MCP](../agents/tools.md) · [memory](../agents/memory.md) · [RAG](../rag/) ·
[guardrails](../guardrails/) · loop limits.

---

## 🎛️ The cross-cutting layers (every serious system has these)

```mermaid
flowchart LR
    CORE[LLM core] --- SEC[🔒 Security & data boundaries]
    CORE --- GRD[🛡️ Guardrails]
    CORE --- EVL[📊 Evals]
    CORE --- OBS[📡 Observability]
    CORE --- COST[💰 Cost & latency]
```

| Layer | Concern | Bricks |
|-------|---------|--------|
| **Security** | Data boundaries, secrets, permissions, PII | [guardrails](../guardrails/), [federated](../federated-learning/) |
| **Safety** | Injection, harmful output, unsafe actions | [guardrails](../guardrails/), [red-teaming](../red-teaming/) |
| **Quality** | Is it right? Did a change regress? | [evaluation](../evaluation/), [hallucination](../hallucination/) |
| **Ops** | Trace, monitor, debug live | [observability](../observability/) |
| **Cost/latency** | Serve it affordably & fast | [routing](../model-routing/), [caching](../semantic-caching/), [inference opt](../inference-optimization/) |

---

## 🧱 Design principles

```mermaid
flowchart TD
    P1[Simplest thing that works<br/>-prompt → RAG → agent → finetune-]
    P2[Measure everything<br/>-evals before & after every change-]
    P3[Never trust model output<br/>-validate, guard, cite-]
    P4[Design for failure<br/>-fallbacks, limits, human-in-loop-]
    P5[Optimize cost last<br/>-first make it work, then cheap-]
```

- **Start simple, escalate deliberately.** Complexity is a cost you pay in reliability.
- **Evals are your ground truth.** Every architecture choice should be validated, not vibed.
- **Treat the model as an untrusted, brilliant intern** — verify its work, bound its power.

---

## 🗺️ Where this fits

This is the **capstone** page — the [FDE](../fde/) reads it to turn a pile of concepts into a
shippable design. Come back here whenever you're staring at a blank architecture diagram.

➡️ Back to the **[learning path](../README.md#-the-path-to-forward-deployed-engineer)** · the role → **[FDE](../fde/)**.
