# 🔭 Appendix Projects — Frontier & Niche

Optional deep cuts. Many are 🟢 conceptual — the point is intuition, not a production build.

➡️ Concepts: [Reasoning Models](../reasoning-models/) · [MoE](../mixture-of-experts/) · [Diffusion](../diffusion-models/) · [Multimodal](../multimodal/) · [Constitutional AI](../constitutional-ai/) · [Synthetic Data](../synthetic-data/) · [Mech Interp](../mechanistic-interpretability/) · [World Models](../world-models/) · [Federated Learning](../federated-learning/)

---

## 🔹 [Reasoning Models](../reasoning-models/) — "Think vs Blurt" 🟡
- **Build:** On 10 hard math/logic problems, compare a standard model answering directly vs a reasoning model (or forced long CoT). Track accuracy **and** tokens/cost.
- **Learn:** test-time compute buys accuracy — at a price. Route accordingly.

## 🔹 [Mixture of Experts](../mixture-of-experts/) — "Manual Router" 🟢
- **Build:** Simulate the idea: write a small **router** that sends a query to one of 3 "expert" prompts (code / math / writing) and only runs that one.
- **Learn:** sparse activation — run the right expert, not all of them.

## 🔹 [Diffusion Models](../diffusion-models/) — "Guidance Sweep" 🟡
- **Build:** With any text-to-image API/tool, generate one prompt at **low vs high guidance scale** and **few vs many steps**. Compare adherence & quality.
- **Learn:** steps ↑ quality/latency; guidance ↑ prompt-adherence vs creativity.

## 🔹 [Multimodal](../multimodal/) — "Chat with a Chart" 🟡
- **Build:** Give a VLM a screenshot of a chart/table and ask 5 questions. Note where it misreads.
- **Learn:** images cost tokens; grounding & chart-reading are real failure modes.

## 🔹 [Constitutional AI](../constitutional-ai/) — "Self-Critique vs a Constitution" 🟡
- **Build:** Write 4 principles. Have the model answer a spicy prompt, then **critique its own answer against the principles** and revise. Compare v1 vs v2.
- **Learn:** principle-guided self-critique = scalable alignment (RLAIF's core move).

## 🔹 [Synthetic Data](../synthetic-data/) — "Generate + Filter 100" 🟡
- **Build:** Generate 100 synthetic examples for a task, then **filter** with rules + an LLM judge. Report how many survived and why.
- **Learn:** generation is easy; **verification** is the real work.

## 🔹 [Mechanistic Interpretability](../mechanistic-interpretability/) — "Logit Lens Peek" 🔴
- **Build:** On a small open model, read the **top predicted token at each layer** for a simple prompt (logit lens) and watch the answer "form."
- **Learn:** computation is progressive across layers; a taste of looking inside.

## 🔹 [World Models](../world-models/) — "Gridworld Predictor" 🟢🟡
- **Build:** In a tiny gridworld, learn/hand-code a next-state predictor and use it to **plan** a path by imagining moves before taking them.
- **Learn:** a world model lets you plan without acting for real.

## 🔹 [Federated Learning](../federated-learning/) — "Simulate FedAvg" 🟡
- **Build:** Split a dataset across 2 "clients," train a simple model locally on each, then **average the weights** into a global model. Compare to centralized training.
- **Learn:** learn from distributed data without moving it (FedAvg).

---

➡️ Back to the **[projects index](./README.md)** · the **[capstones](./capstones.md)** · the **[learning path](../README.md#-the-path-to-forward-deployed-engineer)**.
