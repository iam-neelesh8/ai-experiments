# 🔌 MCP — Model Context Protocol

> **One-liner:** MCP is an **open standard** for connecting AI apps to tools, data, and
> systems. Instead of building a custom integration for every app × every tool, you build
> an **MCP server** once and any MCP-compatible client can use it. Think **"USB-C for AI."**

```mermaid
flowchart TD
    A[AI app / MCP client] --> P{MCP protocol}
    P --> S1[GitHub server]
    P --> S2[Database server]
    P --> S3[Filesystem server]
    P --> S4[Slack server]
```

---

## 🧩 The problem it solves

Without a standard, every AI app needs a **bespoke connector** to every system — an N×M
integration mess.

```mermaid
flowchart LR
    subgraph Before["❌ Before MCP -N×M-"]
        a1[App 1] --- t1[Tool A]
        a1 --- t2[Tool B]
        a2[App 2] --- t1
        a2 --- t2
    end
    subgraph After["✅ With MCP -N+M-"]
        c1[App 1] --> mcp{{MCP}}
        c2[App 2] --> mcp
        mcp --> ta[Tool A server]
        mcp --> tb[Tool B server]
    end
```

Build the tool server **once**; every MCP client gets it for free.

---

## 🏗️ Architecture: client ↔ server

```mermaid
flowchart LR
    HOST[Host app<br/>-e.g. an AI assistant-] --> CLIENT[MCP client]
    CLIENT <-->|JSON-RPC| SERVER[MCP server]
    SERVER --> RES[Data / API / files]
```

- **Host** — the AI application (chat app, IDE, agent).
- **Client** — the connector inside the host that speaks MCP.
- **Server** — exposes capabilities from some system (GitHub, a DB, the filesystem).

---

## 🧰 What an MCP server exposes

| Primitive | What it is |
|-----------|-----------|
| **Tools** | Actions the model can call (functions/APIs) → see [tools](../agents/tools.md) |
| **Resources** | Data the model can read (files, records, docs) |
| **Prompts** | Reusable prompt templates the server offers |

---

## ⚖️ Why it matters

```mermaid
flowchart LR
    MCP[MCP] --> P1[✅ Write a tool once,<br/>use everywhere]
    MCP --> P2[✅ Decouples tools from apps]
    MCP --> P3[✅ Growing ecosystem of servers]
    MCP --> C1[⚠️ Security: servers run with real access]
```

- ✅ **Interoperability** — no per-app rewrites.
- ✅ **Reusability** — a rich ecosystem of ready servers.
- ⚠️ **Security** — an MCP server can touch real systems; treat untrusted servers and their
  outputs carefully (see [guardrails](../guardrails/) and prompt injection).

---

## 🗺️ Where MCP is used

- **AI coding assistants** connecting to repos, docs, terminals.
- **[Agents](../agents/)** that need a standard way to reach many tools.
- **Enterprise assistants** wiring into internal databases & apps.
- Anywhere you'd otherwise write a one-off [function-calling](../agents/tools.md) integration.

➡️ MCP is *how* agents reach tools → see **[agents/tools](../agents/tools.md)**.
