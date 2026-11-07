## Lijo Ferdinand

Software engineer in Bangalore — backend services, the LLM features on top of them, and the
infrastructure underneath. I rebuild the primitives from scratch so I actually understand how they
fail.

**At work** (Torry Harris, since 2023) I build Python services for DigitMarket, a multi-tenant
e-commerce product, and its analytics and AI product 4Sight. I **cut the recommendation endpoints
from ~a minute to interactive** (pooled connections, batched vector search, a FAISS-rebuild fix),
**led the Flask-to-FastAPI migration** as a strangler fig, and made the service run on SQL Server as
well as MySQL through one translation layer. On the AI side I **own the MCP tool layer** and
19-intent orchestration behind three production AI assistants (~1,000 users), built an agent
runtime where the model never writes SQL, a **multilingual content-moderation engine** (rules plus
ONNX models, with a rules-only fallback), and rebuilt our low-code engine's "Draft with AI" as a
**human-in-the-loop LangGraph agent**. Plus KEK/DEK envelope encryption for PII and the Next.js
chat apps on top.

Currently going deeper on system design and Kubernetes.

**Outside work I build the primitives myself:**

| | |
|---|---|
| **[Rewind](https://github.com/lijo123ferdinand/rewind)** | **Record, replay and fork AI agent runs.** A proxy between any agent and its model (OpenAI or Anthropic format, any framework, no code change): replay a run offline and deterministically at $0, catch the step where an agent's behaviour changed (a CI regression test for agents), and fork a run at step N with an edited reply. Python standard library only; verified with the real OpenAI and Anthropic SDKs. |
| **[ToolTune](https://github.com/lijo123ferdinand/tooltune)** | **LoRA fine-tuning a 0.5B model for agent tool calling**, in a hand-written PyTorch loop on Hugging Face Transformers + PEFT, on an 8 GB laptop. Exact tool calls on held-out cases: **49% → 86%**; the base model cancelled orders when asked for their status. Loss masking, gradient checkpointing (measured 7.6× faster on unified memory), and the remaining failures reported, not hidden. |
| **[Agent platform](https://github.com/lijo123ferdinand/agentic-ai-lijofer)** | A commerce agent on **Temporal + LangGraph + MCP + Qdrant**: tenant isolation in the repository layer, idempotent writes with compensations, guardrails that fail closed, an action-policy matrix where the model proposes and never authorises, hybrid retrieval (1.000 vs 0.062 dense on model numbers), and a CI eval gate. |
| **[FerraMesh](https://github.com/lijo123ferdinand/FerraMesh)** | An OpenAI-compatible **LLM gateway**. Pluggable latency/cost routing, **distributed Redis-Lua token-bucket rate limiting** correct across N pods, a two-tier exact + semantic cache, per-model circuit breakers, SSE streaming, Prometheus. 92 tests, runs fully offline. |
| **[LeenusDB](https://github.com/lijo123ferdinand/LeenusDB)** | A crash-safe **LSM key-value store** in Go, stdlib only. CRC32C write-ahead log, memtable → SSTable → compaction, deterministic recovery. A chaos suite induces `kill -9` mid-write, torn writes and on-disk bit flips, and proves **zero acknowledged-data loss**. |
| **[JosephinML](https://github.com/lijo123ferdinand/JosephinML)** | Churn prediction as a *production ML system* — the model is deliberately the smallest piece. Versioned registry, train/serve-skew defence, shadow + deterministic canary routing, PSI drift monitoring, a leakage audit that blocks registration, one-call rollback. |
| **[Self-Pruning Network](https://github.com/lijo123ferdinand/aqua_case_study)** | A neural-network library from scratch in **NumPy** — autodiff, an MLP trained with Adam, and pruning that removes the network's own weakest weights during training. At **90% sparsity** it keeps test accuracy (0.988 vs 0.985 dense) with 89% fewer FLOPs; at 98%, saliency pruning holds 98.9% where magnitude pruning falls to 82.4%. |

Also building, and being prepared for public release: **ml-from-scratch** — the ML stack with no
frameworks: an autograd engine, an MLP it trains on XOR, a Transformer/GPT built on that same engine
and trained end to end, and a BPE tokenizer.

Each repo documents its **design tradeoffs and limitations** honestly — the benchmarks are laptop
numbers, not production traffic, and the READMEs say so. The interesting reading is the failure-mode
tables, not the feature lists.

**Working with:** Python · FastAPI · Flask · LLM tool calling · RAG · MCP · LangGraph · FAISS/Qdrant ·
Redis · Apache Kafka · PostgreSQL · MySQL · Go · Docker · AWS (EC2, S3)

📫 lijoferdinand@gmail.com · [LinkedIn](https://linkedin.com/in/lijo-ferdinand)
