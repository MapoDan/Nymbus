# Nymbus — API Overview

**Document type:** AFU — API contract overview  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Purpose

The API is the controlled boundary between the PWA and Nymbus backend services. It exposes application operations without exposing internal service implementation details.

## 2. API principles

- authenticated requests only where required;
- object-level authorization on every protected resource;
- idempotency for retryable mutations;
- explicit API versioning;
- structured errors;
- no secrets in logs or responses;
- private note plaintext never sent to the backend;
- stable resource identifiers;
- predictable pagination;
- bounded request/response sizes.

## 3. API versioning

V1 endpoints must have an explicit version boundary. Breaking changes require a new API version or a formally managed migration strategy.

## 4. Resource domains

The V1 API is logically divided into:

- authentication/session;
- users/security;
- notes;
- folders;
- tags;
- sharing/permissions;
- search;
- synchronization;
- attachments;
- versions;
- notifications;
- recovery;
- export.

## 5. Trust boundary

The API may receive encrypted note payloads and metadata, but it must not require plaintext private note bodies or plaintext private images.

## 6. API does not define crypto

The API transports encrypted material and key envelopes according to the cryptographic architecture. It must not invent or implement ad-hoc cryptographic protocols at HTTP-controller level.

## 7. Resource identifiers

Identifiers must be opaque and stable. Clients must not infer ownership or authorization from identifier structure.

## 8. Pagination

List endpoints must use deterministic pagination suitable for large collections. Cursor-based pagination is preferred for mutable collections where offset pagination could produce duplicates or skips during concurrent changes.

## 9. Filtering

Filtering parameters must be explicitly allow-listed. Arbitrary database expressions must never be exposed through API parameters.

## 10. Mutation semantics

Create/update/delete operations must define:

- authorization;
- validation;
- idempotency behavior;
- synchronization effect;
- audit effect where applicable;
- error behavior.

## 11. HTTP semantics

The implementation should use standard HTTP semantics consistently for success, validation failure, authentication failure, authorization failure, conflict and server failure.

Exact status-code mapping belongs in `api-conventions.md`.

## 12. Response minimization

Responses must contain only information required by the client. Sensitive internal fields, database identifiers and operational secrets must not leak.

## 13. Rate limiting

Authentication, recovery, search and expensive synchronization operations must have appropriate rate limits.

## 14. API documentation

The implementation should publish a machine-readable API contract generated from the approved specification. The contract itself must remain synchronized with these functional documents.

## 15. Internal services

External clients should not directly address internal microservices. The backend API boundary remains the controlled entry point.
