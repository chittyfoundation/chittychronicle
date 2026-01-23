# ChittyChronicle Charter

## Classification
- **Tier**: 1 (Foundation)
- **Organization**: CHITTYFOUNDATION
- **Domain**: chronicle.chitty.cc

## Mission

ChittyChronicle is the **authoritative event logging and audit trail system** for the ChittyOS ecosystem. It captures all significant events across services for compliance, debugging, and analytics purposes.

## Scope

### IS Responsible For
- Event logging from all ChittyOS services
- Full-text search across events
- Timeline aggregation and visualization
- Audit trail retrieval by entity
- Service health monitoring via event analysis
- Statistics and analytics
- Event correlation and relationship tracking

### IS NOT Responsible For
- Immutable ledger entries (ChittyLedger)
- Identity management (ChittyID)
- Authentication (ChittyAuth)
- Case timeline management (ChittyCases)

## Dependencies

| Type | Service | Purpose |
|------|---------|---------|
| Peer | ChittyAuth | Token validation for API access |
| Peer | ChittyID | Entity identification |
| Downstream | ChittyLedger | Receives transaction logs |
| Upstream | All Services | Receives events from ecosystem |

## API Contract

**Base URL**: https://chronicle.chitty.cc

### Core Endpoints
| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/events` | POST | Log new event |
| `/events` | GET | Search events |
| `/api/entries` | POST/GET | Backward compatibility alias |
| `/timeline` | GET | Get timeline aggregation |
| `/audit/:entityId` | GET | Get audit trail for entity |
| `/statistics` | GET | Get event statistics |
| `/health` | GET | Service health with event analysis |

### Aggregated Routes
| Route | Gateway |
|-------|---------|
| `api.chitty.cc/v1/chronicle/*` | ChittyAPI aggregator |
| `api.chitty.cc/chittychronicle/*` | ChittyAPI aggregator |

## Data Model

### Chronicle Event
```typescript
interface ChronicleEvent {
  id: UUID;
  timestamp: DateTime;
  service: string;
  action: string;
  user_id?: string;
  user_email?: string;
  metadata?: Record<string, any>;
  related_events?: UUID[];
  triggered_by?: string;
  integrations?: string[];
  status: 'success' | 'failed' | 'pending';
  error_message?: string;
  search_vector: tsvector;  // Full-text search
}
```

## Event Schema by Service

| Service | Event Types |
|---------|-------------|
| ChittyID | `identity.minted`, `identity.validated`, `identity.revoked` |
| ChittyAuth | `auth.login`, `auth.logout`, `auth.token_issued` |
| ChittyConnect | `credential.provisioned`, `context.updated` |
| ChittyLedger | `ledger.entry_added`, `ledger.chain_verified` |
| ChittyCases | `case.created`, `case.updated`, `evidence.added` |

## Database

- **Provider**: Neon PostgreSQL (shared with ChittyLedger)
- **Project**: shy-sound-75632194 (ChittyFoundation org)
- **Table**: chronicle_events
