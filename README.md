![Foundation](https://img.shields.io/badge/Foundation-service-8B5CF6?style=flat-square)
![Tier](https://img.shields.io/badge/tier-3%20Operational-0EA5E9?style=flat-square)

# ChittyChronicle

> Authoritative event logging and audit trail system for the ChittyOS ecosystem.

ChittyChronicle captures all significant events across every ChittyOS service for compliance, debugging, and analytics. It provides full-text search across events, timeline aggregation, entity-scoped audit trail retrieval, and event correlation — backed by Neon PostgreSQL with a tsvector search index. Deployed as a Cloudflare Worker (Hono) at `chronicle.chitty.cc` and also accessible via the ChittyAPI aggregator.

**Domain**: `chronicle.chitty.cc`
