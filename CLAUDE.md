# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

ChittyChronicle is the **event logging and audit trail system** for the ChittyOS ecosystem. It captures events from all services for compliance, debugging, analytics, and audit purposes.

### Key Architecture Components

1. **API Module**: `api/` - Cloudflare Worker REST API (Hono framework)
2. **MCP Module**: `mcp/` - Model Context Protocol server for AI integrations
3. **Service Logic**: `svc/` - Core business logic and database operations
4. **Agent Definitions**: `agent/` - AI agent configurations
5. **Frontend**: `app/` - Web application (if applicable)
6. **Library**: `lib/` - Shared utilities and types

### Service Structure

```
chittychronicle/
├── api/           # REST API (Cloudflare Worker)
│   ├── src/
│   │   └── index.ts
│   ├── wrangler.toml
│   └── package.json
├── mcp/           # MCP server
├── svc/           # Service logic
├── agent/         # Agent definitions
├── app/           # Frontend
├── lib/           # Shared library
├── CHARTER.md     # Service charter
└── CLAUDE.md      # This file
```

## Development Commands

### API Development
```bash
cd api/

# Local development
npm run dev

# Deploy to Cloudflare
npm run deploy

# Set secrets
wrangler secret put NEON_DATABASE_URL
```

### Testing
```bash
# Health check
curl https://chronicle.chitty.cc/

# Log event
curl -X POST https://chronicle.chitty.cc/events \
  -H "Content-Type: application/json" \
  -d '{"service":"test","action":"example","status":"success"}'

# Search events
curl "https://chronicle.chitty.cc/events?service=test"

# Get statistics
curl https://chronicle.chitty.cc/statistics
```

## Routing

| Route | Purpose |
|-------|---------|
| `chronicle.chitty.cc/*` | Direct service API |
| `api.chitty.cc/v1/chronicle/*` | Aggregated canonical route |
| `api.chitty.cc/chittychronicle/*` | Aggregated service route |

## Backward Compatibility

ChittyConnect calls `/api/entries` - this is aliased to `/events`:
- `POST /api/entries` → `POST /events`
- `GET /api/entries` → `GET /events`

## Database Schema

The service uses Neon PostgreSQL with the `chronicle_events` table:

```sql
CREATE TABLE chronicle_events (
  id UUID PRIMARY KEY,
  timestamp TIMESTAMPTZ DEFAULT NOW(),
  service VARCHAR(50) NOT NULL,
  action VARCHAR(100) NOT NULL,
  user_id VARCHAR(255),
  user_email VARCHAR(255),
  metadata JSONB,
  related_events UUID[],
  triggered_by VARCHAR(50),
  integrations TEXT[],
  status VARCHAR(20) DEFAULT 'success',
  error_message TEXT,
  search_vector tsvector GENERATED ALWAYS AS (
    to_tsvector('english', service || ' ' || action || ' ' || COALESCE(metadata::text, ''))
  ) STORED
);

CREATE INDEX idx_chronicle_service ON chronicle_events(service, timestamp DESC);
CREATE INDEX idx_chronicle_search ON chronicle_events USING GIN(search_vector);
```

## Key Principles

1. **All Events Welcome**: Accept events from any ChittyOS service
2. **Searchability**: Full-text search enabled by default
3. **Correlation**: Track related events and triggers
4. **Analytics**: Provide timeline and statistics endpoints
