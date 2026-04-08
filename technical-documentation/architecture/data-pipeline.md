---
doc_area: technical-documentation/architecture
last_approver: wilhelm
last_updated: 2026-04-08 15:00:00+00:00
project: data-pipeline
sources: []
version: 1
---

## Decision: duckdb-vs-sqlite

We evaluated DuckDB and SQLite for the local Silver layer storage. We chose DuckDB because of its native vector similarity search extension (vss), columnar storage that suits our analytical query patterns, and significantly better performance on aggregations over large datasets. SQLite is simpler to operate but lacks vector search capabilities and does not perform well on the bulk INSERT and analytical SELECT patterns in our pipeline. DuckDB's Python integration is excellent and matches our existing tooling.

## Architecture: bronze-silver-gold-flow

The pipeline follows a medallion architecture. Bronze is raw markdown files on local filesystem with YAML frontmatter. Silver is DuckDB with two tables: silver_captures for incoming knowledge chunks and silver_docs for curated Gold document chunks. The Silver transformation involves heading-based chunking, sentence-transformers embedding, and HNSW indexed vector storage. Gold is Git-versioned markdown that serves as the human-approved source of truth. Agents consume Gold via MCP tools backed by Silver vector search.
