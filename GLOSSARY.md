# 📓 Glossary

Quick one-line definitions of the buzzwords in this repo. Each links to its deep dive.

> Tip: this is the "I just need the gist" page. Follow a link when you want the diagrams and detail.

---

## Core model concepts

| Term | One-liner |
|------|-----------|
| **[LLM](./large-language-models/)** | A neural net trained to predict the next token; from that, language & reasoning emerge. |
| **[Transformer](./transformers/)** | The architecture behind modern LLMs; uses **self-attention**. |
| **Self-attention** | Every token looks at every other token to build meaning in context. → [transformers](./transformers/) |
| **[Token](./tokenization/)** | The chunk of text (word/sub-word) a model actually reads; cost & context are counted in these. |
| **[Tokenization](./tokenization/)** | Converting text ↔ token IDs (e.g. BPE). |
| **[Embedding](./embeddings/)** | A vector capturing meaning; similar things sit close together. |
| **Foundation model** | A broadly pretrained model you adapt to many tasks. → [LLMs](./large-language-models/) |
| **In-context learning** | Learning a task from examples in the prompt, no weight change. → [LLMs](./large-language-models/) |
| **Context window** | Max tokens a model can consider at once. → [context engineering](./context-engineering/) |
| **Parameters** | The learned weights of a model (billions). → [LLMs](./large-language-models/) |

## Retrieval & knowledge

| Term | One-liner |
|------|-----------|
| **[RAG](./rag/)** | Retrieve documents at query time and feed them to the LLM for grounded answers. |
| **[Chunking](./rag/chunking.md)** | Splitting documents into retrievable pieces — the #1 RAG quality lever. |
| **[Retrieval](./rag/retrieval.md)** | Finding relevant chunks: sparse (keyword), dense (semantic), or hybrid. |
| **[Reranking](./rag/reranking.md)** | Re-scoring retrieved candidates so the best land on top. |
| **[Vector database](./vector-databases/)** | Stores embeddings & finds nearest ones fast via ANN indexes (HNSW, IVF). |
| **ANN** | Approximate Nearest Neighbor search — fast, slightly-inexact vector lookup. → [vector DBs](./vector-databases/) |
| **Hybrid search** | Combine keyword (BM25) + vector retrieval. → [retrieval](./rag/retrieval.md) |
| **[Knowledge graph](./knowledge-graphs/)** | Entities + relationships; powers multi-hop reasoning and Graph RAG. |
| **[Graph RAG](./rag/architectures/)** | RAG that retrieves over a knowledge graph instead of flat chunks. |

## Training & adaptation

| Term | One-liner |
|------|-----------|
| **[Fine-tuning](./fine-tuning/)** | Updating a model's weights to teach new skills, style, or format. |
| **[PEFT](./fine-tuning/methods.md)** | Parameter-Efficient Fine-Tuning — train a tiny fraction of params. |
| **[LoRA](./fine-tuning/methods.md)** | Inject small low-rank matrices; the popular cheap fine-tuning method. |
| **[QLoRA](./fine-tuning/methods.md)** | LoRA on top of a 4-bit quantized base — fine-tune big models on one GPU. |
| **[SFT](./fine-tuning/alignment.md)** | Supervised fine-tuning on (prompt → ideal answer) pairs. |
| **[RLHF](./fine-tuning/alignment.md)** | Reinforcement Learning from Human Feedback; align to human preferences. |
| **[DPO](./fine-tuning/alignment.md)** | Direct Preference Optimization — simpler alignment without a reward model. |
| **[Distillation](./distillation/)** | Train a small "student" to imitate a large "teacher." |
| **[Quantization](./quantization/)** | Store weights in fewer bits (e.g. 4-bit) → smaller, faster models. |

## Prompting & context

| Term | One-liner |
|------|-----------|
| **[Prompt engineering](./prompt-engineering/)** | Shaping the input text to steer output. |
| **Zero-/few-shot** | Prompting with no / a few examples. → [prompt engineering](./prompt-engineering/) |
| **[Chain-of-Thought](./prompt-engineering/)** | Ask the model to reason step by step. |
| **[Context engineering](./context-engineering/)** | Managing everything in the context window within a token budget. |

## Agents

| Term | One-liner |
|------|-----------|
| **[Agent](./agents/)** | An LLM that decides, uses tools, and acts in a loop toward a goal. |
| **[Agentic loop](./agentic-loop/)** | The reason → act → observe cycle powering agents. |
| **[ReAct](./agents/architectures.md)** | Interleave reasoning and tool actions. |
| **[Tool / function calling](./agents/tools.md)** | The model emits a structured call; your code runs it. |
| **[MCP](./mcp/)** | Open standard connecting AI apps to tools/data — "USB-C for AI." |
| **[Agent memory](./agents/memory.md)** | Short-term (context) + long-term (retrieved) state. |
| **[Multi-agent](./agents/architectures.md)** | Multiple specialized agents collaborating. |

## Model families & scaling

| Term | One-liner |
|------|-----------|
| **[Mixture of Experts (MoE)](./mixture-of-experts/)** | Activate only a few "expert" sub-nets per token; big capacity, low per-token compute. |
| **[Reasoning model](./reasoning-models/)** | Trained to "think" longer at inference (test-time compute). |
| **[Diffusion model](./diffusion-models/)** | Generate images/video by iteratively denoising. |
| **[Multimodal](./multimodal/)** | Handles text + images + audio + video together. |
| **Scaling laws** | More data/params/compute → predictably better models. → [LLMs](./large-language-models/) |

## Serving, quality & safety

| Term | One-liner |
|------|-----------|
| **[Inference optimization](./inference-optimization/)** | Make serving faster/cheaper: KV cache, batching, speculative decoding. |
| **KV cache** | Store past tokens' keys/values to avoid recomputation. → [inference](./inference-optimization/) |
| **[Evaluation](./evaluation/)** | Measuring whether a system is good — "the new unit tests." |
| **[LLM-as-a-judge](./evaluation/)** | Use a strong LLM to score another model's outputs. |
| **[Guardrails](./guardrails/)** | Safety checks on inputs & outputs. |
| **[Prompt injection](./guardrails/)** | Malicious instructions hidden in input/data hijack the model. |
| **[Hallucination](./hallucination/)** | Confident, plausible, but false output. |

---

➡️ Back to the **[full map & pillars](./README.md)**.
