---
name: cosmosdb-best-practices
license: MIT
description: |
  Azure Cosmos DB performance optimization and best practices guidelines for NoSQL,
  partitioning, queries, SDK usage, and vector search. Use when writing, reviewing,
  or refactoring code that interacts with Azure Cosmos DB, designing data models,
  optimizing queries, or implementing high-performance database operations.
metadata:
  author: cosmosdb-agent-kit
  version: "1.0.0"
---

# Azure Cosmos DB Best Practices

Comprehensive performance optimization guide for Azure Cosmos DB applications, containing
127 rules across 13 categories, prioritized by impact to guide automated
refactoring and code generation.

127 individual rule files are in the [rules/](./rules/) directory, one file per rule,
synced from the [AzureCosmosDB/cosmosdb-agent-kit](https://github.com/AzureCosmosDB/cosmosdb-agent-kit).
Load only the relevant rule file(s) when answering a question — do NOT load all files at once.
Run `/azure-cosmos-db-assistant:generate-skills` to sync with the latest rules from the agent-kit.

## When to Apply

Reference these guidelines when:
- Designing data models for Cosmos DB
- Choosing partition keys
- Writing or optimizing queries
- Implementing SDK patterns
- Reviewing code for performance issues
- Configuring throughput and scaling
- Building globally distributed applications
- Implementing vector search and RAG patterns

## Rule Index

Rules are grouped by category prefix. For a given question, load files matching
the relevant prefix (e.g., `model-*.md` for data modeling, `sdk-*.md` for SDK usage).
See [rules/_sections.md](rules/_sections.md) for category descriptions.

### 1. Data Modeling — CRITICAL (prefix: `model-`)

- [model-embed-related.md](rules/model-embed-related.md) — Embed related data retrieved together
- [model-reference-large.md](rules/model-reference-large.md) — Reference data when items get too large
- [model-avoid-2mb-limit.md](rules/model-avoid-2mb-limit.md) — Keep items well under 2MB limit
- [model-id-constraints.md](rules/model-id-constraints.md) — Follow ID value length and character constraints
- [model-nesting-depth.md](rules/model-nesting-depth.md) — Stay within 128-level nesting depth limit
- [model-numeric-precision.md](rules/model-numeric-precision.md) — Understand IEEE 754 numeric precision limits
- [model-denormalize-reads.md](rules/model-denormalize-reads.md) — Denormalize for read-heavy workloads including pre-computed aggregates
- [model-schema-versioning.md](rules/model-schema-versioning.md) — Version your document schemas
- [model-type-discriminator.md](rules/model-type-discriminator.md) — Use type discriminators for polymorphic data
- [model-json-serialization.md](rules/model-json-serialization.md) — Handle JSON serialization correctly for Cosmos DB documents
- [model-relationship-references.md](rules/model-relationship-references.md) — Use ID references with transient hydration for document relationships

### 2. Partition Key Design — CRITICAL (prefix: `partition-`)

- [partition-high-cardinality.md](rules/partition-high-cardinality.md) — Choose high-cardinality partition keys
- [partition-avoid-hotspots.md](rules/partition-avoid-hotspots.md) — Distribute writes evenly
- [partition-hierarchical.md](rules/partition-hierarchical.md) — Use hierarchical partition keys for flexibility; order levels broad→narrow
- [partition-query-patterns.md](rules/partition-query-patterns.md) — Align partition key with query patterns
- [partition-synthetic-keys.md](rules/partition-synthetic-keys.md) — Create synthetic keys when needed
- [partition-key-length.md](rules/partition-key-length.md) — Respect partition key value length limits
- [partition-immutable-key.md](rules/partition-immutable-key.md) — Choose immutable properties as partition keys
- [partition-20gb-limit.md](rules/partition-20gb-limit.md) — Plan for 20GB logical partition limit

### 3. Query Optimization — HIGH (prefix: `query-`)

- [query-aggregate-single-pass.md](rules/query-aggregate-single-pass.md) — Compute min/max/avg with one scoped aggregate query
- [query-avoid-cross-partition.md](rules/query-avoid-cross-partition.md) — Minimize cross-partition queries
- [query-use-projections.md](rules/query-use-projections.md) — Project only needed fields; prefer dedicated result types for projections
- [query-pagination.md](rules/query-pagination.md) — Use continuation tokens for pagination
- [query-avoid-scans.md](rules/query-avoid-scans.md) — Avoid full container scans
- [query-parameterize.md](rules/query-parameterize.md) — Use parameterized queries
- [query-order-filters.md](rules/query-order-filters.md) — Order filters by selectivity
- [query-top-literal.md](rules/query-top-literal.md) — Use literal integers for TOP, never parameters
- [query-latest-by-timestamp.md](rules/query-latest-by-timestamp.md) — Query "latest" documents with explicit ORDER BY and TOP 1
- [query-olap-detection.md](rules/query-olap-detection.md) — Detect and redirect analytical queries away from transactional containers
- [query-point-reads.md](rules/query-point-reads.md) — Use point reads (ReadItem) instead of queries when id and partition key are known
- [query-distinct-keyword.md](rules/query-distinct-keyword.md) — Use DISTINCT keyword to eliminate duplicate results efficiently

### 4. SDK Best Practices — HIGH (prefix: `sdk-`)

- [sdk-singleton-client.md](rules/sdk-singleton-client.md) — Reuse CosmosClient as singleton
- [sdk-async-api.md](rules/sdk-async-api.md) — Use async APIs for throughput
- [sdk-retry-429.md](rules/sdk-retry-429.md) — Handle 429s with retry-after
- [sdk-connection-mode.md](rules/sdk-connection-mode.md) — Use Direct mode for production
- [sdk-preferred-regions.md](rules/sdk-preferred-regions.md) — Configure preferred regions
- [sdk-excluded-regions.md](rules/sdk-excluded-regions.md) — Exclude regions experiencing issues
- [sdk-availability-strategy.md](rules/sdk-availability-strategy.md) — Configure availability strategy for resilience
- [sdk-circuit-breaker.md](rules/sdk-circuit-breaker.md) — Use circuit breaker for fault tolerance
- [sdk-diagnostics.md](rules/sdk-diagnostics.md) — Log diagnostics for troubleshooting
- [sdk-serialization-enums.md](rules/sdk-serialization-enums.md) — Serialize enums as strings not integers
- [sdk-emulator-ssl.md](rules/sdk-emulator-ssl.md) — Configure SSL and connection mode for Cosmos DB Emulator
- [sdk-conditional-create-etag.md](rules/sdk-conditional-create-etag.md) — Use `setIfNoneMatchETag("*")` on `createItem` to reject duplicates atomically (409 on conflict)
- [sdk-request-options-per-call.md](rules/sdk-request-options-per-call.md) — Never reuse a `CosmosItemRequestOptions` instance across multiple `createItem` calls
- [sdk-patch-counter-increment.md](rules/sdk-patch-counter-increment.md) — Use `CosmosPatchOperations.incr()` for atomic counter increments
- [sdk-continuation-token-null-guard.md](rules/sdk-continuation-token-null-guard.md) — Guard against empty-string continuation tokens before calling `byPage()`
- [sdk-etag-concurrency.md](rules/sdk-etag-concurrency.md) — Use ETags for optimistic concurrency on read-modify-write operations
- [sdk-java-content-response.md](rules/sdk-java-content-response.md) — Enable content response on write operations (Java)
- [sdk-java-cosmos-config.md](rules/sdk-java-cosmos-config.md) — Configure Cosmos DB initialization correctly in Spring Boot
- [sdk-java-spring-boot-versions.md](rules/sdk-java-spring-boot-versions.md) — Match Java version to Spring Boot requirements
- [sdk-local-dev-config.md](rules/sdk-local-dev-config.md) — Configure local development to avoid cloud conflicts
- [sdk-dotnet-cosmos-package-id.md](rules/sdk-dotnet-cosmos-package-id.md) — Use `Microsoft.Azure.Cosmos`, not the abandoned `Azure.Cosmos` v4-preview package
- [sdk-newtonsoft-dependency.md](rules/sdk-newtonsoft-dependency.md) — Explicitly reference Newtonsoft.Json package (.NET)
- [sdk-python-async-deps.md](rules/sdk-python-async-deps.md) — Include aiohttp when using Python async SDK
- [sdk-spring-data-annotations.md](rules/sdk-spring-data-annotations.md) — Annotate entities for Spring Data Cosmos
- [sdk-spring-data-repository.md](rules/sdk-spring-data-repository.md) — Use CosmosRepository correctly and handle Iterable return types
- [sdk-langchain-cosmosdb-saver.md](rules/sdk-langchain-cosmosdb-saver.md) — Use CosmosDBSaver for LangGraph checkpointing with async container client
- [sdk-langchain-async-checkpointer.md](rules/sdk-langchain-async-checkpointer.md) — Initialize async Cosmos DB container in startup routine, not module level
- [sdk-langchain-js-chat-history.md](rules/sdk-langchain-js-chat-history.md) — Persist chat history with the LangChain.js Cosmos DB message history
- [sdk-langchain-js-embedding-model.md](rules/sdk-langchain-js-embedding-model.md) — Configure the embedding model for LangChain.js vector stores
- [sdk-langchain-js-filter-injection.md](rules/sdk-langchain-js-filter-injection.md) — Avoid filter injection in LangChain.js vector search
- [sdk-langchain-js-fulltext-prerequisites.md](rules/sdk-langchain-js-fulltext-prerequisites.md) — Meet full-text search prerequisites for LangChain.js
- [sdk-langchain-js-managed-identity.md](rules/sdk-langchain-js-managed-identity.md) — Use managed identity with the LangChain.js Cosmos DB integration
- [sdk-langchain-js-search-types.md](rules/sdk-langchain-js-search-types.md) — Choose the correct search type for LangChain.js vector stores
- [sdk-langchain-js-semantic-cache.md](rules/sdk-langchain-js-semantic-cache.md) — Configure the LangChain.js Cosmos DB semantic cache
- [sdk-langchain-js-vectorstore-init.md](rules/sdk-langchain-js-vectorstore-init.md) — Initialize the LangChain.js Cosmos DB vector store correctly
- [sdk-langchain-mcp-persistent-session.md](rules/sdk-langchain-mcp-persistent-session.md) — Maintain persistent MCP client sessions for application lifetime
- [sdk-langchain-mcp-tool-content-format.md](rules/sdk-langchain-mcp-tool-content-format.md) — Handle both string and list formats in MCP ToolMessage content
- [sdk-langgraph-mcp-tool-filtering.md](rules/sdk-langgraph-mcp-tool-filtering.md) — Filter MCP tools by name prefix for per-agent assignment
- [sdk-go-partition-key-metadata.md](rules/sdk-go-partition-key-metadata.md) — Set the partition key from item metadata in the Go SDK
- [sdk-dotnet-namespace-collision.md](rules/sdk-dotnet-namespace-collision.md) — Avoid `Microsoft.Azure.Cosmos` namespace collisions with domain models

### 5. Indexing Strategies — MEDIUM-HIGH (prefix: `index-`)

- [index-exclude-unused.md](rules/index-exclude-unused.md) — Exclude paths never queried
- [index-path-syntax.md](rules/index-path-syntax.md) — Use correct path notation (`/?`, `/[]`, `/*`)
- [index-composite.md](rules/index-composite.md) — Use composite indexes for ORDER BY
- [index-composite-direction.md](rules/index-composite-direction.md) — Match composite index directions to ORDER BY
- [index-spatial.md](rules/index-spatial.md) — Add spatial indexes for geo queries
- [index-range-vs-hash.md](rules/index-range-vs-hash.md) — Choose appropriate index types
- [index-lazy-consistent.md](rules/index-lazy-consistent.md) — Understand indexing modes

### 6. Throughput & Scaling — MEDIUM (prefix: `throughput-`)

- [throughput-autoscale.md](rules/throughput-autoscale.md) — Use autoscale for variable workloads
- [throughput-right-size.md](rules/throughput-right-size.md) — Right-size provisioned throughput
- [throughput-serverless.md](rules/throughput-serverless.md) — Consider serverless for dev/test
- [throughput-burst.md](rules/throughput-burst.md) — Understand burst capacity
- [throughput-container-vs-database.md](rules/throughput-container-vs-database.md) — Choose allocation level wisely

### 7. Global Distribution — MEDIUM (prefix: `global-`)

- [global-multi-region.md](rules/global-multi-region.md) — Configure multi-region writes
- [global-consistency.md](rules/global-consistency.md) — Choose appropriate consistency level
- [global-conflict-resolution.md](rules/global-conflict-resolution.md) — Implement conflict resolution
- [global-failover.md](rules/global-failover.md) — Configure automatic failover
- [global-read-regions.md](rules/global-read-regions.md) — Add read regions near users
- [global-zone-redundancy.md](rules/global-zone-redundancy.md) — Enable zone redundancy for HA

### 8. Monitoring & Diagnostics — LOW-MEDIUM (prefix: `monitoring-`)

- [monitoring-ru-consumption.md](rules/monitoring-ru-consumption.md) — Track RU consumption
- [monitoring-latency.md](rules/monitoring-latency.md) — Monitor P99 latency
- [monitoring-throttling.md](rules/monitoring-throttling.md) — Alert on throttling
- [monitoring-azure-monitor.md](rules/monitoring-azure-monitor.md) — Integrate Azure Monitor
- [monitoring-diagnostic-logs.md](rules/monitoring-diagnostic-logs.md) — Enable diagnostic logging

### 9. Design Patterns — HIGH (prefix: `pattern-`)

- [pattern-change-feed-materialized-views.md](rules/pattern-change-feed-materialized-views.md) — Use Change Feed for cross-partition query optimization
- [pattern-efficient-ranking.md](rules/pattern-efficient-ranking.md) — Use count-based or cached approaches for efficient ranking
- [pattern-service-layer-relationships.md](rules/pattern-service-layer-relationships.md) — Use a service layer to hydrate document references
- [pattern-ai-grounding-access.md](rules/pattern-ai-grounding-access.md) — Enforce access control when grounding AI responses on Cosmos DB data
- [pattern-background-task-writes.md](rules/pattern-background-task-writes.md) — Use FastAPI background tasks for non-blocking chat history writes
- [pattern-langgraph-multi-agent.md](rules/pattern-langgraph-multi-agent.md) — Use StateGraph with conditional edges for multi-agent routing
- [pattern-langgraph-interrupt-human.md](rules/pattern-langgraph-interrupt-human.md) — Use LangGraph interrupt for human-in-the-loop confirmation flows
- [pattern-langgraph-resume-checkpoint.md](rules/pattern-langgraph-resume-checkpoint.md) — Resume LangGraph from checkpoint after interrupt for multi-turn conversations
- [pattern-langgraph-agent-routing-cosmosdb.md](rules/pattern-langgraph-agent-routing-cosmosdb.md) — Persist active agent in Cosmos DB for deterministic routing via point reads
- [pattern-langgraph-fastapi-startup.md](rules/pattern-langgraph-fastapi-startup.md) — Initialize LangGraph agents in FastAPI startup with retry logic
- [pattern-langgraph-chat-history-separate.md](rules/pattern-langgraph-chat-history-separate.md) — Store chat history in a dedicated container, not the checkpointer
- [pattern-langgraph-async-cosmos-routing.md](rules/pattern-langgraph-async-cosmos-routing.md) — Wrap Cosmos DB sync calls in asyncio.to_thread for LangGraph routing functions
- [pattern-langgraph-async-cosmos-writes.md](rules/pattern-langgraph-async-cosmos-writes.md) — Use asyncio.to_thread for active agent writes in async node functions
- [pattern-langgraph-agent-name-attribution.md](rules/pattern-langgraph-agent-name-attribution.md) — Tag AI messages with agent name for API response attribution

### 10. Developer Tooling — MEDIUM (prefix: `tooling-`)

- [tooling-vscode-extension.md](rules/tooling-vscode-extension.md) — Use the VS Code extension for routine inspection and management
- [tooling-emulator-setup.md](rules/tooling-emulator-setup.md) — Use the Emulator for local development and testing

### 11. Vector Search — HIGH (prefix: `vector-`)

- [vector-enable-feature.md](rules/vector-enable-feature.md) — Enable vector search on the account before using vector features
- [vector-embedding-policy.md](rules/vector-embedding-policy.md) — Define vector embedding policy for vector properties
- [vector-index-type.md](rules/vector-index-type.md) — Configure vector indexes in the indexing policy
- [vector-normalize-embeddings.md](rules/vector-normalize-embeddings.md) — Normalize embeddings for cosine similarity
- [vector-distance-query.md](rules/vector-distance-query.md) — Use VectorDistance for similarity search
- [vector-repository-pattern.md](rules/vector-repository-pattern.md) — Implement a repository pattern for vector search

### 12. Full-Text Search — HIGH (prefix: `fts-`)

- [fts-enable-capability.md](rules/fts-enable-capability.md) — Enable `EnableNoSQLFullTextSearch` capability on the account
- [fts-define-policy.md](rules/fts-define-policy.md) — Define `fullTextPolicy` on the container with correct language code
- [fts-add-index.md](rules/fts-add-index.md) — Add `fullTextIndexes` entry in the indexing policy to build the inverted index
- [fts-keyword-matching.md](rules/fts-keyword-matching.md) — Use `FullTextContains` / `FullTextContainsAll` / `FullTextContainsAny` instead of `CONTAINS(LOWER(...))`
- [fts-relevance-ranking.md](rules/fts-relevance-ranking.md) — Use `ORDER BY RANK FullTextScore(path, term)` for BM25 relevance ranking
- [fts-hybrid-queries.md](rules/fts-hybrid-queries.md) — Combine FTS predicates with range/equality filters; put most selective filter first

### 13. Security — HIGH (prefix: `security-`)

- [security-managed-identity.md](rules/security-managed-identity.md) — Use managed identity with DefaultAzureCredential
- [security-disable-local-auth.md](rules/security-disable-local-auth.md) — Disable local authentication (keys)
- [security-network-restrict.md](rules/security-network-restrict.md) — Restrict network access
- [security-rbac-least-privilege.md](rules/security-rbac-least-privilege.md) — Assign minimum RBAC roles with narrow scope
- [security-continuous-backup.md](rules/security-continuous-backup.md) — Enable continuous backup for point-in-time restore

## How to Use

Each rule is a separate file under [rules/](./rules/). When answering a Cosmos DB question,
read only the relevant rule file(s) based on the prefix matching the topic:

- Data modeling question → read `rules/model-*.md` files
- Partition key question → read `rules/partition-*.md` files
- SDK/client question → read `rules/sdk-*.md` files
- etc.

Each rule file contains:
- Brief explanation of why it matters
- Incorrect code example with explanation
- Correct code example with explanation
- Additional context and references

Source: [AzureCosmosDB/cosmosdb-agent-kit](https://github.com/AzureCosmosDB/cosmosdb-agent-kit/tree/main/skills/cosmosdb-best-practices/rules)
