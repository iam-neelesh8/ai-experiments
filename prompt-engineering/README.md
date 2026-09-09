# ✍️ Prompt Engineering

> **One-liner:** Prompt engineering is shaping the **input text** to steer the model's
> output — the cheapest, fastest, most reversible way to change behavior. No training,
> no infrastructure, just words.

```mermaid
flowchart LR
    P[Prompt<br/>instructions + examples + input] --> M[(LLM)] --> O[Output]
    O -. iterate .-> P
```

Where [fine-tuning](../fine-tuning/) changes the model and [RAG](../rag/) changes its knowledge,
**prompt engineering changes what you ask and how**. Always try it first.

---

## 🧱 Anatomy of a good prompt

```mermaid
flowchart TD
    ROLE[Role / persona<br/>'You are a senior editor'] --> P
    TASK[Task / instruction<br/>clear, specific] --> P
    CTX[Context<br/>background, data] --> P
    EX[Examples<br/>show the pattern] --> P
    FMT[Output format<br/>JSON, bullets, schema] --> P
    CON[Constraints<br/>length, tone, do/don't] --> P
    P[📩 Final prompt]
```

The more precisely you specify **role, task, context, format, and constraints**, the more reliable the output.

---

## 🎓 Core techniques

### Zero-shot
Just ask. No examples. Works for simple, common tasks.

```mermaid
flowchart LR
    Q["'Classify sentiment: I love this'"] --> M[(LLM)] --> A["Positive"]
```

### Few-shot
Show a few **examples** of the input→output pattern. The model imitates them.

```mermaid
flowchart LR
    E["'great' → +<br/>'awful' → −<br/>'okay' → ?"] --> M[(LLM)] --> A["neutral"]
```

- Great for enforcing **format** and handling **ambiguous** tasks. 2–5 diverse examples usually enough.

### Chain-of-Thought (CoT)
Ask the model to **reason step by step** before answering. Dramatically improves math,
logic, and multi-step problems.

```mermaid
flowchart LR
    Q[Hard question] --> R["'Let's think<br/>step by step'"] --> STEPS[Show reasoning] --> A[Better answer]
```

### Self-consistency
Sample **multiple** CoT reasoning paths and take the **majority** answer.

```mermaid
flowchart TD
    Q[Question] --> P1[Reasoning path 1 → 42]
    Q --> P2[Reasoning path 2 → 42]
    Q --> P3[Reasoning path 3 → 17]
    P1 & P2 & P3 --> V[Vote → 42 ✅]
```

### Beyond linear: ToT / GoT
**Tree-of-Thought** explores and *evaluates* multiple branches; **Graph-of-Thought**
generalizes to a graph. Used for hard search/planning problems.

```mermaid
flowchart TD
    R[Problem] --> A1[Branch A] --> A2[eval]
    R --> B1[Branch B] --> B2[eval ✅ best]
    R --> C1[Branch C] --> C2[prune]
```

---

## 🧰 Technique cheat-sheet

| Technique | Use when |
|-----------|----------|
| **Zero-shot** | Simple, familiar tasks |
| **Few-shot** | Need a specific format or handle ambiguity |
| **Chain-of-Thought** | Math, logic, multi-step reasoning |
| **Self-consistency** | High-stakes reasoning; can afford extra calls |
| **Role prompting** | Set expertise/persona/tone |
| **Structured output** | Need parseable JSON/tables (use schemas) |
| **Instruction + delimiters** | Separate instructions from data (also safer vs injection) |
| **Prompt chaining** | Break a big task into smaller prompted steps |
| **ReAct** | Reasoning + tool use → see [agents](../agents/) |

---

## 🎛️ Decoding knobs (not the prompt, but shape the output)

| Parameter | Effect |
|-----------|--------|
| **Temperature** | Higher = more random/creative; lower = more deterministic |
| **Top-p / top-k** | Restrict the candidate token pool |
| **Max tokens** | Cap output length |
| **Stop sequences** | Where to cut generation |

> For extraction/classification → **low temperature**. For brainstorming → **higher**.

---

## ✅ Best practices

```mermaid
flowchart TD
    B1[Be specific<br/>vague in → vague out]
    B2[Show, don't just tell<br/>-examples-]
    B3[Separate instructions from data<br/>-delimiters-]
    B4[Ask for a format<br/>-and give a schema-]
    B5[Give the model an out<br/>-'say I don't know'-]
    B6[Iterate & test<br/>on real inputs]
```

- **Specificity beats cleverness.** Say exactly what you want.
- **Positive instructions** ("respond in 3 bullets") beat negative ones ("don't be verbose").
- **Let it say "I don't know"** to cut hallucination.
- **Test systematically** — small changes have big effects; keep a prompt eval set.

---

## 🔗 Prompt engineering vs Context engineering

Prompt engineering is about **the words you write**. As soon as you're also deciding
*what memory, retrieved docs, tool outputs, and history* fill the window, you've crossed
into **[context engineering](../context-engineering/)** — the superset for agentic systems.

➡️ Next: managing the whole window → **[context engineering](../context-engineering/)**.
