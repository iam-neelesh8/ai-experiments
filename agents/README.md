# 🤖 Agents

> **One-liner:** An **agent** is an LLM that doesn't just answer — it **decides what to do,
> uses tools, observes the results, and repeats** until a goal is met. The LLM becomes the
> *reasoning engine* driving a loop, not a one-shot text generator.

```mermaid
flowchart LR
    G([Goal]) --> LLM[(LLM = brain)]
    LLM -->|decides action| T[Tools / APIs]
    T -->|results| LLM
    LLM --> R([Goal achieved])
```

---

## 🧠 Anatomy of an agent

```mermaid
flowchart TD
    subgraph Agent
        BRAIN[(LLM<br/>reasoning + planning)]
        MEM[Memory<br/>short & long term]
        TOOLS[Tools<br/>search, code, APIs]
        PLAN[Planning<br/>decompose & sequence]
    end
    G([Goal]) --> BRAIN
    BRAIN <--> MEM
    BRAIN <--> TOOLS
    BRAIN <--> PLAN
    BRAIN --> OUT([Actions / answer])
```

The four building blocks:
1. **Brain** — the LLM that reasons and chooses actions.
2. **[Tools](./tools.md)** — how it acts on the world (search, code exec, DB, APIs).
3. **[Memory](./memory.md)** — what it remembers within and across tasks.
4. **Planning** — breaking a goal into steps (and re-planning when things go wrong).

---

## 🔁 The core idea: the loop

Everything an agent does runs on the **[agentic loop](../agentic-loop/)** — reason → act → observe:

```mermaid
flowchart LR
    R[🧠 Reason<br/>what next?] --> A[🛠️ Act<br/>call a tool] --> O[👀 Observe<br/>read result] --> D{Done?}
    D -- no --> R
    D -- yes --> F([Final answer])
```

This loop is what separates an **agent** from a plain **[RAG](../rag/)** or chatbot call: it can take *many* steps, adapt, and recover from errors. (Full detail: **[agentic-loop](../agentic-loop/)**.)

---

## 🧩 Go deeper

| Topic | What's inside |
|-------|---------------|
| **[Architectures](./architectures.md)** | ReAct, Plan-and-Execute, Reflection, and **multi-agent** systems. |
| **[Memory](./memory.md)** | Short-term (context), long-term (vector), episodic, semantic. |
| **[Tools](./tools.md)** | Function calling, tool design, MCP, safety & sandboxing. |

---

## 🗺️ Where agents are used

| Use case | What the agent does |
|----------|---------------------|
| **Coding assistants** | Read repo, edit files, run tests, fix, repeat |
| **Deep research** | Search web, read, synthesize across many sources |
| **Customer ops** | Look up orders, issue refunds, update records via APIs |
| **Data analysis** | Write & run code, inspect results, iterate |
| **Workflow automation** | Multi-step business processes across systems |
| **Computer use** | Operate a browser / GUI to complete tasks |

---

## ⚠️ Why agents are hard

```mermaid
flowchart TD
    E1[Errors compound<br/>10 steps × 95% = 60% success] 
    E2[Cost & latency<br/>many LLM calls per task]
    E3[Getting stuck in loops]
    E4[Tool misuse / unsafe actions]
    E5[Hard to evaluate & debug]
```

- **Compounding errors:** each step can fail; long chains are fragile.
- **Cost/latency:** every loop iteration is an LLM call.
- **Reliability:** they can loop forever, hallucinate tool calls, or take unsafe actions.

> 🧭 **Design principle:** use the *simplest* thing that works. A fixed [workflow](../agentic-loop/) (predefined steps) is more reliable than a free-roaming agent — only give the LLM control of the loop when the task genuinely needs open-ended decisions.

➡️ Start with **[architectures](./architectures.md)** to see the common agent patterns.
