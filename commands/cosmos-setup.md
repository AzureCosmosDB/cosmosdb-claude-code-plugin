---
description: Set up the optional Azure Cosmos DB MCP Toolkit connection for live database operations
---

# Cosmos DB MCP Setup

Help the user configure the optional Azure Cosmos DB MCP Toolkit connection.
This enables live database operations (queries, schema discovery, vector search)
but is not required — the plugin's skills, agent, and commands work without it.

For detailed instructions, see [SETUP.md](../SETUP.md).

## Steps

1. **Check prerequisites:**
   - Ask if they have an Azure Cosmos DB account. If not, direct them to: https://learn.microsoft.com/azure/cosmos-db/nosql/quickstart-portal
   - Ask if they have deployed the MCP Toolkit. If not, direct them to: https://github.com/AzureCosmosDB/MCPToolKit#quick-start

2. **Get connection details:**
   - Ask for their deployed MCP Toolkit URL (from `deployment-info.json` in the MCPToolKit output)
   - Ask for their Entra App Client ID (also from `deployment-info.json`)
   - Help them get a JWT token by running:
     ```
     az login
     az account get-access-token --resource "YOUR-ENTRA-APP-CLIENT-ID" --query accessToken -o tsv
     ```
   - The `--resource` value is the Entra App Client ID from the MCPToolKit deployment output

3. **Set environment variables:**
   - Guide them to set `COSMOSDB_MCP_SERVER_URL` to their toolkit URL
   - Guide them to set `COSMOSDB_MCP_JWT_TOKEN` to their JWT token
   - Note: JWT tokens expire after ~1 hour and need to be refreshed

4. **Create MCP config:**
   - Guide them to copy `.mcp.example.json` to `.mcp.json` in their project root
   - The config references the environment variables set above

5. **Verify connection:**
   - Restart Claude Code after adding `.mcp.json`
   - Try listing databases using the MCP tools
   - If connection fails, refer to troubleshooting in [SETUP.md](../SETUP.md)

$ARGUMENTS
