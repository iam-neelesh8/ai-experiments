# 💬 Part II Projects — Talking to Models

🟢 conceptual · 🟡 needs code/API · 🔴 heavier.

➡️ Concepts: [Prompt Engineering](../prompt-engineering/) · [Context Engineering](../context-engineering/) · [Structured Output](../structured-output/) · [Long Context](../long-context/)

---

## 🔹 [Prompt Engineering](../prompt-engineering/) — "Prompt A/B Lab" 🟡
- **Goal:** measure that prompt technique matters.
- **Build:** Pick 10 tricky word-problems. Answer each three ways — **zero-shot**, **few-shot**, **chain-of-thought** ("think step by step") — and score accuracy for each.
- **Learn:** CoT lifts reasoning; few-shot pins format.
- **Stretch:** add **self-consistency** (sample 5 CoT paths, majority vote) and compare.

## 🔹 [Context Engineering](../context-engineering/) — "Budget-Aware Chatbot" 🟡
- **Goal:** keep a long chat inside the context window.
- **Build:** A chat loop that tracks token count; when it nears a limit, **summarize the oldest turns** into a running summary and drop the raw turns.
- **Learn:** the window is finite; summarize/offload to survive long chats.
- **Stretch:** compare "sliding window" vs "summary" on a 50-turn conversation — which keeps facts better?

## 🔹 [Structured Output](../structured-output/) — "Resume → JSON" 🟡
- **Goal:** get output your code can trust.
- **Build:** Extract `{name, email, skills[], years_experience}` from messy resume text. Define the schema (e.g. a Pydantic model / JSON Schema) and **validate** every response; retry on failure.
- **Learn:** schema/function-calling gives parseable output; always validate.
- **Stretch:** use a constrained-decoding lib (e.g. Outlines) so invalid JSON is *impossible*, and count how many retries you save.

## 🔹 [Long Context](../long-context/) — "Needle in a Haystack" 🟡
- **Goal:** test where a model actually reads.
- **Build:** Hide a sentence (*"The launch code is 4471."*) at the start, middle, and end of a long document. Ask for it at each position and record accuracy.
- **Learn:** the "lost in the middle" effect; put key info at the edges.
- **Stretch:** repeat at 2k / 8k / 32k tokens and chart accuracy vs length & position.

---

✅ **Part II checkpoint:** you can reliably get correct, well-formatted answers out of a raw
model — and you know its context limits firsthand.

➡️ Next: **[Part III — RAG](./part3-rag.md)**.
