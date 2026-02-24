---
uri: chittycanon://docs/ops/policy/chitty-chronicle-charter
namespace: chittycanon://docs/ops
type: policy
version: 1.0.0
status: DRAFT
registered_with: chittycanon://core/services/canon
title: "ChittyChronicle Charter"
certifier: chittycanon://core/services/chittycertify
visibility: PUBLIC
---

# ChittyChronicle Charter

## Classification
- **Canonical URI**: `chittycanon://core/services/chitty-chronicle`
- **Tier**: 3 (Service Layer)
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

## Document Triad

This charter is part of a synchronized documentation triad. Changes to shared fields must propagate.

| Field | Canonical Source | Also In |
|-------|-----------------|---------|
| Canonical URI | CHARTER.md (Classification) | CHITTY.md (blockquote) |
| Tier | CHARTER.md (Classification) | CHITTY.md (blockquote) |
| Domain | CHARTER.md (Classification) | CHITTY.md (blockquote), CLAUDE.md (header) |
| Endpoints | CHARTER.md (API Contract) | CHITTY.md (Endpoints table), CLAUDE.md (API section) |
| Dependencies | CHARTER.md (Dependencies) | CHITTY.md (Dependencies table), CLAUDE.md (Architecture) |
| Certification badge | CHITTY.md (Certification) | CHARTER.md frontmatter `status` |

**Related docs**: [CHITTY.md](CHITTY.md) (badge/one-pager) | [CLAUDE.md](CLAUDE.md) (developer guide)

---
*Charter Version: 1.0.0 | Last Updated: 2026-02-23*
