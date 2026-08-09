# Nymbus — Service Boundaries

**Document type:** AFU — Backend service boundary analysis  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Objective

Nymbus requires a microservice-oriented backend, but the NAS resource constraint makes excessive fragmentation undesirable.

The V1 target is therefore a **small set of coarse-grained services** with explicit responsibilities.

## 2. Proposed logical services

### 2.1 Identity service

Responsibilities:

- Google authentication integration;
- application identity mapping;
- session management;
- passkey/WebAuthn registration and authentication;
- account security metadata.

Must not:

- store note plaintext;
- decrypt private notes;
- perform note business logic.

### 2.2 Core service

Responsibilities:

- notes and note metadata;
- folders;
- tags;
- favorites;
- sharing/permissions;
- version metadata;
- notifications;
- server-visible search;
- export orchestration where appropriate.

The core service handles authorization for its resources.

### 2.3 Sync service

Responsibilities:

- synchronization protocol;
- change streams;
- collaborative editing sessions;
- realtime transport;
- CRDT operations where the chosen model requires server coordination.

The sync service must not require private plaintext.

### 2.4 Optional media/asset boundary

V1 should avoid a separate media microservice unless storage/security requirements demonstrate a meaningful benefit. Asset operations can initially be part of the core service while maintaining a clear internal boundary.

## 3. Why not more services?

The following are intentionally not independent V1 services:

- tags;
- folders;
- search;
- notifications;
- version history;
- export;
- attachments.

Splitting these into separate containers would increase memory consumption, deployment complexity, network hops and failure modes without a proportional product benefit.

## 4. Inter-service communication

Service communication must use explicit contracts. Direct database access between services is prohibited as a default architectural pattern.

A service may own specific tables/data structures. Other services access that ownership through defined APIs or events.

## 5. Authentication between services

Internal service calls must be authenticated/authorized. The system must not assume that any process on the Docker network is inherently trusted.

## 6. Data ownership

| Data domain | Owner |
|---|---|
| Identity/account identity | Identity |
| Passkeys | Identity |
| Sessions | Identity |
| Note metadata | Core |
| Folders/tags/favorites | Core |
| Permissions | Core |
| Version metadata | Core |
| Sync state | Sync, with authoritative references owned by Core where needed |
| Realtime sessions | Sync |
| Encrypted note blobs | Core/storage boundary |
| Notification state | Core |

## 7. Failure behavior

### Identity unavailable

Existing sessions should continue only according to their defined session lifetime and security policy. New authentication cannot proceed.

### Core unavailable

Server-side note operations fail, but local offline work must remain available.

### Sync unavailable

Editing can continue locally. Changes remain pending and are reconciled when synchronization returns.

### Realtime unavailable

Active collaboration may degrade to reconnecting/pending synchronization. Local accepted edits must not disappear.

## 8. Resource constraints

Each service must have documented expected idle and normal operating resource behavior before deployment.

No service should run an independent polling loop unless required.

## 9. Service boundary evolution

A new service may be introduced only when at least one of these conditions is met:

1. security isolation requires it;
2. resource profile differs materially;
3. independent scaling is necessary;
4. failure isolation is materially improved;
5. the service has a stable bounded responsibility.

## 10. V1 target

The architecture should initially be capable of running with approximately:

- one reverse proxy/edge component;
- one identity service;
- one core service;
- one synchronization service;
- one relational database;
- one persistent object/blob storage mechanism if required.

The exact number may be reduced if an ADR demonstrates that two logical boundaries can safely coexist in one deployable service without compromising security or maintainability.
