# Azure Cosmos DB Plugin for Claude Code

The Azure Cosmos DB plugin for [Claude Code](https://code.claude.com/) gives Claude the tools and skills needed to work effectively with Azure Cosmos DB projects.

## What's Included

- **MCP Server** — Connection to the [Azure Cosmos DB MCP Toolkit](https://github.com/AzureCosmosDB/MCPToolKit) for database operations, queries, vector search, and schema discovery
- **Skills** — 60+ best-practice rules across 9 categories covering data modeling, partition key design, query optimization, SDK patterns, indexing, throughput, global distribution, monitoring, and design patterns — sourced from the [cosmosdb-agent-kit](https://github.com/AzureCosmosDB/cosmosdb-agent-kit)
- **Agent** — A specialized Cosmos DB expert agent for in-depth guidance
- **Commands** — Setup, code review, and skill regeneration commands

## MCP Tools Available

Once connected to the MCP Toolkit, the following tools are available:

| Tool | Description |
|------|-------------|
| `list_databases` | List all databases in the Cosmos DB account |
| `list_collections` | List all containers in a database |
| `get_approximate_schema` | Sample documents to infer schema (top-level properties) |
| `get_recent_documents` | Get N most recent documents ordered by timestamp |
| `find_document_by_id` | Find a document by its id |
| `text_search` | Search for documents where a property contains a search phrase |
| `vector_search` | Perform vector search using Azure OpenAI embeddings |

## Prerequisites

To use the MCP server features, you need:

1. **Azure Cosmos DB account** — [Create one](https://learn.microsoft.com/azure/cosmos-db/nosql/quickstart-portal)
2. **Deployed MCP Toolkit** — Follow the [MCPToolKit deployment guide](https://github.com/AzureCosmosDB/MCPToolKit#quick-start)
3. **JWT Bearer Token** — For authentication (see setup below)

The skills and agent work without any setup — they provide best practices guidance regardless of MCP server configuration.

## Setup

### Step 1: Deploy the MCP Toolkit

Follow the [Azure Cosmos DB MCP Toolkit Quick Start](https://github.com/AzureCosmosDB/MCPToolKit#quick-start):

```bash
git clone https://github.com/AzureCosmosDB/MCPToolKit.git
cd MCPToolKit

# Deploy infrastructure
azd up

# Deploy the MCP server application
.\scripts\Deploy-Cosmos-MCP-Toolkit.ps1 -ResourceGroup "YOUR-RESOURCE-GROUP"
```

### Step 2: Configure Environment Variables

Set these environment variables so the MCP server can connect:

```bash
# Your deployed MCP Toolkit URL (from deployment-info.json)
COSMOSDB_MCP_SERVER_URL=https://YOUR-CONTAINER-APP.azurecontainerapps.io

# JWT token for authentication
COSMOSDB_MCP_JWT_TOKEN=<your-bearer-token>
```

#### Getting Your JWT Token

```bash
# Login to Azure
az login

# Get the Entra App Client ID (from deployment-info.json)
az account get-access-token --resource YOUR-ENTRA-APP-CLIENT-ID --query accessToken -o tsv
```

> **Note:** JWT tokens expire after ~1 hour. Refresh by re-running the command above.

### Step 3: Test Locally

Load the plugin directly from this directory:

```bash
claude --plugin-dir ./
```

Then try:

```
/azure-cosmosdb:cosmos-setup
```

Or just ask Claude:

```
List all databases in my Cosmos DB account
```

### Step 4: Verify

In Claude Code, try these commands:

```
/azure-cosmosdb:cosmos-review     # Review your code for Cosmos DB best practices
/azure-cosmosdb:cosmos-setup      # Configure MCP Toolkit connection
/azure-cosmosdb:generate-skills   # Regenerate skills from upstream agent-kit
```

Or use the agent:

```
/agents cosmosdb-expert
```

## Project Structure

```
cosmosdb-claude-plugin/
├── .claude-plugin/
│   └── plugin.json                 # Plugin manifest
├── .mcp.json                       # MCP server configuration
├── agents/
│   └── cosmosdb-expert.md          # Specialized Cosmos DB agent
├── commands/
│   ├── cosmos-setup.md             # MCP Toolkit setup command
│   ├── cosmos-review.md            # Code review command
│   └── generate-skills.md          # Regenerate skills from agent-kit
├── skills/
│   └── cosmosdb-best-practices/
│       ├── SKILL.md                # Main skill (triggers activation)
│       ├── data-modeling.md        # Data modeling rules
│       ├── partition-key.md        # Partition key design rules
│       ├── query-optimization.md   # Query optimization rules
│       ├── sdk-patterns.md         # SDK best practices
│       └── indexing-throughput-global-monitoring-patterns.md
├── assets/
│   └── logo.svg                    # Plugin logo
├── LICENSE
└── README.md
```

## Keeping Skills Up to Date

Skills are derived from the [AzureCosmosDB/cosmosdb-agent-kit](https://github.com/AzureCosmosDB/cosmosdb-agent-kit) skills.

To regenerate skills after the upstream agent-kit updates:

1. Run the regeneration command in Claude Code:
   ```
   /azure-cosmosdb:generate-skills
   ```
2. This fetches the latest skills from the agent-kit repo and updates local files

## Usage Examples

### With MCP Server (requires deployment)

- "List all databases in my Cosmos DB account"
- "Show me the schema of the `users` container in the `mydb` database"
- "Get the latest 10 documents from the `orders` container"
- "Search for documents where the name contains 'Azure'"
- "Find the document with id 'user-001' in the `users` container"

### With Skills (no setup needed)

- "Review my Cosmos DB data model for this application"
- "What partition key should I use for a multi-tenant SaaS app?"
- "Optimize this Cosmos DB query for lower RU cost"
- "Should I embed or reference this related data?"
- "Review my Cosmos DB code for best practices"

### With Agent

- `/agents azure-cosmosdb:cosmosdb-expert` — Start a conversation with the Cosmos DB expert

## Submitting to Claude Code Marketplace

To submit this plugin to the official Anthropic marketplace:

1. **Claude.ai**: Go to [claude.ai/settings/plugins/submit](https://claude.ai/settings/plugins/submit)
2. **Console**: Go to [platform.claude.com/plugins/submit](https://platform.claude.com/plugins/submit)

Provide the GitHub repository URL and plugin details from `plugin.json`.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch: `git checkout -b feature/my-feature`
3. Commit your changes: `git commit -m 'feat: add my feature'`
4. Push to the branch: `git push origin feature/my-feature`
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
