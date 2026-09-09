# 🤖 Part V Projects — Building Agents

Build a real agent one capability at a time. 🟢 conceptual · 🟡 needs code/API.

➡️ Concepts: [Agents](../agents/) · [Agentic Loop](../agentic-loop/) · [Architectures](../agents/architectures.md) · [Tools/MCP](../agents/tools.md) · [Memory](../agents/memory.md)

---

## 🔹 [Agents](../agents/) — "ReAct Calculator" 🟡
- **Goal:** the smallest real agent.
- **Build:** An agent with ONE tool (`calculator`). Prompt it in **ReAct** style (Thought → Action → Observation) to solve *"What is 23% of 1,540 plus 87?"* — it must call the tool, not do mental math.
- **Learn:** the reason → act → observe loop; tools beat mental arithmetic.
- **Stretch:** add a second tool (`get_time`) and a question needing both.

## 🔹 [Agentic Loop](../agentic-loop/) — "Add Stopping Conditions" 🟡
- **Goal:** make the loop safe.
- **Build:** Take the ReAct agent and add **max-steps**, a **cost/step counter**, and **loop detection** (bail if it repeats the same action twice).
- **Learn:** unbounded loops are bugs & bills; every agent needs exits.
- **Stretch:** log each iteration's thought/action/observation as a trace.

## 🔹 [Agent Architectures](../agents/architectures.md) — "Plan-and-Execute Researcher" 🟡
- **Goal:** plan before acting.
- **Build:** Given a research question, have the model **write a plan** (3–4 steps) first, then execute each with a search tool, then synthesize.
- **Learn:** planning beats greedy ReAct on multi-step tasks.
- **Stretch:** add a **reflection** step that critiques the final answer and revises once.

## 🔹 [Tools & Function Calling](../agents/tools.md) — "Two-Tool Agent (or Mini MCP)" 🟡
- **Goal:** clean tool design.
- **Build:** Define `search_web` and `get_weather` with clear schemas & descriptions; let the model choose. Handle a bad/hallucinated argument gracefully.
- **Learn:** descriptions are prompts; validate inputs; return concise results.
- **Stretch:** wrap one tool as a tiny **[MCP](../mcp/) server** and call it from an MCP client.

## 🔹 [Memory](../agents/memory.md) — "Chatbot That Remembers You" 🟡
- **Goal:** long-term memory via retrieval.
- **Build:** After each chat, **store** notable facts (embedded) in a vector store; on new messages, **retrieve** relevant memories into context. Test across "sessions."
- **Learn:** memory = write (what to save) + read (what to pull in).
- **Stretch:** add memory **expiry/versioning** so new facts override stale ones.

---

✅ **Part V checkpoint:** you can build a tool-using agent that plans, has memory, and stops
safely.

➡️ Next: **[Part VI — Production](./part6-production.md)** · or try the **[intermediate capstone](./capstones.md)**.
