# Azure Cosmos DB Plugin for Claude Code

The Azure Cosmos DB plugin for [Claude Code](https://docs.anthropic.com/en/docs/claude-code) gives Claude deep expertise in Azure Cosmos DB best practices, data modeling, query optimization, and SDK usage — ready to use immediately after install with no configuration required.

## Works Out of the Box

This plugin installs cleanly with **no required environment variables, no external services, and no auto-running binaries**. All skills, the expert agent, and commands are available immediately.

## What's Included

- **Skills** — 73 best-practice rules across 10 categories covering data modeling, partition key design, query optimization, SDK patterns, indexing, throughput, global distribution, monitoring, design patterns, and vector search — sourced from the [cosmosdb-agent-kit](https://github.com/AzureCosmosDB/cosmosdb-agent-kit)
- **Agent** — A specialized Cosmos DB expert agent for in-depth guidance
- **Commands** — Code review and skill regeneration commands

## Optional: MCP Toolkit Integration

For users who want to connect Claude Code to a **live Azure Cosmos DB account**, the optional [Azure Cosmos DB MCP Toolkit](https://github.com/AzureCosmosDB/MCPToolKit) integration adds database operations, schema discovery, and vector search. This requires a separate deployment and is **not required** for the plugin to function.

See [SETUP.md](SETUP.md) for full integration instructions.

## Getting Started

After installing the plugin, try these in Claude Code:

### Commands

```
/azure-cosmosdb:cosmos-review     # Review your code for Cosmos DB best practices
/azure-cosmosdb:generate-skills   # Regenerate skills from upstream agent-kit
/azure-cosmosdb:cosmos-setup      # Configure optional MCP Toolkit connection
```

### Agent

```
/agents cosmosdb-expert
```

### Example Questions

- "Review my Cosmos DB data model for this application"
- "What partition key should I use for a multi-tenant SaaS app?"
- "Optimize this Cosmos DB query for lower RU cost"
- "Should I embed or reference this related data?"
- "Review my Cosmos DB code for best practices"
- "How do I set up vector search in Cosmos DB for RAG?"

## Project Structure

```
cosmosdb-claude-code-plugin/
├── .claude-plugin/
│   └── plugin.json                 # Plugin manifest
├── .mcp.example.json               # Example MCP config (copy to .mcp.json to activate)
├── agents/
│   └── cosmosdb-expert.md          # Specialized Cosmos DB expert agent
├── commands/
│   ├── cosmos-setup.md             # Optional MCP Toolkit setup command
│   ├── cosmos-review.md            # Code review command
│   └── generate-skills.md          # Regenerate skills from agent-kit
├── skills/
│   └── cosmosdb-best-practices/
│       ├── SKILL.md                # Skill manifest and rule index
│       └── rules/                  # 73 individual best-practice rule files
│           ├── model-*.md          # Data modeling (11 rules)
│           ├── partition-*.md      # Partition key design (7 rules)
│           ├── query-*.md          # Query optimization (6 rules)
│           ├── sdk-*.md            # SDK best practices (19 rules)
│           ├── index-*.md          # Indexing strategies (5 rules)
│           ├── throughput-*.md     # Throughput & scaling (5 rules)
│           ├── global-*.md         # Global distribution (6 rules)
│           ├── monitoring-*.md     # Monitoring & diagnostics (5 rules)
│           ├── pattern-*.md        # Design patterns (3 rules)
│           └── vector-*.md         # Vector search (6 rules)
├── assets/
│   └── logo.svg                    # Plugin logo
├── SETUP.md                        # Optional MCP Toolkit integration guide
├── LICENSE
└── README.md
```

## Keeping Skills Up to Date

Skills are derived from the [AzureCosmosDB/cosmosdb-agent-kit](https://github.com/AzureCosmosDB/cosmosdb-agent-kit).

To regenerate skills after the upstream agent-kit updates:

```
/azure-cosmosdb:generate-skills
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch: `git checkout -b feature/my-feature`
3. Commit your changes: `git commit -m 'feat: add my feature'`
4. Push to the branch: `git push origin feature/my-feature`
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
