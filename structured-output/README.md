# 🧾 Structured Output & Constrained Decoding

> **One-liner:** When you need an LLM's output to be **valid JSON** (or match a schema/
> grammar) *every time* — not "usually" — you use structured output techniques that
> **constrain** what the model is allowed to generate.

```mermaid
flowchart LR
    Q[Prompt + schema] --> M[(LLM)]
    M --> V{Valid JSON<br/>matching schema?}
    V -- guaranteed --> OUT["{ 'name': ..., 'age': ... }"]
```

---

## 🤔 Why it's hard

Free-form generation can produce *almost*-valid JSON — a trailing comma, a missing quote, a
chatty preamble — and your parser breaks. For pipelines and [agents](../agents/tools.md),
"almost valid" = broken.

```mermaid
flowchart LR
    BAD["Sure! Here's the JSON:<br/>{name: 'Al', age: 30,}"] --> ERR[💥 Parse error]
```

---

## 🧰 The spectrum of approaches (weak → strong guarantee)

```mermaid
flowchart LR
    P[Prompt nicely] --> F[Few-shot examples] --> J[JSON mode] --> S[Schema-enforced] --> G[Grammar-constrained decoding]
    P -.- W[hope] 
    G -.- GUAR[guarantee]
```

| Approach | Guarantee | How |
|----------|-----------|-----|
| **Prompting** | Weak | "Respond only with JSON" |
| **Few-shot** | Better | Show example outputs |
| **JSON mode** | Valid JSON | Provider forces JSON syntax |
| **Schema / function calling** | Matches your schema | Give a JSON Schema; model fills it |
| **Constrained decoding** | Strict | Only allow tokens that keep output valid |

---

## 🔒 Constrained / guided decoding (the strong version)

At each step the decoder **masks out** any token that would violate the schema/grammar — so
invalid output is *impossible*, not just discouraged.

```mermaid
flowchart TD
    STATE[Current partial output] --> ALLOW[Compute allowed next tokens<br/>per grammar/schema]
    ALLOW --> MASK[Mask everything else]
    MASK --> PICK[Model picks from allowed only]
    PICK --> STATE
```

- Tools/libraries: **Outlines**, **JSON Schema / function calling**, **GBNF grammars** (llama.cpp), **XGrammar**.
- Works for JSON, regex patterns, enums, even full context-free grammars.

---

## 🔗 Relationship to function calling

[Function/tool calling](../agents/tools.md) *is* structured output: the model emits
arguments that must match the tool's parameter schema. Reliable agents depend on this.

```mermaid
flowchart LR
    TOOL[Tool schema] --> M[(LLM)] --> CALL["valid args:<br/>get_weather{city:'Paris'}"]
```

---

## ⚖️ Trade-offs & tips

- ✅ Reliable, parseable output; fewer retries; safer pipelines.
- ⚠️ Over-constraining can hurt quality (the model can't "think" in prose first) — allow a reasoning field, or reason *then* format.
- ⚠️ Very complex schemas can confuse the model — keep them as simple as the task allows.
- 💡 Still **validate** downstream; treat model output as untrusted ([guardrails](../guardrails/)).

---

## 🗺️ Where it's used

- **Data extraction** (docs → JSON) and classification.
- **[Agent tool calls](../agents/tools.md)** and API integrations.
- **Any LLM step feeding code** that expects a fixed shape.

➡️ Related: **[agents/tools](../agents/tools.md)** · **[prompt-engineering](../prompt-engineering/)** · **[guardrails](../guardrails/)**.
