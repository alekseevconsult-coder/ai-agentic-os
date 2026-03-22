# 🔌 MCP Servers Setup

MCP (Model Context Protocol) servers give your AI agents direct access to tools — databases, GitHub, web browsing, and more.

## Available MCP Servers

### 1. PostgreSQL MCP (Most Useful!)
Lets Claude/Grok/Gemini query your KeyCRM analytics database in plain English.

```bash
# Add to your docker-compose.yml or run standalone:
docker run -d \
  --name mcp-postgres \
  --network ai-agentic-os_agentic-os-net \
  -e POSTGRES_CONNECTION_STRING="postgresql://agenticadmin:PASSWORD@postgres:5432/analytics" \
  mcp/postgres:latest
```

### 2. Filesystem MCP
Lets AI read/write files on your server.

### 3. GitHub MCP
Lets AI manage your GitHub repos, issues, PRs from chat.

## Configure in Open WebUI

1. Open WebUI → Settings → Tools → Add MCP Server
2. URL: `http://mcp-postgres:3000`
3. Name: `KeyCRM Analytics DB`
4. Save and enable

## Configure in LibreChat

Add to `config/librechat/librechat.yaml`:

```yaml
mcpServers:
  postgres-analytics:
    command: npx
    args:
      - "-y"
      - "@modelcontextprotocol/server-postgres"
      - "postgresql://agenticadmin:PASSWORD@postgres:5432/analytics"
```

Then restart LibreChat:
```bash
docker compose restart librechat
```

## Example Queries After MCP Setup

In Open WebUI or LibreChat chat:
- "What were my top 10 SKUs by revenue last month?"
- "How many new customers did I acquire this week?"
- "Show me all orders with status 'pending' older than 7 days"
- "What's my average order value by source channel?"
- "Compare revenue this month vs last month"

## Full MCP Server List

See the awesome-mcp-servers list for 500+ available servers:
https://github.com/punkpeye/awesome-mcp-servers
