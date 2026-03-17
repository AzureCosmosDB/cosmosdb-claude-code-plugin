---
description: Review code for Azure Cosmos DB best practices and suggest optimizations
---

# Cosmos DB Code Review

Review the user's code against Azure Cosmos DB best practices. Use the cosmosdb-best-practices skill as your reference.

## Review Checklist (in priority order)

### CRITICAL
- [ ] Data model design (embedding vs referencing, document size)
- [ ] Partition key choice (cardinality, query alignment, hot partitions)
- [ ] CosmosClient lifecycle (singleton pattern)

### HIGH
- [ ] Query optimization (cross-partition, projections, pagination)
- [ ] SDK patterns (async, connection mode, retry configuration)
- [ ] Concurrency control (ETags for read-modify-write)
- [ ] Design patterns (Change Feed, materialized views)

### MEDIUM
- [ ] Indexing strategy (exclude unused paths, composite indexes)
- [ ] Throughput configuration (autoscale vs fixed, right-sizing)
- [ ] Global distribution (preferred regions, consistency level)

### LOW-MEDIUM
- [ ] Monitoring (diagnostics logging, RU tracking, alerts)

## Output Format

For each finding:
1. **Rule**: Which best practice rule applies
2. **Severity**: CRITICAL / HIGH / MEDIUM / LOW
3. **Current code**: What the code does now
4. **Issue**: Why it's problematic (with RU/latency/scalability impact)
5. **Fix**: Corrected code example

$ARGUMENTS
