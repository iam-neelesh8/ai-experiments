# 🔐 Federated Learning & Privacy-Preserving AI

> **One-liner:** Federated learning trains a shared model across many devices/organizations
> **without the raw data ever leaving its source**. The data stays home; only model updates
> travel. It's how you learn from private data you're not allowed to centralize.

```mermaid
flowchart TD
    S[(Central server<br/>global model)] -->|send model| D1[📱 Device 1]
    S -->|send model| D2[📱 Device 2]
    S -->|send model| D3[🏥 Hospital]
    D1 -->|send *updates* only| S
    D2 -->|send *updates* only| S
    D3 -->|send *updates* only| S
    S --> AGG[Aggregate → new global model]
```

---

## 🔁 The training round

```mermaid
flowchart LR
    A[1. Server sends<br/>current model] --> B[2. Each client trains<br/>on its LOCAL data]
    B --> C[3. Clients send back<br/>weight updates -not data-]
    C --> D[4. Server averages updates<br/>-FedAvg-]
    D --> A
```

The classic algorithm is **FedAvg** (Federated Averaging): average clients' updates, weighted
by data size. Raw data **never** moves.

---

## 🛡️ Why it matters: privacy

```mermaid
flowchart LR
    CENTRAL["Centralized:<br/>copy all data to one place"] --> RISK[❌ Privacy & compliance risk]
    FED["Federated:<br/>data stays local"] --> SAFE[✅ Learn without collecting raw data]
```

Enables learning from data that **can't** be pooled — medical records, phone keyboards,
across companies/regulatory borders.

---

## 🔒 Layering on stronger guarantees

Updates alone can still leak information, so federated learning is often combined with:

| Technique | Protects by |
|-----------|-------------|
| **Differential privacy** | Adding calibrated noise so no single record is identifiable |
| **Secure aggregation** | Server only sees the *sum* of updates, not any individual's |
| **Homomorphic encryption** | Compute on encrypted updates |

---

## ⚖️ Challenges

```mermaid
flowchart TD
    C[Challenges] --> NIID["Non-IID data<br/>-each client's data differs-"]
    C --> COMM["Communication cost<br/>-many rounds, big models-"]
    C --> HET["Device heterogeneity<br/>-phones vs servers-"]
    C --> SEC["Poisoning attacks<br/>-malicious clients-"]
```

- Clients have **different, skewed** data → harder to converge.
- **Communication** is a bottleneck for large models.
- **Security** — malicious clients can try to poison the global model.

---

## 🗺️ Where it's used

- **Mobile keyboards** (next-word prediction learned on-device).
- **Healthcare** — train across hospitals without sharing patient data.
- **Finance / cross-org** collaboration under privacy rules.
- **Edge / IoT** learning.

> 💡 For LLMs specifically, full federated *pretraining* is rare (too big), but federated /
> privacy-preserving **[fine-tuning](../fine-tuning/)** on sensitive data is an active area.

➡️ Related: **[fine-tuning](../fine-tuning/)** · privacy themes in **[guardrails](../guardrails/)** · **[synthetic-data](../synthetic-data/)** (another privacy workaround).
