# 🎛️ Part IV Projects — Customizing Models

Several of these are 🔴 (need a GPU / Colab). Where you lack hardware, the 🟢 conceptual
versions still build the judgment that matters most.

➡️ Concepts: [Fine-tuning](../fine-tuning/) · [Methods](../fine-tuning/methods.md) · [Alignment](../fine-tuning/alignment.md) · [Data Prep](../fine-tuning/data-preparation.md) · [Distillation](../distillation/) · [Quantization](../quantization/) · [Model Merging](../model-merging/)

---

## 🔹 [Fine-tuning](../fine-tuning/) — "RAG vs Fine-tune Memo" 🟢
- **Goal:** build the most valuable instinct: *which lever?*
- **Build:** Write a one-page memo deciding **RAG / fine-tune / prompt / both** for 5 scenarios (e.g. "answer from our changing docs", "always output our JSON format", "sound like our brand").
- **Learn:** knowledge → RAG; behavior/format → fine-tune; try prompting first.
- **Stretch:** add cost & maintenance trade-offs to each recommendation.

## 🔹 [Methods](../fine-tuning/methods.md) — "LoRA a Personality" 🔴
- **Goal:** run a real (tiny) fine-tune.
- **Build:** On free Colab, **QLoRA** fine-tune a small open model on ~100 examples of a style (e.g. "reply like a pirate" or a fixed JSON format). Compare before/after.
- **Learn:** LoRA/QLoRA make fine-tuning cheap; adapters are tiny.
- **Stretch:** vary rank `r` and epochs; watch for overfitting.

## 🔹 [Alignment](../fine-tuning/alignment.md) — "Preference Pairs for DPO" 🟢🔴
- **Goal:** understand preference tuning.
- **Build (🟢):** For 20 prompts, hand-write a **chosen** and **rejected** answer encoding a preference (concise > rambling). **Build (🔴):** run DPO on them.
- **Learn:** DPO learns from (chosen, rejected) pairs — no reward model.
- **Stretch:** describe how the same data would flow through full RLHF instead.

## 🔹 [Data Preparation](../fine-tuning/data-preparation.md) — "Clean 50-Example Dataset" 🟡
- **Goal:** experience that data is 90% of the work.
- **Build:** Assemble 50 (prompt → ideal answer) pairs in chat format; **dedupe**, fix inconsistent formats, split train/val/test.
- **Learn:** quality > quantity; consistent format; no leakage.
- **Stretch:** intentionally add 5 bad examples, then write a filter that catches them.

## 🔹 [Distillation](../distillation/) — "Teacher → Student Labels" 🟡
- **Goal:** small model imitates a big one.
- **Build:** Use a strong model to **label** 300 texts (e.g. sentiment); train a tiny classifier (logistic regression / small model) on those labels. Measure the gap to the teacher.
- **Learn:** data distillation — cheap capable small models.
- **Stretch:** compare student trained on teacher labels vs on 50 human labels.

## 🔹 [Quantization](../quantization/) — "4-bit Speed/Quality Test" 🔴
- **Goal:** see the size/speed/quality trade-off.
- **Build:** Load a model in FP16 vs **4-bit** (bitsandbytes / GGUF). Compare **memory, tokens/sec, and answers** on 10 prompts.
- **Learn:** 4-bit ≈ 4× smaller with small quality loss.
- **Stretch:** push to a lower bit-width until quality visibly breaks.

## 🔹 [Model Merging](../model-merging/) — "Merge Two Models" 🔴
- **Goal:** combine skills with no training.
- **Build:** With **mergekit**, SLERP-merge two same-base fine-tunes (e.g. a code model + a chat model); test both skills in the merged model.
- **Learn:** task vectors add; merging is arithmetic, not training.
- **Stretch:** try TIES vs linear and compare interference.

---

✅ **Part IV checkpoint:** you can pick RAG vs fine-tune correctly and run a LoRA fine-tune
end to end.

➡️ Next: **[Part V — Agents](./part5-agents.md)**.
