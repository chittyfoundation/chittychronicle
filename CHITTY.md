---
uri: chittycanon://docs/ops/architecture/chitty-chronicle
namespace: chittycanon://docs/ops
type: summary
version: 1.0.0
status: DRAFT
registered_with: chittycanon://core/services/canon
title: "ChittyChronicle"
certifier: chittycanon://core/services/chittycertify
visibility: PUBLIC
---

# ChittyChronicle

> `chittycanon://core/services/chitty-chronicle` | Tier 3 (Service Layer) | chronicle.chitty.cc

## What It Does

Authoritative event logging and audit trail system for the ChittyOS ecosystem. Captures events from all services for compliance, debugging, and analytics. Provides full-text search, timeline aggregation, and event correlation.

## Architecture

Cloudflare Worker (Hono) deployed at chronicle.chitty.cc. Modular structure with separate API, MCP, service logic, and agent modules.

### Stack
- **Runtime**: Cloudflare Workers
- **Framework**: Hono
- **Database**: Neon PostgreSQL (shared with ChittyLedger)
- **Search**: PostgreSQL tsvector full-text search

### Key Components
- `api/src/index.ts` — REST API entry point
- `mcp/` — Model Context Protocol server
- `svc/` — Core business logic
- `agent/` — AI agent configurations

## ChittyOS Ecosystem

### Certification
- **Badge**: ChittyOS Compatible
- **Certifier**: ChittyCertify (`chittycanon://core/services/chittycertify`)
- **Last Certified**: --

### ChittyDNA
- **ChittyID**: --
- **DNA Hash**: --
- **Lineage**: root (event logging foundation)

### Dependencies
| Service | Purpose |
|---------|---------|
| ChittyAuth | Token validation for API access |
| ChittyID | Entity identification |
| All Services | Receives events from ecosystem |

### Endpoints
| Path | Method | Auth | Purpose |
|------|--------|------|---------|
| `/health` | GET | No | Health check |
| `/events` | POST | Yes | Log new event |
| `/events` | GET | Yes | Search events |
| `/timeline` | GET | Yes | Timeline aggregation |
| `/audit/:entityId` | GET | Yes | Audit trail for entity |
| `/statistics` | GET | No | Event statistics |

## Document Triad

This badge is part of a synchronized documentation triad. Changes to shared fields must propagate.

| Field | Canonical Source | Also In |
|-------|-----------------|---------|
| Canonical URI | CHARTER.md (Classification) | CHITTY.md (blockquote) |
| Tier | CHARTER.md (Classification) | CHITTY.md (blockquote) |
| Domain | CHARTER.md (Classification) | CHITTY.md (blockquote), CLAUDE.md (header) |
| Endpoints | CHARTER.md (API Contract) | CHITTY.md (Endpoints table), CLAUDE.md (API section) |
| Dependencies | CHARTER.md (Dependencies) | CHITTY.md (Dependencies table), CLAUDE.md (Architecture) |
| Certification badge | CHITTY.md (Certification) | CHARTER.md frontmatter `status` |

**Related docs**: [CHARTER.md](CHARTER.md) (charter/policy) | [CLAUDE.md](CLAUDE.md) (developer guide)
