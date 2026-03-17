---
description: Regenerate best-practice skills from the upstream cosmosdb-agent-kit repository
---

# Sync Cosmos DB Best Practices Rules

Fetch the latest individual rule files from the [AzureCosmosDB/cosmosdb-agent-kit](https://github.com/AzureCosmosDB/cosmosdb-agent-kit) repository and update the local rule files.

## Steps

1. Fetch the file listing from the GitHub API:
   `https://api.github.com/repos/AzureCosmosDB/cosmosdb-agent-kit/contents/skills/cosmosdb-best-practices/rules`

2. For each `.md` file returned, download it from:
   `https://raw.githubusercontent.com/AzureCosmosDB/cosmosdb-agent-kit/main/skills/cosmosdb-best-practices/rules/{filename}`

3. Save each file to:
   `skills/cosmosdb-best-practices/rules/{filename}`

4. Compare files before and after. Report:
   - New rules added
   - Existing rules updated (content changed)
   - Rules removed upstream (exist locally but not in API response)

5. Update `skills/cosmosdb-best-practices/SKILL.md`:
   - Add any new rules to the appropriate category in the Rule Index section
   - Remove any rules that were deleted upstream
   - Update the total rule count in the description

6. Report a summary of what changed.

## Notes

- The source of truth is: `https://github.com/AzureCosmosDB/cosmosdb-agent-kit/tree/main/skills/cosmosdb-best-practices/rules`
- Rules are individual markdown files named by `{prefix}-{topic}.md` (e.g., `model-embed-related.md`, `sdk-singleton-client.md`)
- The prefix determines the category: `model-` → Data Modeling, `partition-` → Partition Key, `query-` → Query Optimization, `sdk-` → SDK, `index-` → Indexing, `throughput-` → Throughput, `global-` → Global Distribution, `monitoring-` → Monitoring, `pattern-` → Design Patterns, `vector-` → Vector Search
- Files starting with `_` (like `_sections.md`, `_template.md`) are metadata, not rules

$ARGUMENTS
