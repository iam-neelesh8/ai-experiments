# 🏭 Part VI Projects — Production-Grade AI

Harden the RAG bot / agent you already built. 🟢 conceptual · 🟡 needs code/API.

➡️ Concepts: [Evaluation](../evaluation/) · [Observability](../observability/) · [Guardrails](../guardrails/) · [Hallucination](../hallucination/) · [Red-Teaming](../red-teaming/) · [Inference Opt](../inference-optimization/) · [Semantic Caching](../semantic-caching/) · [Model Routing](../model-routing/)

---

## 🔹 [Evaluation](../evaluation/) — "LLM-as-Judge Harness" 🟡
- **Goal:** automated, repeatable scoring.
- **Build:** For 15 outputs, write a rubric and have a strong model score each 1–5 with a reason. Store results so you can re-run after changes.
- **Learn:** evals are the new unit tests; judge with a rubric.
- **Stretch:** test for **position bias** — swap answer order and see if the judge flips.

## 🔹 [Observability](../observability/) — "Trace Your RAG App" 🟡
- **Goal:** see every step of a request.
- **Build:** Add tracing (Langfuse/Phoenix, or just structured logs) capturing retrieval, prompt, tokens, latency, and cost per request.
- **Learn:** you can't improve what you can't see; trace to localize failures.
- **Stretch:** build a tiny dashboard of cost & latency over your last 50 runs.

## 🔹 [Guardrails](../guardrails/) — "Input/Output Filters" 🟡
- **Goal:** safe in, safe out.
- **Build:** Add an **input** check (block obvious [prompt injection](../guardrails/) like "ignore your instructions") and an **output** check (block PII / require a citation).
- **Learn:** two checkpoints; never trust raw model output.
- **Stretch:** detect indirect injection hidden inside a retrieved document.

## 🔹 [Hallucination](../hallucination/) — "Groundedness Checker" 🟡
- **Goal:** catch unsupported claims.
- **Build:** Split an answer into claims; for each, ask a model *"is this supported by the provided context? yes/no + quote."* Flag unsupported ones.
- **Learn:** faithfulness = every claim traces to a source.
- **Stretch:** auto-regenerate the answer using only the supported claims.

## 🔹 [Red-Teaming](../red-teaming/) — "Break Your Own Bot" 🟡
- **Goal:** find failures before users do.
- **Build:** Write 15 attack prompts (jailbreaks, injection, PII extraction). Run them; log which succeed; add the winners as permanent eval cases.
- **Learn:** offense hardens defense; findings become regression tests.
- **Stretch:** have a model **generate** 100 attack variants automatically.

## 🔹 [Inference Optimization](../inference-optimization/) — "Measure & Speed Up" 🟡
- **Goal:** feel latency & throughput.
- **Build:** Measure **TTFT** and **tokens/sec** for a request; try **streaming** vs not; enable **prompt caching** if your provider supports it and re-measure.
- **Learn:** TTFT vs throughput; caching helps repeated prefixes.
- **Stretch:** batch 10 requests and compare total time vs sequential.

## 🔹 [Semantic Caching](../semantic-caching/) — "Cache by Meaning" 🟡
- **Goal:** skip repeat LLM calls.
- **Build:** Before calling the model, embed the query and check a cache of past queries; on a hit above a **similarity threshold**, return the cached answer. Track **hit rate** and cost saved.
- **Learn:** semantic cache hits on paraphrases; threshold tuning is critical.
- **Stretch:** show a false-positive when the threshold is too loose, and fix it.

## 🔹 [Model Routing](../model-routing/) — "Cheap/Expensive Router" 🟡
- **Goal:** spend big-model money only when needed.
- **Build:** Classify each query as easy/hard (small model or rules); route easy → cheap model, hard → strong model. Compare **cost & quality** vs always-big.
- **Learn:** most traffic is easy; route by difficulty.
- **Stretch:** turn it into a **cascade** — try cheap first, escalate on low confidence.

---

✅ **Part VI checkpoint:** your system is evaluated, observable, guarded, and cost-aware —
i.e. actually shippable.

➡️ Next: **[Part VII — The FDE Craft](./part7-fde.md)**.
