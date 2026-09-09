# 🧑‍🔬 Mixture of Experts (MoE)

> **One-liner:** Instead of one giant dense network where **every** parameter runs for
> **every** token, an MoE has many smaller "expert" sub-networks and a **router** that
> activates only a **few** per token. Huge total capacity, but only a fraction runs each time.

```mermaid
flowchart TD
    T[Token] --> R{Router / gate}
    R -->|top-2| E2[Expert 2 ✅]
    R -->|top-2| E5[Expert 5 ✅]
    R -.-> E1[Expert 1]
    R -.-> E3[Expert 3]
    R -.-> E4[Expert 4]
    E2 & E5 --> C[Combine] --> O[Output]
```

---

## 🧠 Dense vs sparse (the key contrast)

```mermaid
flowchart LR
    subgraph Dense["Dense model"]
        D[Every param runs<br/>for every token]
    end
    subgraph MoE["MoE model"]
        M["Only k of N experts run<br/>per token -sparse-"]
    end
```

- **Total parameters**: huge (all experts exist).
- **Active parameters**: small (only the routed experts run).
- Result: **the knowledge capacity of a big model at the compute cost of a small one.**

---

## 🔀 The router is everything

A small learned **gating network** decides which experts handle each token. Getting this
right is the hard part.

| Challenge | Why |
|-----------|-----|
| **Load balancing** | Don't let a few experts get all the traffic (others go untrained). Fixed with auxiliary "load-balancing" losses. |
| **Routing instability** | Small changes can flip routing; needs care to train. |
| **Which experts?** | Usually **top-1 or top-2** of N experts per token. |

```mermaid
flowchart LR
    R[Router] --> B{Balanced?}
    B -- no --> BAD[Some experts idle<br/>→ wasted capacity]
    B -- yes --> GOOD[All experts learn]
```

---

## ⚖️ Trade-offs

| ✅ Pros | ❌ Cons |
|--------|--------|
| More capacity per unit of compute | **All experts must fit in memory** (big VRAM) |
| Faster training & inference vs equally-"smart" dense model | Routing adds complexity & instability |
| Experts can specialize | Harder to fine-tune / deploy |
| Scales to very large total params | Uneven load can waste capacity |

---

## 🗺️ Where MoE is used

- Many **frontier LLMs** use MoE to reach huge scale affordably.
- **Efficient inference** when you want big-model quality without big-model per-token cost.
- Any setting where **memory is available but compute-per-token must stay low**.

> 💡 **Mental model:** a hospital with many specialists. A receptionist (router) sends each
> patient (token) to the right 1–2 doctors (experts) instead of making every patient see
> every doctor.

➡️ Related scaling ideas → **[large-language-models](../large-language-models/)** · efficiency → **[inference-optimization](../inference-optimization/)**.
