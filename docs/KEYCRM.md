# KeyCRM Integration Guide

## Overview

Activepieces syncs your KeyCRM data to PostgreSQL every 4 hours.
Metabase reads from PostgreSQL to show dashboards.
Open WebUI (via MCP) lets you query data in plain English.

```
KeyCRM API
    ↓ (Activepieces sync every 4h)
PostgreSQL analytics DB
    ↓                    ↓
Metabase           Open WebUI MCP
(Dashboards)       (AI queries)
```

## Step 1: Get Your KeyCRM API Key

1. Log in to KeyCRM
2. Settings → Integrations → API
3. Generate Token
4. Copy the token

## Step 2: Add to .env

```bash
KEYCRM_API_KEY=your_token_here
KEYCRM_BASE_URL=https://openapi.keycrm.app/v1
```

Then restart: `docker compose restart activepieces`

## Step 3: Import Activepieces Flow

1. Open Activepieces: `https://auto.yourdomain.com`
2. Sign up / log in
3. Flows → Import → select `activepieces/keycrm-sync-template.json`
4. Edit the flow → set your PostgreSQL connection
5. Enable the flow

### PostgreSQL connection in Activepieces:

- Host: `postgres`
- Port: `5432`
- Database: `analytics`
- Username: value of `POSTGRES_USER` from `.env`
- Password: value of `POSTGRES_PASSWORD` from `.env`

## Step 4: Connect Metabase

1. Open Metabase: `https://bi.yourdomain.com`
2. Complete initial setup (create admin account)
3. Add database:
   - Type: PostgreSQL
   - Name: KeyCRM Analytics
   - Host: `postgres`
   - Port: `5432`
   - Database: `analytics`
   - Username: from `.env`
   - Password: from `.env`
4. Save and sync

See [config/metabase/keycrm-setup.md](../config/metabase/keycrm-setup.md) for dashboard templates.

## Step 5: Enable AI Queries (MCP)

1. Start MCP servers:
   ```bash
   docker compose -f docker-compose.yml -f mcp/docker-compose.mcp.yml up -d mcp-postgres
   ```
2. In Open WebUI → Settings → Tools → Add MCP Server
3. See [mcp/README.md](../mcp/README.md) for details

## KeyCRM API Reference

Base URL: `https://openapi.keycrm.app/v1`

Key endpoints used by Activepieces:
- `GET /order` — list orders
- `GET /order/{id}` — order details with products
- `GET /buyer` — customers list
- `GET /payment` — payments

Auth: `Authorization: Bearer YOUR_API_KEY`

## Troubleshooting Sync

```bash
# Check Activepieces logs
docker compose logs activepieces --tail=50

# Check if data is arriving
docker compose exec postgres psql -U agenticadmin -d analytics -c "SELECT COUNT(*) FROM keycrm_orders;"

# Check sync log
docker compose exec postgres psql -U agenticadmin -d analytics -c "SELECT * FROM keycrm_sync_log ORDER BY started_at DESC LIMIT 5;"
```
