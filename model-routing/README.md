# 🚦 Model Routing & Cascades

> **One-liner:** Don't send every request to your biggest, most expensive model. A
> **router** sends each query to the *cheapest model that can handle it* — big savings on
> cost and latency with little quality loss.

```mermaid
flowchart TD
    Q([Query]) --> R{Router}
    R -->|simple| SMALL[Small / cheap model]
    R -->|hard| BIG[Large / smart model]
    R -->|reasoning-heavy| REASON[Reasoning model]
    SMALL & BIG & REASON --> A([Answer])
```

---

## 💡 The core idea

Most real traffic is easy. Reserving a frontier model for *every* request wastes money.
Route by **difficulty**.

```mermaid
flowchart LR
    E[Easy queries<br/>~most traffic] --> CHEAP[Cheap model 💸]
    H[Hard queries<br/>~the minority] --> STRONG[Strong model 🧠]
```

---

## 🧭 Routing strategies

| Strategy | How it decides |
|----------|----------------|
| **Classifier router** | A small model predicts difficulty/category → picks a model |
| **Cascade** | Try cheap model first; escalate only if confidence is low |
| **Rule-based** | Route by keywords, length, task type, or user tier |
| **Semantic** | Route by query embedding / similarity to known buckets |
| **Capability** | Send code → code model, images → [multimodal](../multimodal/), etc. |

### Cascade pattern (try cheap, escalate)

```mermaid
flowchart LR
    Q([Query]) --> C[Cheap model]
    C --> J{Confident /<br/>good enough?}
    J -- yes --> DONE[✅ Return]
    J -- no --> BIG[Escalate to big model]
    BIG --> DONE
```

---

## ⚖️ Trade-offs

```mermaid
flowchart LR
    R[Routing] --> P1[✅ Lower cost]
    R --> P2[✅ Lower latency for easy queries]
    R --> C1[❌ Router can misroute]
    R --> C2[❌ Extra system complexity]
    R --> C3[❌ Cascades add latency on escalation]
```

- The router itself must be **cheap and accurate** — a bad router negates the savings.
- Cascades trade a little **extra latency** on hard queries for big savings on easy ones.

---

## 🗺️ Where routing is used

- **Cost optimization** at scale (the headline reason).
- **Latency-sensitive** apps (fast path for common queries).
- **Multi-capability** systems (route to specialized/[reasoning](../reasoning-models/) models).
- **AI gateways** and LLM proxies.

> 💡 Related idea: **model cascades** and **speculative decoding** ([inference-optimization](../inference-optimization/)) both exploit "use a small model when you can, the big one only when you must."

➡️ Related: **[inference-optimization](../inference-optimization/)** · **[reasoning-models](../reasoning-models/)** · **[evaluation](../evaluation/)** (to tune the router).
