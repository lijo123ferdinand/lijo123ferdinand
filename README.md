## Lijo Ferdinand

Backend engineer in Bangalore. I build production LLM and data systems — and I build the
infrastructure underneath them from scratch, so I know how it fails.

**At work** (Torry Harris, since 2023) I own the LLM assistant platform for DigitMarket, a
multi-tenant e-commerce product: tool calling, streaming, and an MCP tool server that
generates LLM tools straight from OpenAPI specs — a 650-operation API becomes auth-injected,
PII-redacting tools with no hand-written glue. Also Kafka/Flink pipelines at 10k events/sec
and a dynamic-pricing engine that moved gross margin 12%.

**Outside work** I build the primitives myself:

| | |
|---|---|
| **[LeenusDB](https://github.com/lijo123ferdinand/LeenusDB)** | A crash-safe LSM key-value store in Go, stdlib only. CRC32C write-ahead log, memtable → SSTable → compaction, deterministic recovery. A chaos suite induces `kill -9` mid-write, torn writes and on-disk bit flips, and proves **zero acknowledged-data loss**. |
| **[FerraMesh](https://github.com/lijo123ferdinand/FerraMesh)** | An OpenAI-compatible LLM gateway. Pluggable routing, **distributed Redis-Lua token-bucket rate limiting** correct across N pods, a two-tier exact + semantic cache, per-model circuit breakers, SSE streaming, Prometheus. 80 tests, runs fully offline. |
| **[JosephinML](https://github.com/lijo123ferdinand/JosephinML)** | Churn prediction as a *production ML system* — the model is deliberately the smallest piece. Versioned registry, train/serve-skew defence, shadow + deterministic canary routing, PSI drift monitoring, a leakage audit that blocks registration, one-call rollback. |

Each repo documents its **design tradeoffs and its limitations** honestly — the benchmarks are
laptop numbers, not production traffic, and the READMEs say so. The interesting reading is the
failure-mode tables, not the feature lists.

**Working with:** Python · Go · FastAPI · Flask · Kafka · Flink · Redis · PostgreSQL · MySQL ·
Docker · LLM tool calling, MCP, RAG, evals

📫 lijoferdinand@gmail.com · [LinkedIn](https://linkedin.com/in/lijo-ferdinand)
