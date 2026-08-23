# AGENTS.md — DARS (Dynamic Automated RAG Solution)

DARS is a generic, domain-agnostic RAG engine: ingestion, Smart Router, retrieval, generation, evaluation. **Pluggable and traceable by design** (see the repo README).

> **Current status (2026-08):** DARS is developed as workspace packages (`packages/rag-core`, `rag-ingest`, `eval`, plus shared `contracts`/`infra`) inside the KajianQ monorepo per ADR-0005. KajianQ is the first consumer. This standalone repo becomes the home of DARS only when a second real consumer exists — do not maintain parallel implementation here until then.

Until the split, all DARS implementation work happens in the KajianQ repo, governed by `KajianQ/AGENTS.md` and the project skills `dars-pluggability` and `kajianq-traceability`.

## The standing contract (applies in any repo that hosts DARS code)

1. **Domain-agnostic, always.** Engine packages carry zero domain logic from any consumer. Domain concepts arrive as typed inputs from a domain pack (KajianQ's `kajianq-domain` is the reference example).
2. **Pluggable seams.** Pipeline stages are typed interfaces (`Router`, `Retriever`, `Assembler`, `Generator`, `Reviewer`, `Provider`, `RagStore`, `ObjectStore`). Implementations swap by configuration; vendors, stores, and stage models are never named outside adapters and config.
3. **Traceable, always.** Every pipeline run emits a structured trace: stage outputs, retrieved evidence with scores, model identity, tokens, latency, cost. Traces are persisted records, not logs — consumable by product UIs and eval harnesses.
4. **Pipeline stages communicate in-process.** No HTTP between stages; extraction to separate services only happens when a real second consumer demands it.

Any future documentation, spec, or template shipped from this repo must include an agent-instruction file (like `KajianQ/AGENTS.md`) carrying these four rules verbatim in spirit, so adopters' AI agents inherit the constraints.
