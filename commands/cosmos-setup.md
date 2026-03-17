---
description: Set up the Azure Cosmos DB MCP Toolkit connection for this plugin
---

# Cosmos DB MCP Setup

Help the user configure their Azure Cosmos DB MCP Toolkit connection.

## Steps

1. **Check prerequisites:**
   - Ask if they have an Azure Cosmos DB account. If not, direct them to: https://learn.microsoft.com/azure/cosmos-db/nosql/quickstart-portal
   - Ask if they have deployed the MCP Toolkit. If not, direct them to: https://github.com/AzureCosmosDB/MCPToolKit#quick-start

2. **Get connection details:**
   - Ask for their deployed MCP Toolkit URL (from deployment-info.json)
   - Help them get a JWT token by running:
     ```
     az login
     az account get-access-token --resource <ENTRA-APP-CLIENT-ID> --query accessToken -o tsv
     ```

3. **Set environment variables:**
   - Guide them to set `COSMOSDB_MCP_SERVER_URL` to their toolkit URL
   - Guide them to set `COSMOSDB_MCP_JWT_TOKEN` to their JWT token
   - Note: JWT tokens expire after ~1 hour and need to be refreshed

4. **Verify connection:**
   - Try listing databases using the MCP tools
   - If connection fails, help troubleshoot common issues

$ARGUMENTS
