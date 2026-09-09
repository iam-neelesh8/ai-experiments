# 🧱 Part I Projects — Foundations

One small build per concept. 🟢 conceptual · 🟡 needs code/API · 🔴 heavier.

➡️ Concepts: [LLMs](../large-language-models/) · [Transformers](../transformers/) · [Tokenization](../tokenization/) · [Embeddings](../embeddings/) · [Vector DBs](../vector-databases/)

---

## 🔹 [Large Language Models](../large-language-models/) — "Next-Token Peek" 🟡
- **Goal:** *feel* that an LLM is a next-token predictor.
- **Build:** Call a model with `logprobs` enabled on the prompt *"The capital of France is"*. Print the **top-5 next tokens and their probabilities**. Then let it generate 10 tokens one at a time, appending each.
- **Learn:** generation = a probability loop; why temperature changes output.
- **Stretch:** plot how the top token's probability changes across a sentence.

## 🔹 [Transformers](../transformers/) — "Attention Explorer" 🔴
- **Goal:** see self-attention resolve meaning.
- **Build:** Load a small open model (e.g. via `transformers` + `bertviz`). Feed *"The animal didn't cross the street because it was tired."* and visualize which word **"it"** attends to.
- **Learn:** attention links related tokens; multiple heads capture different relations.
- **Stretch:** change "tired" → "wide" and watch the attention target shift to "street".

## 🔹 [Tokenization](../tokenization/) — "Token Counter & Cost Estimator" 🟡
- **Goal:** stop guessing token counts.
- **Build:** Use `tiktoken` to count tokens in a paragraph. Compare the **same sentence in English vs another language**. Multiply by a model's price to estimate cost.
- **Learn:** tokens ≠ words; some languages cost more; cost/context are token-based.
- **Stretch:** find a word that splits into 4+ tokens; count the tokens in "strawberry" and reason about the "how many r's" problem.

## 🔹 [Embeddings](../embeddings/) — "Semantic Search in 30 Lines" 🟡
- **Goal:** search by meaning, not keywords.
- **Build:** Embed ~20 sentences with `sentence-transformers`. For a query, compute **cosine similarity** to all and print the top 3.
- **Learn:** similar meanings → nearby vectors; the same-model rule for query & docs.
- **Stretch:** show that *"How do I get a refund?"* matches *"What's the return policy?"* despite zero shared keywords.

## 🔹 [Vector Databases](../vector-databases/) — "Tiny Vector DB" 🟡
- **Goal:** move from a loop to a real index.
- **Build:** Load those embeddings into **Chroma** or **FAISS**, attach metadata (e.g. `topic`), and run a top-k query **with a metadata filter**.
- **Learn:** ANN search + metadata filtering; why this scales past a Python loop.
- **Stretch:** time brute-force cosine vs the index as you grow to 10k vectors.

---

✅ **Part I checkpoint:** you can explain, with your own running code, how text → tokens →
embeddings → nearest-neighbor search.

➡️ Next: **[Part II — Talking to Models](./part2-talking-to-models.md)** · or the **[capstones](./capstones.md)**.
