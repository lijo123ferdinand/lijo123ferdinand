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

**The LLM-application stack, one primitive per repo.** Each is dependency-light Python with a CI-tested
benchmark. Each runs offline against a deterministic mock model, with optional Anthropic and Ollama providers.
Where a number comes from the mock rather than a real model, the README says so.

| | |
|---|---|
| **[tinyserve](https://github.com/lijo123ferdinand/tinyserve)** | **vLLM-style inference engine in NumPy**: paged KV cache with copy-on-write, continuous batching with preemption, prefix caching, speculative decoding with the exact rejection-sampling rule, and OpenAI-compatible SSE streaming. Paging cut KV waste **61.7% → 7.1%** (3.25× more concurrent sequences); continuous batching needed 3.5× fewer forward passes. Includes an honest note on why speculative decoding gives no wall-clock gain on CPU. |
| **[hnsw-lite](https://github.com/lijo123ferdinand/hnsw-lite)** | **A vector database from scratch in NumPy**: HNSW (with deletes and compaction), IVF-Flat and product quantization; cosine, L2 and dot product; metadata filters backed by an inverted index; atomic save/load with no pickle; a CLI and an HTTP API. **0.998 recall@10 at 3,464 queries/s** on 20k×128 vectors (1.8× brute force). The README also reports the hard case: on random Gaussian data recall falls to 0.89, and to 0.72 after deletes. |
| **[ragscope](https://github.com/lijo123ferdinand/ragscope)** | **RAG pipeline plus an evaluation harness**: three chunkers, BM25 written from scratch, dense and hybrid (RRF) retrieval, answers that cite character spans in the source, recall@k / MRR / nDCG / faithfulness. Finding: **chunking moved MRR more (~0.10) than the choice of retriever did**; best setup 0.858 MRR. |
| **[text2sql-agent](https://github.com/lijo123ferdinand/text2sql-agent)** | **Self-correcting NL→SQL agent on SQLite**: narrows the prompt to the relevant tables, enforces read-only in three independent ways (validator, read-only connection, SQLite authorizer), and retries using database feedback. With no sample values in the prompt, the repair loop lifts execution accuracy **71% → 86%**; narrowing to the relevant tables beats showing all 12 (91% vs 83%). |
| **[schemaforge](https://github.com/lijo123ferdinand/schemaforge)** | **Reliable structured extraction**: a tolerant JSON repair parser, a JSON Schema validator, a re-ask loop that names the exact failing fields, and source grounding that flags values not found in the input. Lifts F1 **71.5% → 89.5%** over plain `json.loads` at ~2 calls per document; flagged every planted fake value. |
| **[promptshield](https://github.com/lijo123ferdinand/promptshield)** | **LLM security firewall**, as a library or a reverse proxy: prompt injection (including look-alike letters, invisible characters and base64 disguises), instructions hidden in documents and tool output, personal-data redaction (Luhn and Aadhaar checksums) and secret-leak detection, with policies and an audit log. PII and secret detection scored 1.0 precision and recall on a test set written after the rules were frozen; the README reports plainly that the pattern rules catch only 17% of reworded injection attacks. |
| **[mnemo](https://github.com/lijo123ferdinand/mnemo)** | **Long-term memory for agents** on SQLite, also served as an MCP server: episodic, semantic and procedural memory; a new fact that contradicts an old one replaces it, with versioning; retrieval scored by relevance × recency × importance. Same recall as pasting the whole history, with **18.8× fewer context tokens** (58× at 24 sessions). |
| **[taskforge](https://github.com/lijo123ferdinand/taskforge)** | **Framework-free multi-agent orchestration**: a planner builds a validated task graph, workers with JSON-schema tools run in parallel on asyncio, and a critic sends bad output back. Checkpoint and resume, token budgets, human approval gates. The critic raised end-to-end success **39% → 82%** under injected failures. |
| **[promptopt](https://github.com/lijo123ferdinand/promptopt)** | **DSPy-style prompt optimizer, framework-free**: bootstrap few-shot, instruction search with successive halving, random search, and compiled programs saved as diffable files, scored on a held-out set with an overfitting check. |
| **[diffsage](https://github.com/lijo123ferdinand/diffsage)** | **AI code reviewer for git diffs**: its own diff parser, static checks, an LLM review per hunk, then a second pass that tries to disprove each finding. Outputs SARIF and GitHub PR comments, and ships as a GitHub Action. Static checks alone catch 41.5% of seeded bugs; the full pipeline reaches **F1 0.93**. |

Also building, and being prepared for public release: **ml-from-scratch** — the ML stack with no
frameworks: an autograd engine, an MLP it trains on XOR, a Transformer/GPT built on that same engine
and trained end to end, and a BPE tokenizer.

Each repo documents its **design tradeoffs and limitations** honestly — the benchmarks are laptop
numbers, not production traffic, and the READMEs say so. The interesting reading is the failure-mode
tables, not the feature lists.

**Working with:** Python · FastAPI · Flask · LLM tool calling · RAG · MCP · LangGraph · FAISS/Qdrant ·
Redis · Apache Kafka · PostgreSQL · MySQL · Go · Docker · AWS (EC2, S3)

📫 lijoferdinand@gmail.com · [LinkedIn](https://linkedin.com/in/lijo-ferdinand)
