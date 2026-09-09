# 🧷 Model Merging

> **One-liner:** Model merging combines **two or more fine-tuned models into one** — by
> mathematically blending their weights — **without any additional training**. You get a
> model with multiple skills, for basically the cost of some arithmetic.

```mermaid
flowchart LR
    A[Model A<br/>-good at code-] --> MERGE[Merge weights]
    B[Model B<br/>-good at chat-] --> MERGE
    MERGE --> C[Merged model<br/>-both skills-]
```

---

## 💡 Why it works (task vectors)

Fine-tuning moves weights in a direction — a **"task vector"** = (fine-tuned − base). You
can **add** task vectors to combine skills, or subtract to remove behaviors.

```mermaid
flowchart LR
    BASE[Base model] --> V1["+ task vector A -code-"]
    BASE --> V2["+ task vector B -math-"]
    V1 & V2 --> SUM["Base + A + B = multi-skill model"]
```

- ✅ **No training, no GPUs for gradients** — just weight math (minutes on CPU/one GPU).
- ⚠️ Models must be **compatible** — same architecture & size (usually same base).

---

## 🧰 Merging methods

| Method | Idea |
|--------|------|
| **Linear / Model Soup** | Average the weights of several models |
| **SLERP** | Spherical interpolation between **two** models (smoother than linear) |
| **Task Arithmetic** | Add/subtract task vectors to add/remove skills |
| **TIES** | Trim small changes, resolve sign conflicts, then merge |
| **DARE** | Randomly drop & rescale deltas before merging (reduces interference) |
| **Franken-merging / passthrough** | Stack layers from different models (can change size) |

```mermaid
flowchart TD
    M{How many + goal?} --> TWO[Two models] --> SLERP[SLERP]
    M --> MANY[Many models] --> SOUP[Model Soup]
    M --> SKILLS[Combine distinct skills] --> TIES[TIES / DARE / Task Arithmetic]
```

---

## ⚖️ Trade-offs

- ✅ Cheap, fast, no training data needed; combine community fine-tunes.
- ✅ Can improve robustness (souping) or stack capabilities.
- ⚠️ **Interference** — skills can degrade each other; TIES/DARE fight this.
- ⚠️ Results are **empirical** — often needs trial-and-error + [evaluation](../evaluation/).
- ⚠️ Only same-family models; can't merge fundamentally different architectures.

---

## 🆚 Merging vs alternatives

```mermaid
flowchart LR
    MERGE["Merging:<br/>blend weights, no training"]
    MOE["MoE:<br/>route to experts at runtime"]
    FT["Fine-tuning:<br/>train on combined data"]
```

- **[Fine-tuning](../fine-tuning/)** on combined data is stronger but needs data + compute.
- **[MoE](../mixture-of-experts/)** keeps experts separate and routes at inference.
- **Merging** is the cheapest way to get "several skills in one model."

---

## 🗺️ Where model merging is used

- **Combining community fine-tunes** into one capable open model (very common on model hubs).
- **Adding a skill** to a model without retraining.
- **Model soups** for a more robust, higher-average model.
- Tooling: **mergekit** is the popular toolkit.

➡️ Related: **[fine-tuning](../fine-tuning/)** · **[mixture-of-experts](../mixture-of-experts/)** · **[LoRA](../fine-tuning/methods.md)** (adapters can also be merged).
