# 🧹 Data Preparation for Fine-tuning

> **One-liner:** Fine-tuning is **90% data work**. The model can only be as good as the
> examples you show it. Quality, consistency, and coverage beat raw quantity every time.

```mermaid
flowchart LR
    C[Collect] --> F[Format] --> CL[Clean] --> SP[Split] --> T[Train] --> E[Evaluate]
    E -. gaps found .-> C
```

---

## 1️⃣ Collect

Sources of training pairs:

| Source | Notes |
|--------|-------|
| **Historical logs** | Real support tickets, past outputs — gold if you have them |
| **Human-written** | Experts craft ideal responses — high quality, slow |
| **Synthetic (LLM-generated)** | A strong model generates examples — fast, needs filtering |
| **Public datasets** | Bootstrap, then specialize |

> ⚖️ **Diversity matters:** cover the full range of inputs you'll see in production, including edge cases and the "don't do this" cases.

## 2️⃣ Format

Match the **chat template** the model expects. For instruction tuning, examples usually look like a conversation:

```json
{"messages": [
  {"role": "system",    "content": "You extract invoice fields as JSON."},
  {"role": "user",      "content": "Invoice #A-2231, total $4,200, due 2025-06-01"},
  {"role": "assistant", "content": "{\"id\": \"A-2231\", \"total\": 4200, \"due\": \"2025-06-01\"}"}
]}
```

```mermaid
flowchart LR
    RAW[Raw pairs] --> TMPL[Apply chat template<br/>system/user/assistant]
    TMPL --> TOK[Tokenize]
    TOK --> MASK[Mask the prompt —<br/>train only on the response]
```

- **Loss masking:** you typically compute loss **only on the assistant's tokens**, not the prompt — you want the model to learn the *response*, not to reproduce the question.
- **Be consistent:** same system prompt style, same output schema, everywhere.

## 3️⃣ Clean

```mermaid
flowchart TD
    D[Raw dataset] --> DUP[Remove duplicates]
    DUP --> BAD[Drop wrong / low-quality answers]
    BAD --> PII[Strip PII / secrets]
    PII --> BAL[Balance classes / lengths]
    BAL --> READY[Clean dataset]
```

- **Deduplicate** — repeats bias the model and inflate metrics.
- **Fix inconsistencies** — one output format, not five.
- **Remove leakage** — no test examples hiding in the training set.
- **Redact PII / secrets** — you're baking data into weights.

## 4️⃣ Split

```mermaid
flowchart LR
    ALL[All data] --> TR[Train ~80–90%]
    ALL --> VAL[Validation ~5–10%]
    ALL --> TEST[Test ~5–10%<br/>never seen during tuning]
```

- **Train:** what the model learns from.
- **Validation:** watch during training to catch **overfitting** (val loss rising while train loss falls).
- **Test:** the honest final grade — touch it once.

---

## 📏 How much data?

```mermaid
flowchart LR
    A[Format / style] -->|50–500| ok1[often enough]
    B[Narrow task] -->|500–5k| ok2[solid]
    C[Broad skill] -->|5k–50k+| ok3[needed]
```

- Simple format/tone changes: **dozens to a few hundred** clean examples can move the needle (esp. with LoRA).
- New skills/domains: **thousands**.
- **Always** start small, evaluate, and add data where you see failures — don't over-collect up front.

---

## 🎯 Quality checklist

- [ ] Every example is **correct** and in the **exact** target format.
- [ ] Coverage of real inputs **and** edge cases.
- [ ] Includes examples of **what to refuse / when to say "I don't know."**
- [ ] Deduplicated, no train/test leakage.
- [ ] PII/secrets removed.
- [ ] Consistent system prompt & schema across all rows.

---

## 🔁 It's a loop, not a step

```mermaid
flowchart LR
    T[Train] --> EV[Evaluate on test set]
    EV --> ERR[Inspect failures]
    ERR --> ADD[Add targeted examples<br/>for the failure mode]
    ADD --> T
```

The fastest path to a good fine-tune: train small → find where it fails → add examples that fix *exactly* that → repeat. This "error-driven data collection" beats guessing.

➡️ Back to [Fine-tuning overview](./README.md) · compare with [RAG](../rag/).
