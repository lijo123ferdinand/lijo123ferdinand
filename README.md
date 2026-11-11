## Lijo Ferdinand

Software engineer in Bangalore. I build backend services and the LLM systems on top of them, and I
rebuild the infrastructure underneath from scratch (consensus, storage engines, inference servers)
to learn exactly how it fails.

### At work: Torry Harris, 2023 to now

Python services for DigitMarket, a multi-tenant e-commerce product, and 4Sight, its analytics and AI product.

- **Performance.** Cut the recommendation endpoints from about a minute to interactive: pooled
  connections, batched vector search, and a fix to a FAISS index that was rebuilt on every request.
- **Migration.** Led the Flask to FastAPI migration as a strangler fig, and made the service run on
  SQL Server as well as MySQL through one translation layer.
- **AI platform.** Own the MCP tool layer and the 19-intent orchestration behind three production
  AI assistants (about 1,000 users). The model never writes SQL; it calls typed tools.
- **Safety and security.** Built a multilingual content-moderation engine (rules plus ONNX models,
  with a rules-only fallback) and KEK/DEK envelope encryption for personal data.

### Selected projects

Each one is built from the primitives up, tested by trying to break it, and documented with its
limitations next to its results.

| Project | What it is | Evidence |
|---|---|---|
| **[quorumkv](https://github.com/lijo123ferdinand/quorumkv)** <br> Go, gRPC | A linearizable, fault-tolerant key-value store. Raft written from scratch: pre-vote, group commit, snapshots, exactly-once client writes. | A chaos run sends `kill -9` to live nodes while 8 clients write, then checks the recorded history with Porcupine. It passes locally and in CI. |
| **[LeenusDB](https://github.com/lijo123ferdinand/LeenusDB)** <br> Go, stdlib only | A crash-safe LSM storage engine: CRC32C write-ahead log, memtable, SSTables, compaction, deterministic recovery. | Zero acknowledged writes lost under `kill -9` mid-write, torn writes and on-disk bit flips. |
| **[tinyserve](https://github.com/lijo123ferdinand/tinyserve)** <br> Python, NumPy | A vLLM-style inference engine: paged KV cache with copy-on-write, continuous batching, prefix caching, speculative decoding, OpenAI-compatible streaming. | Paging cut KV-cache waste from 61.7% to 7.1%, which fits 3.25× more concurrent sequences. |
| **[FerraMesh](https://github.com/lijo123ferdinand/FerraMesh)** <br> Python, FastAPI, Redis | An OpenAI-compatible LLM gateway: latency and cost routing, a two-tier exact and semantic cache, per-model circuit breakers, SSE streaming. | Token-bucket rate limiting in Redis Lua that stays correct across N pods. The test suite runs offline. |
| **[Rewind](https://github.com/lijo123ferdinand/rewind)** <br> Python, stdlib only | Record, replay and fork AI agent runs through a proxy. Works with any framework, with no code change. | Replays are deterministic, offline and free. A changed agent step fails CI. Verified against the real OpenAI and Anthropic SDKs. |
| **[ToolTune](https://github.com/lijo123ferdinand/tooltune)** <br> PyTorch, PEFT | LoRA fine-tuning of a 0.5B model for agent tool calling, in a hand-written training loop on an 8 GB laptop. | Exact tool calls on held-out cases rose from 49% to 86%. The remaining failures are listed in the README. |

<details>
<summary><b>More: ML systems, and the LLM application stack one primitive per repo</b></summary>

<br>

| Project | What it is |
|---|---|
| **[Agent platform](https://github.com/lijo123ferdinand/agentic-ai-lijofer)** | A commerce agent on Temporal, LangGraph, MCP and Qdrant: tenant isolation in the repository layer, idempotent writes with compensations, guardrails that fail closed, and a CI eval gate. |
| **[JosephinML](https://github.com/lijo123ferdinand/JosephinML)** | Churn prediction as a production ML system: versioned registry, shadow and canary routing, PSI drift monitoring, a leakage audit that blocks registration, one-call rollback. |
| **[Self-Pruning Network](https://github.com/lijo123ferdinand/aqua_case_study)** | Autodiff and an MLP in NumPy that prunes its own weakest weights during training. At 90% sparsity it keeps test accuracy (0.988 against 0.985 dense). |
| **[hnsw-lite](https://github.com/lijo123ferdinand/hnsw-lite)** | A vector database in NumPy: HNSW, IVF-Flat, product quantization, metadata filters. 0.998 recall@10 at 3,464 queries/s on 20k×128 vectors. |
| **[ragscope](https://github.com/lijo123ferdinand/ragscope)** | A RAG pipeline with an evaluation harness. Finding: chunking moved MRR more than the choice of retriever did. |
| **[text2sql-agent](https://github.com/lijo123ferdinand/text2sql-agent)** | A self-correcting NL to SQL agent, read-only by three independent mechanisms. The repair loop lifts execution accuracy from 71% to 86%. |
| **[schemaforge](https://github.com/lijo123ferdinand/schemaforge)** | Structured extraction with JSON repair, schema validation and field-scoped re-asks. F1 rises from 71.5% to 89.5%. |
| **[promptshield](https://github.com/lijo123ferdinand/promptshield)** | An LLM security firewall, as a library or a reverse proxy: prompt injection, PII redaction, secret-leak detection. |
| **[mnemo](https://github.com/lijo123ferdinand/mnemo)** | Long-term memory for agents on SQLite, served over MCP. Same recall as pasting the full history, with 18.8× fewer context tokens. |
| **[taskforge](https://github.com/lijo123ferdinand/taskforge)** | Multi-agent orchestration without a framework: a planner, a validated task graph, parallel workers, a critic. |
| **[promptopt](https://github.com/lijo123ferdinand/promptopt)** | A DSPy-style prompt optimizer: bootstrap few-shot and instruction search, scored on a held-out set. |
| **[diffsage](https://github.com/lijo123ferdinand/diffsage)** | An AI code reviewer for git diffs that tries to disprove its own findings. Outputs SARIF and ships as a GitHub Action. |

The LLM-stack numbers come from an offline deterministic mock model, not a hosted one, and each README says so.

</details>

### How to read these repos

The benchmarks are laptop numbers, not production traffic. Start with the limitations and
failure-mode sections; they say more than the feature lists.

**Languages and tools:** Python, Go, SQL, TypeScript · FastAPI, gRPC, Kafka, Redis, PostgreSQL, MySQL ·
MCP, LangGraph, RAG, FAISS, Qdrant, PyTorch · Docker, AWS (EC2, S3)

lijoferdinand@gmail.com · [LinkedIn](https://linkedin.com/in/lijo-ferdinand)
