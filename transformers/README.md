# ⚙️ Transformers

> **One-liner:** The **transformer** is the neural network architecture behind virtually
> every modern LLM. Its superpower is **self-attention** — letting every token look at
> every other token to figure out meaning in context. ("Attention Is All You Need," 2017.)

```mermaid
flowchart LR
    T[Tokens] --> E[Embeddings + positions]
    E --> B1[Transformer block] --> B2[Transformer block] --> B3[... ×N]
    B3 --> H[Output head] --> P[Next-token probs]
```

---

## 👀 Self-attention — the core idea

For each token, attention asks: *"which other tokens should I pay attention to?"* — and
mixes their information accordingly. That's how the model resolves what "it" refers to.

```mermaid
flowchart TD
    W["'The animal didn't cross the street because IT was tired'"]
    W --> A{Attention for 'it'}
    A -->|high weight| ANIMAL[animal ✅]
    A -->|low weight| STREET[street]
```

Mechanically, each token produces a **Query, Key, and Value**; attention scores every
Query against every Key, then blends the Values:

```mermaid
flowchart LR
    X[Token] --> Q[Query]
    X --> K[Key]
    X --> V[Value]
    Q --> S["score = Q·K → softmax"]
    K --> S
    S --> O["weighted sum of V"]
    V --> O
```

- **Multi-head attention** runs several attention "heads" in parallel, each learning different relationships (syntax, coreference, etc.).

---

## 🧱 Inside a transformer block

```mermaid
flowchart TD
    IN[Input] --> ATT[Multi-head self-attention]
    ATT --> R1[Add & normalize]
    R1 --> FF[Feed-forward network]
    FF --> R2[Add & normalize]
    R2 --> OUT[Output]
    IN -. residual .-> R1
    R1 -. residual .-> R2
```

Stack N of these blocks (dozens to over a hundred) and you have the model. **Residual
connections** and **normalization** keep very deep stacks trainable.

---

## 📍 Positional encoding

Attention alone has no sense of order. **Positional encodings** inject "where each token
sits" so the model knows sequence order.

| Type | Note |
|------|------|
| Sinusoidal / learned | Original approaches |
| **RoPE** (rotary) | Common in modern LLMs; helps with longer contexts |
| ALiBi | Biases attention by distance |

---

## 🧭 Three transformer shapes

```mermaid
flowchart LR
    DEC["Decoder-only<br/>-GPT-<br/>generation / chat"]
    ENC["Encoder-only<br/>-BERT-<br/>embeddings / classify"]
    ED["Encoder-decoder<br/>-T5-<br/>translate / summarize"]
```

Modern LLMs are overwhelmingly **decoder-only**: they generate text left-to-right, each
token attending only to previous tokens (**causal / masked attention**).

---

## ⏳ The quadratic problem

Self-attention compares every token to every other → cost grows with the **square** of
sequence length. This is why long contexts are expensive and why there's constant work on
efficient attention.

```mermaid
flowchart LR
    N[Sequence length n] --> C["Attention cost ∝ n²"]
    C --> FIX[FlashAttention, sparse/linear attention,<br/>see inference-optimization]
```

➡️ See how this gets faster in practice → **[inference-optimization](../inference-optimization/)**.
Or how tokens become vectors → **[embeddings](../embeddings/)**.
