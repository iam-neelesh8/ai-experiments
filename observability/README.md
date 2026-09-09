# 📡 LLM Observability & LLMOps

> **One-liner:** Once an LLM app is live, you need to **see what it's actually doing** —
> trace every call, measure quality/cost/latency, and catch regressions. Observability is
> "logging + tracing + evals" for non-deterministic AI systems.

```mermaid
flowchart LR
    APP[LLM app] --> TRACE[Trace every call]
    TRACE --> STORE[(Store: inputs, outputs,<br/>tokens, latency, cost)]
    STORE --> DASH[Dashboards + alerts]
    STORE --> EVAL[Run evals on real traffic]
```

---

## 🤔 Why LLM apps need special observability

Traditional apps are deterministic; LLM apps are **non-deterministic**, multi-step, and fail
*softly* (a plausible wrong answer, not a stack trace).

```mermaid
flowchart LR
    T["Traditional: crash = obvious error"] 
    L["LLM: 'works' but answer is subtly wrong 😬"]
```

You can't just watch for exceptions — you have to watch **quality**.

---

## 🧵 Tracing a multi-step call

A single user request can fan out into retrieval, reranking, several LLM calls, and tool
calls. Tracing ties them into one **timeline** you can inspect.

```mermaid
flowchart TD
    Q([User request]) --> R[Retrieve chunks]
    R --> RR[Rerank]
    RR --> L1[LLM call 1]
    L1 --> TOOL[Tool call]
    TOOL --> L2[LLM call 2]
    L2 --> A([Answer])
    R & RR & L1 & TOOL & L2 -.logged as spans.-> TRACE[(One trace)]
```

Essential for debugging [RAG](../rag/) and [agents](../agents/) — you can see *exactly* which
step went wrong (bad retrieval? bad prompt? bad tool result?).

---

## 📊 What to measure

| Category | Metrics |
|----------|---------|
| **Quality** | Faithfulness, relevance, task success, thumbs up/down |
| **Cost** | Tokens & $ per request / per user / per feature |
| **Latency** | TTFT, total time, per-step timing |
| **Reliability** | Error rates, tool failures, timeouts, retries |
| **Safety** | Flagged/blocked content, injection attempts → [guardrails](../guardrails/) |
| **Usage** | Volume, top queries, cache hit rate |

---

## 🔁 Offline evals vs online monitoring

```mermaid
flowchart LR
    OFF["Offline evals<br/>-fixed test set, pre-ship-"] --> SHIP[Ship]
    SHIP --> ON["Online monitoring<br/>-real traffic, post-ship-"]
    ON --> FEED[Feed real failures<br/>back into the eval set]
    FEED --> OFF
```

- **Offline** ([evaluation](../evaluation/)) catches regressions before release.
- **Online** catches what your test set missed — then you *add those cases* to the eval set.

---

## 🧰 Tooling & the LLMOps picture

LangSmith, Langfuse, Arize Phoenix, Helicone, Weights & Biases, OpenTelemetry (GenAI). Often
paired with prompt versioning, dataset management, and A/B testing — collectively **LLMOps**.

---

## 🗺️ Where observability matters

- **Any production LLM app** — you can't improve what you can't see.
- **[Agents](../agents/)** — many steps = many failure points to trace.
- **Cost control** — find the expensive calls (pairs with [model-routing](../model-routing/), [semantic-caching](../semantic-caching/)).
- **Safety & compliance** — audit trails of what the model did.

➡️ Related: **[evaluation](../evaluation/)** · **[guardrails](../guardrails/)** · **[inference-optimization](../inference-optimization/)**.
