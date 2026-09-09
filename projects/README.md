# 🛠️ Projects — Learn by Building

Reading is not enough — **build one small thing per concept**. This folder has a
bite-sized project for *every buzzword* in the atlas, plus three big **[capstones](./capstones.md)**
that force you to combine everything.

```mermaid
flowchart LR
    READ[Read a concept] --> BUILD[Build its mini-project] --> UNDERSTAND[Actually get it] --> CAP[Capstones]
```

---

## 🧭 How to use this

1. Finish a chapter in the **[learning path](../README.md#-the-path-to-forward-deployed-engineer)**.
2. Do that concept's **mini-project** below (an hour or two each — keep it scrappy).
3. After each Part, attempt the matching **[capstone](./capstones.md)** slice.

**Difficulty legend:** 🟢 quick & conceptual · 🟡 needs code/an API key · 🔴 heavier (GPU/tooling).

> 🧱 **Toolbox (suggested, not required):** Python, an LLM API (or a local model),
> `tiktoken`, `sentence-transformers`, a vector store (`chromadb`/`faiss`), and a notebook.
> Every project can be done scrappy — a script beats a framework here.

---

## 📚 Project index (by Part)

| Part | Projects file | Covers |
|------|---------------|--------|
| **I — Foundations** | [part1-foundations.md](./part1-foundations.md) | LLMs · transformers · tokenization · embeddings · vector DBs |
| **II — Talking to Models** | [part2-talking-to-models.md](./part2-talking-to-models.md) | prompting · context · structured output · long context |
| **III — RAG** | [part3-rag.md](./part3-rag.md) | RAG · chunking · retrieval · reranking · eval · architectures · KGs |
| **IV — Customizing** | [part4-customizing.md](./part4-customizing.md) | fine-tuning · LoRA · alignment · data · distillation · quantization · merging |
| **V — Agents** | [part5-agents.md](./part5-agents.md) | agents · loop · architectures · tools/MCP · memory |
| **VI — Production** | [part6-production.md](./part6-production.md) | eval · observability · guardrails · hallucination · red-team · inference · caching · routing |
| **VII — The FDE Craft** | [part7-fde.md](./part7-fde.md) | FDE workflow · system design |
| **Appendix — Frontier** | [appendix-frontier.md](./appendix-frontier.md) | reasoning · MoE · diffusion · multimodal · constitutional AI · synthetic data · interp · world models · federated |
| **🎓 Capstones** | [capstones.md](./capstones.md) | 3 end-to-end projects with rubrics |

---

## 🏁 The one rule

> **Ship something you can show.** A working scrappy demo beats a perfect plan. Every project
> below ends with a thing you can run and screenshot — that's the [FDE](../fde/) mindset.

➡️ Start: **[Part I projects](./part1-foundations.md)** · or jump to the **[capstones](./capstones.md)**.
