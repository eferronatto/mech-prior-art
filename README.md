# mech-prior-art

> AI-assisted prior art search system for mechanical inventions, built on retrieval-augmented generation (RAG), agentic search flows, and rigorous evaluation over global patent corpora.

![status](https://img.shields.io/badge/status-early%20development-orange)
![license](https://img.shields.io/badge/license-MIT-blue)

---

## Overview

**mech-prior-art** is an open-source system for prior art search and patent analysis focused specifically on **mechanical inventions** — industrial machinery, packaging, consumer products with mechanical mechanisms, manufacturing equipment, and the wider mechanical engineering domain.

The mechanical patent space is large, commercially important, and notably underserved by modern AI tooling. Most AI-for-patents efforts target pharmaceutical, biotech, semiconductor and software inventions, where the corpus structure and economics favor early investment. This project addresses the gap.

## Why this project

There is a meaningful asymmetry between:

- The volume and economic weight of mechanical inventions worldwide
- The amount of attention these inventions receive from modern retrieval and agentic AI systems

Small and mid-sized patent practitioners, court-appointed experts, and academic researchers in mechanical domains cannot easily access the kind of high-end IP intelligence tooling available to large pharmaceutical or tech companies. mech-prior-art aims to make rigorous AI-assisted analysis available for this domain, openly.

## Status

🚧 **Active early development.** The project is in foundation phase. Infrastructure, ingestion, and extraction pipelines are being built. No usable end-user release yet.

Public progress is tracked in this repository and discussed in LinkedIn posts by the author.

## Planned architecture

```
[Global patent sources]
  ├─ Google Patents Public Dataset (BigQuery) — bulk text and metadata
  ├─ EPO Open Patent Services — patent families, legal status, citations
  └─ Lens.org Patent Search API (pending access request)

[Ingestion pipeline]
  → Family-aware deduplication
  → Multilingual structured extraction via Claude (Pydantic models)
  → Normalization to English

[Indexing]
  → pgvector (semantic search) + Postgres FTS (lexical / BM25)

[Retrieval]
  → Hybrid search (reciprocal rank fusion)
  → Reranking
  → Citation-grounded answer generation

[Agentic layer]
  → Multi-step search flows for prior art identification
  → Claim comparison across patent families
  → Continuous monitoring of IPC subclasses

[Evaluation]
  → Golden dataset of 100+ curated queries
  → Multi-metric evaluation (retrieval and generation quality)
  → Separately released as legal-tech-rag-eval-pt — a community contribution
  
[Exposure]
  → REST API (FastAPI)
  → Model Context Protocol (MCP) server for native agent integration
```

## Roadmap

The project is structured in six phases, each producing a public release on completion:

1. **Multi-source ingestion** — patent retrieval, family deduplication, structured extraction
2. **Baseline RAG** — embeddings, vector indexing, naive question answering
3. **Hybrid search and reranking** — production-grade retrieval with verifiable citations
4. **Agentic flows** — multi-step reasoning, claim comparison, prior art identification
5. **Evaluation** — full eval suite; separate open-source release of the evaluation framework
6. **Production and MCP exposure** — deployable service and MCP server

## Domain scope

Initial IPC subclasses covered:

| IPC | Description |
|---|---|
| **B65** | Conveying, packaging, storing (machines and containers) |
| **B07** | Separating solids from solids (sieving, sorting) |
| **B06** | Generating or transmitting mechanical vibrations |
| **A47** | Furniture (chairs, headrests, etc.) |
| **A46** | Brushware |
| **A43** | Footwear (manufacturing equipment) |
| **F16** | Engineering elements (gears, bearings, joints, transmissions) |
| **B23** | Machine tools |
| **B25** | Hand tools, portable power-driven tools |

Initial jurisdictions: US, EP, WIPO, BR, CN, JP, DE. Time window: 2005-2025.

## Tech stack (planned)

- **Language and runtime:** Python 3.12, managed with `uv`
- **LLM:** Anthropic Claude (Sonnet for extraction at scale, Opus for high-quality reasoning)
- **Embeddings:** Voyage AI or OpenAI text-embedding-3-large (selection after empirical comparison)
- **Reranking:** Cohere Rerank or Voyage Rerank
- **Vector store:** pgvector on PostgreSQL
- **Agent orchestration:** LangGraph
- **API:** FastAPI
- **MCP:** Model Context Protocol official SDK
- **Observability:** Phoenix or Braintrust

## Acknowledgments

This project stands on the shoulders of:

- The **patent transparency community** — the EPO Open Patent Services, the Google Patents Public Dataset team, USPTO PatentsView, WIPO, and Cambia / Lens.org for their work making global patent data accessible
- **Anthropic** for the Claude API
- The open-source maintainers of `pgvector`, `LangGraph`, `Pydantic`, `Polars`, and the wider Python AI engineering ecosystem
- The patent attorneys, examiners, and inventors whose work over decades produced the corpus this system reasons over

## About the author

**Eleandro Ferronatto de Souza** is a mechanical and mechatronic engineer trained at the University of São Paulo (USP) with an MBA in Strategic and Economic Business Management from Fundação Getúlio Vargas (FGV). He has served as a court-appointed expert witness (*perito judicial federal*) for Brazilian Federal Courts on patent infringement cases involving mechanical inventions and utility models. Previously, he founded and led an electronic energy meter manufacturer acquired by the Wasion Group in 2019, and served as Head of Product at a Brazilian fintech startup. He is a Brazilian and Italian citizen.

LinkedIn: [linkedin.com/in/eleandroferronatto](https://www.linkedin.com/in/eleandroferronatto/)

## License

MIT License — see [LICENSE](LICENSE) for details.

## Contact

For questions, collaboration interest, or feedback, open a GitHub issue or reach the author directly via LinkedIn or email.
