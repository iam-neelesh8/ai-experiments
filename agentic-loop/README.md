# 🔁 The Agentic Loop

> **One-liner:** The agentic loop is the engine inside every [agent](../agents/): a cycle
> of **Reason → Act → Observe** that repeats until the goal is met. It's what lets an LLM
> take *many* steps and adapt, instead of answering in one shot.

```mermaid
flowchart LR
    R[🧠 Reason<br/>what should I do next?] --> A[🛠️ Act<br/>take an action / call a tool]
    A --> O[👀 Observe<br/>read the result]
    O --> C{🎯 Goal met?}
    C -- no --> R
    C -- yes --> F([✅ Final answer])
```

---

## 🔬 One turn of the loop, in detail

```mermaid
sequenceDiagram
    participant M as LLM -brain-
    participant R as Runtime
    participant T as Tool / env
    Note over M: 🧠 Reason: decide next action
    M->>R: Action: call tool X with args
    R->>T: execute X
    T->>R: raw result
    R->>M: 👀 Observation -added to context-
    Note over M: Reason again with new info...
    M->>R: Action: final answer
```

The **LLM proposes**, the **runtime executes**, the **result feeds back** into context.
Repeat. That feedback of *real-world results* into the next reasoning step is the whole point.

---

## 🧩 What's happening each iteration

```mermaid
flowchart TD
    S[Current state + goal + history] --> REASON[🧠 LLM reasons]
    REASON --> DECIDE{Decide}
    DECIDE -->|need a tool| ACT[Call tool]
    DECIDE -->|done| DONE[Answer]
    ACT --> OBS[Observation]
    OBS --> UPDATE[Update context / memory]
    UPDATE --> S
```

Every loop draws on the other pillars:
- **Reason** uses [prompt engineering](../prompt-engineering/) (how it's asked to think).
- The state it reasons over is [context-engineered](../context-engineering/) (what's in the window).
- **Act** uses [tools](../agents/tools.md).
- **Observe** updates [memory](../agents/memory.md).

---

## 🛑 Loop control — stopping conditions

An unbounded loop is a bug (and a bill). Every agent needs **exit conditions**:

```mermaid
flowchart TD
    LOOP[Loop iteration] --> C1{Goal achieved?}
    C1 -- yes --> STOP1[✅ Done]
    C1 -- no --> C2{Max steps hit?}
    C2 -- yes --> STOP2[⏹️ Give up / escalate]
    C2 -- no --> C3{Budget / time exceeded?}
    C3 -- yes --> STOP3[💸 Halt]
    C3 -- no --> C4{Stuck / repeating?}
    C4 -- yes --> STOP4[🔁 Break loop]
    C4 -- no --> LOOP
```

| Guard | Why |
|-------|-----|
| **Goal check** | The intended exit |
| **Max iterations** | Prevent infinite loops |
| **Token / cost / time budget** | Prevent runaway spend |
| **Loop / repetition detection** | Catch the agent doing the same thing over and over |
| **Human escalation** | Hand off when stuck or on risky actions |

---

## 🌀 Enriched loops: planning & reflection

The basic loop is reactive. Add cognition on top:

```mermaid
flowchart LR
    P[📋 Plan<br/>upfront] --> LOOP
    subgraph LOOP["Reason → Act → Observe"]
      direction LR
      RE[Reason] --> AC[Act] --> OB[Observe]
    end
    LOOP --> REF[🔍 Reflect<br/>did that work?]
    REF -- adjust --> LOOP
    REF -- replan --> P
```

- **Planning** ([Plan-and-Execute](../agents/architectures.md)) — decide the steps before looping.
- **Reflection** ([Reflexion / Self-Refine](../agents/architectures.md)) — critique results and self-correct.
- These are just the loop with a **plan** in front and a **critic** at the end.

---

## ⚖️ Workflows vs Agents (know the difference)

```mermaid
flowchart TB
    subgraph WF["🧭 Workflow -you control the path-"]
        w1[Step 1] --> w2[Step 2] --> w3[Step 3]
    end
    subgraph AG["🤖 Agent -LLM controls the path-"]
        a1[LLM decides] --> a2[LLM decides] --> a3[LLM decides]
    end
```

- **Workflow:** predefined steps; the LLM fills in pieces. **Predictable, cheap, reliable.**
- **Agent:** the LLM dynamically decides the path and when to stop. **Flexible, but riskier.**

> 🧭 **Golden rule:** use the loop only when you need it. If a fixed workflow solves the
> task, prefer it — free-roaming loops are powerful but harder to make reliable. Give the
> LLM control of the loop only when the task genuinely requires open-ended decisions.

---

## 🗺️ Where you see the agentic loop

- **Coding agents** — reason → edit → run tests → read failures → fix → repeat.
- **Deep research** — search → read → decide what's missing → search again → synthesize.
- **[Agentic RAG](../rag/architectures/)** — retrieve → judge relevance → re-retrieve → answer.
- **Computer/browser use** — look at screen → act → observe → continue.

➡️ Back to [Agents](../agents/) · or the [full map](../README.md).
