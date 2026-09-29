Lijo Ferdinand
AI/ML engineer in Bangalore — LLM/GenAI features, the ML systems that serve them, and the infrastructure underneath. I rebuild the primitives from scratch so I actually understand how they fail.

At work (Torry Harris, since 2023) I build REST APIs in Flask and FastAPI for DigitMarket, a multi-tenant e-commerce product, and its analytics sidecar 4Sight. I built the tool-calling layer behind the team's AI assistant (~1,000 users) — it generates the model's callable tools from our OpenAPI spec, injects each caller's auth and strips PII — cut the recommendation endpoints from ~a minute to interactive (pooled connections, batched vector search, a FAISS-rebuild fix), and shipped AI recommendations where the model phrases the answer but never picks or prices the product. Plus a MySQL↔SQL Server portability layer, KEK/DEK envelope encryption for PII, and the Next.js chat UIs on top.

Currently going deeper on system design and Kubernetes.

Outside work I build the primitives myself:

FerraMesh	An OpenAI-compatible LLM gateway. Pluggable latency/cost routing, distributed Redis-Lua token-bucket rate limiting correct across N pods, a two-tier exact + semantic cache, per-model circuit breakers, SSE streaming, Prometheus. 80 tests, runs fully offline.
LeenusDB	A crash-safe LSM key-value store in Go, stdlib only. CRC32C write-ahead log, memtable → SSTable → compaction, deterministic recovery. A chaos suite induces kill -9 mid-write, torn writes and on-disk bit flips, and proves zero acknowledged-data loss.
JosephinML	Churn prediction as a production ML system — the model is deliberately the smallest piece. Versioned registry, train/serve-skew defence, shadow + deterministic canary routing, PSI drift monitoring, a leakage audit that blocks registration, one-call rollback.
Also building, and being prepared for public release: an agent platform on Temporal + LangGraph

MCP + Qdrant (tenant isolation in the repository layer, idempotent writes with compensations, guardrails that fail closed, a canonical eval suite), and ml-from-scratch — the ML stack with no frameworks: an autograd engine, an MLP it trains on XOR, a Transformer/GPT built on that same engine and trained end to end, and a BPE tokenizer.
Each repo documents its design tradeoffs and limitations honestly — the benchmarks are laptop numbers, not production traffic, and the READMEs say so. The interesting reading is the failure-mode tables, not the feature lists.

Working with: Python · FastAPI · Flask · LLM tool calling · RAG · MCP · LangGraph · FAISS/Qdrant · Redis · Apache Kafka · PostgreSQL · MySQL · Go · Docker · AWS (EC2, S3)

📫 lijoferdinand@gmail.com · LinkedIn
