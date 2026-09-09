# 🗣️ Large Language Models (LLMs)

> **One-liner:** An LLM is a neural network trained on massive text to do one deceptively
> simple thing — **predict the next token** — and from that emerges the ability to write,
> reason, code, and converse.

```mermaid
flowchart LR
    IN["'The capital of France is'"] --> M[(LLM)]
    M --> P["next-token probabilities:<br/>Paris 0.92 · London 0.01 ..."]
    P --> OUT["'Paris'"]
    OUT -. feed back .-> IN
```

Generation is just this loop: predict a token, append it, predict again (**autoregression**).

---

## 🏭 How an LLM is made

```mermaid
flowchart LR
    D[Trillions of tokens<br/>web, books, code] --> PRE[Pretraining<br/>learn to predict next token]
    PRE --> BASE[Base model<br/>raw knowledge]
    BASE --> SFT[Instruction tuning]
    SFT --> AL[Alignment -RLHF/DPO-]
    AL --> CHAT[Assistant]
```

- **[Pretraining](../fine-tuning/)** — the expensive part: learn language & world knowledge from raw text.
- **Instruction tuning + [alignment](../fine-tuning/alignment.md)** — turn a text-predictor into a helpful assistant.
- This is the **[foundation model](#-foundation-models) → fine-tune** paradigm (transfer learning).

---

## 🧠 Key concepts & the jargon

| Term | Meaning |
|------|---------|
| **Parameters** | The learned weights (billions). Rough proxy for capacity. |
| **[Tokens](../tokenization/)** | The units the model reads/writes. |
| **[Context window](../context-engineering/)** | Max tokens it can consider at once. |
| **[Transformer](../transformers/)** | The neural architecture underneath. |
| **Temperature / sampling** | How random the output is. |
| **Emergent abilities** | Skills that appear only at scale (reasoning, in-context learning). |
| **In-context learning** | Learning a task from examples in the prompt — no weight change. |
| **Knowledge cutoff** | Training data has an end date → stale facts (fix with [RAG](../rag/)). |
| **Hallucination** | Confident, wrong output → see [hallucination](../hallucination/). |

---

## 🌱 In-context learning (why prompting works)

```mermaid
flowchart LR
    P["Prompt with 3 examples<br/>of a NEW task"] --> M[(LLM)]
    M --> A["Does the task<br/>-weights unchanged-"]
```

The model "learns" the task from the prompt at inference time. This is what makes
[prompt engineering](../prompt-engineering/) and [few-shot](../prompt-engineering/) possible.

---

## 📈 Scaling laws & emergence

More data + more parameters + more compute → predictably lower loss, and at certain
scales, **new abilities emerge** (multi-step reasoning, tool use, in-context learning).

```mermaid
flowchart LR
    S[↑ Scale<br/>data · params · compute] --> L[↓ Loss -smooth-]
    S --> E[✨ Emergent skills -sudden-]
```

---

## 🧱 Foundation models

A **foundation model** is a large model pretrained on broad data that you **adapt** to many
downstream tasks (via prompting, [RAG](../rag/), or [fine-tuning](../fine-tuning/)) instead
of training from scratch. LLMs are the text example; there are also vision and
[multimodal](../multimodal/) foundation models.

---

## 🗺️ The families you'll hear about

- **Decoder-only** (GPT-style) — text generation, chat. The dominant LLM design.
- **Encoder-only** (BERT-style) — embeddings, classification (see [transformers](../transformers/)).
- **Encoder-decoder** (T5-style) — translation, summarization.
- **[Reasoning models](../reasoning-models/)** — spend extra compute "thinking" before answering.
- **[Mixture-of-Experts](../mixture-of-experts/)** — huge models that activate only part per token.

➡️ Dive into the engine → **[transformers](../transformers/)**.
