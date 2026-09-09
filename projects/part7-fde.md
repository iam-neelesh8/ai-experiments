# 🧑‍💼 Part VII Projects — The FDE Craft

These are less about code, more about **judgment and communication** — the FDE difference.

➡️ Concepts: [The FDE Role](../fde/) · [LLM System Design](../llm-system-design/)

---

## 🔹 [The FDE Workflow](../fde/) — "Run a Real Discovery" 🟢
- **Goal:** practice finding the *real* problem.
- **Build:** Pick a friend/colleague with a repetitive task. Interview them for 20 minutes using: *What's the task? What's painful? What does success look like? What data exists? What's the current workaround?* Write a **half-page discovery note** ending with the **smallest valuable slice** (the wedge).
- **Learn:** the brief is rarely the real problem; scope beats scope-creep.
- **Stretch:** turn the note into a one-paragraph "what I'd build first and why."

## 🔹 [The FDE Workflow](../fde/) — "POC in a Day" 🟡
- **Goal:** ship a scrappy thing fast.
- **Build:** For that discovery, build the **simplest** POC that shows value — prefer prompt → RAG → agent order. Time-box it. Demo it and capture their reaction.
- **Learn:** a working scrappy demo > a perfect plan; get feedback early.
- **Stretch:** write the "how I'd measure if this is working" plan (2–3 metrics).

## 🔹 [LLM System Design](../llm-system-design/) — "Design Doc" 🟢
- **Goal:** turn a use case into an architecture.
- **Build:** For a chosen use case, write a 1-page design: the **decision-tree choice** (prompt/RAG/agent/fine-tune), an **architecture diagram** (mermaid), and the **cross-cutting layers** (security, guardrails, evals, observability, cost).
- **Learn:** systems = bricks + cross-cutting layers; justify each choice.
- **Stretch:** add a "what breaks first at 100× scale" section.

## 🔹 [LLM System Design](../llm-system-design/) — "Failure-Mode Pre-Mortem" 🟢
- **Goal:** design for failure up front.
- **Build:** List 8 ways your design could fail (bad retrieval, injection, cost blowup, hallucination, tool error…) and the **mitigation + fallback** for each.
- **Learn:** reliability is designed in, not bolted on.
- **Stretch:** mark which failures need **human-in-the-loop**.

---

✅ **Part VII checkpoint:** you can take a vague problem through discovery → scoped POC →
design doc → measurement plan — the actual FDE job.

➡️ Finish strong: the **[🎓 Capstones](./capstones.md)** · optional deep cuts: **[Appendix](./appendix-frontier.md)**.
