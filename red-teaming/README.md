# 🧨 Red-Teaming

> **One-liner:** Red-teaming is **deliberately attacking your own AI** to find how it breaks
> *before* real users or adversaries do — probing for harmful outputs, jailbreaks, prompt
> injection, and data leaks. It's offense in service of defense.

```mermaid
flowchart LR
    RT[🧨 Red team<br/>-attack-] --> FIND[Find failures]
    FIND --> FIX[Blue team fixes<br/>-guardrails, tuning-]
    FIX --> RETEST[Re-test]
    RETEST --> RT
```

---

## 🎯 What red-teamers try to make the model do

```mermaid
flowchart TD
    G[Attack goals] --> H[Produce harmful / unsafe content]
    G --> J[Jailbreak past safety rules]
    G --> I[Fall for prompt injection]
    G --> L[Leak system prompt / PII / secrets]
    G --> B[Reveal bias / toxicity]
    G --> M[Misuse tools / take unsafe actions]
```

For [agents with tools](../agents/tools.md), the stakes are higher — an exploit can trigger
**real actions**, not just bad text.

---

## 🗡️ Common attack techniques

| Technique | Idea |
|-----------|------|
| **Prompt injection** | Hide instructions in input or retrieved data → [guardrails](../guardrails/) |
| **Jailbreak prompts** | "Ignore previous instructions", roleplay, hypotheticals |
| **Obfuscation** | Encodings, typos, other languages to dodge filters |
| **Many-shot** | Flood context with examples that erode safety |
| **Crescendo** | Escalate gradually over turns toward a harmful goal |
| **Tool/data poisoning** | Malicious content in docs an [agent](../agents/) reads |

---

## 🤖 Manual vs automated red-teaming

```mermaid
flowchart LR
    MAN["👤 Manual<br/>creative human experts"] --> COV[Deep, novel attacks]
    AUTO["🤖 Automated<br/>AI generates attacks at scale"] --> SCALE[Broad coverage, cheap]
    COV & SCALE --> BEST[Use both]
```

- **Manual** — humans find creative, novel exploits.
- **Automated** — another model generates thousands of attack prompts ([synthetic-data](../synthetic-data/)) for scale and regression testing.

---

## 🔁 Red-teaming in the lifecycle

```mermaid
flowchart LR
    BUILD[Build] --> RT[Red-team]
    RT --> FIX[Add guardrails / retrain]
    FIX --> EVAL[Add attacks to eval suite]
    EVAL --> SHIP[Ship]
    SHIP --> MON[Monitor + red-team again]
    MON --> RT
```

Findings become **permanent [evaluation](../evaluation/) cases** so the same hole can't silently reopen.

---

## ⚖️ Red-teaming vs evaluation vs guardrails

- **[Evaluation](../evaluation/)** — measures overall quality/behavior.
- **Red-teaming** — actively *hunts for worst-case* failures.
- **[Guardrails](../guardrails/)** — the runtime defenses you build to block what red-teaming found.

---

## 🗺️ Where red-teaming is used

- **Pre-launch safety sign-off** for customer-facing AI.
- **Regulated / high-risk** deployments.
- **[Agent](../agents/)** systems with tool access.
- **Ongoing** — new jailbreaks appear constantly; it's never "done."

➡️ Related: **[guardrails](../guardrails/)** · **[evaluation](../evaluation/)** · **[hallucination](../hallucination/)**.
