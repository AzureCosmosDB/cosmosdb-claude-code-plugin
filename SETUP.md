# Optional: Azure Cosmos DB MCP Toolkit Integration

> **This setup is optional and only required for advanced live Azure Cosmos DB integration via MCP. The plugin works out of the box without completing these steps.**

The agent, commands, and 73 best-practice rules are available immediately after install with no configuration.

For users who want to connect Claude Code to a live Azure Cosmos DB account, the optional [Azure Cosmos DB MCP Toolkit](https://github.com/AzureCosmosDB/MCPToolKit) integration adds the ability to query databases, discover schemas, search documents, and perform vector search directly from Claude Code.

## What MCP Adds

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

1. **Azure Cosmos DB account** — [Create one](https://learn.microsoft.com/azure/cosmos-db/nosql/quickstart-portal)
2. **Deployed MCP Toolkit** — Follow the [MCP Toolkit Quick Start](https://github.com/AzureCosmosDB/MCPToolKit#quick-start)
3. **Azure CLI** — [Install](https://learn.microsoft.com/cli/azure/install-azure-cli) (for JWT token generation)

## Step 1: Deploy the MCP Toolkit

```bash
git clone https://github.com/AzureCosmosDB/MCPToolKit.git
cd MCPToolKit

# Deploy infrastructure
azd up

# Deploy the MCP server application
.\scripts\Deploy-Cosmos-MCP-Toolkit.ps1 -ResourceGroup "YOUR-RESOURCE-GROUP"
```

After deployment, note the following values from `deployment-info.json` in the MCPToolKit output:
- **MCP Server URL** — the deployed container app URL (e.g., `https://YOUR-CONTAINER-APP.azurecontainerapps.io`)
- **Entra App Client ID** — the registered application ID for authentication

## Step 2: Get a JWT Token

```bash
# Login to Azure
az login

# Get the token using the Entra App Client ID from deployment-info.json
az account get-access-token \
  --resource "YOUR-ENTRA-APP-CLIENT-ID" \
  --query accessToken -o tsv
```

Replace `YOUR-ENTRA-APP-CLIENT-ID` with the actual client ID from your `deployment-info.json` file.

> **Note:** JWT tokens expire after approximately 1 hour. Re-run the command above to refresh.

## Step 3: Set Environment Variables

```bash
export COSMOSDB_MCP_SERVER_URL=https://YOUR-CONTAINER-APP.azurecontainerapps.io
export COSMOSDB_MCP_JWT_TOKEN=<your-bearer-token-from-step-2>
```

On Windows (PowerShell):

```powershell
$env:COSMOSDB_MCP_SERVER_URL = "https://YOUR-CONTAINER-APP.azurecontainerapps.io"
$env:COSMOSDB_MCP_JWT_TOKEN = "<your-bearer-token-from-step-2>"
```

## Step 4: Configure the MCP Server in Claude Code

Copy the example config file into place:

```bash
cp .mcp.example.json .mcp.json
```

The `.mcp.json` file references the environment variables you set in Step 3. Claude Code will read them automatically.

Alternatively, run the setup command in Claude Code:

```
/azure-cosmosdb:cosmos-setup
```

## Step 5: Verify the Connection

Start Claude Code and try:

```
List all databases in my Cosmos DB account
```

If the MCP server is configured correctly, Claude will use the `list_databases` tool to show your databases.

## Troubleshooting

| Problem | Solution |
|---------|----------|
| "MCP server not configured" | Ensure `.mcp.json` exists in your project root and environment variables are set |
| Connection timeout | Verify `COSMOSDB_MCP_SERVER_URL` is correct and the container app is running |
| 401 Unauthorized | Your JWT token has expired — regenerate it with the `az account get-access-token` command |
| 403 Forbidden | Ensure your Azure account has access to the Cosmos DB account and the Entra App |
| Tools not appearing | Restart Claude Code after adding `.mcp.json` |

## File Reference

- [`.mcp.example.json`](.mcp.example.json) — Example MCP configuration (copy to `.mcp.json` to activate)
- [`commands/cosmos-setup.md`](commands/cosmos-setup.md) — Interactive setup command for Claude Code
