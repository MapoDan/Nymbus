# Nymbus — Architecture Overview

**Document type:** AFU — System architecture overview  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Target architecture

Nymbus is a self-hosted PWA composed of a browser client and a small set of backend services running as containers on the user's NAS.

The architecture is intentionally modular but avoids excessive microservice fragmentation.

```text
                         Internet / LAN
                               │
                         Reverse Proxy
                               │
                 ┌─────────────┴─────────────┐
                 │                           │
             Web Client                 API Gateway/BFF
                 │                           │
                 │              ┌────────────┼─────────────┐
                 │              │            │             │
                 │          Identity     Core Data      Sync/Realtime
                 │              │            │             │
                 │              └────────────┼─────────────┘
                 │                           │
                 │                    Persistent Store
                 │                           │
                 └────────────── Local Client Storage
```

This diagram is conceptual. Exact service boundaries and technology choices are defined in subsequent architecture documents and ADRs.

## 2. Architectural layers

### Client layer

Responsible for:

- UI;
- editor;
- local persistence;
- offline operation;
- local private-note decryption;
- local private-content search;
- synchronization client;
- platform authenticator interactions.

### Edge layer

Responsible for:

- HTTPS termination;
- routing;
- security headers;
- request limits;
- optional websocket upgrade handling.

The reverse proxy is infrastructure and should not contain business logic.

### Identity layer

Responsible for:

- Google identity integration;
- application account identity;
- session lifecycle;
- WebAuthn/passkey registration and authentication;
- account security state.

### Core data layer

Responsible for:

- note metadata;
- folders;
- tags;
- sharing permissions;
- version metadata;
- notification state;
- attachment metadata;
- synchronization persistence.

It must not require private-note plaintext.

### Synchronization layer

Responsible for:

- synchronization protocol;
- change distribution;
- collaboration sessions;
- CRDT transport/persistence where required;
- reconnect handling.

### Persistence layer

Stores only data that the server is permitted to possess, plus encrypted blobs and cryptographic envelopes as defined by the security architecture.

## 3. Service-count philosophy

V1 should aim for a small number of deployable services rather than independently deploying every domain.

A service boundary is justified only when it provides one or more of:

- strong security isolation;
- independent lifecycle/scaling;
- materially different workload characteristics;
- clear ownership boundary;
- reduced failure coupling.

Otherwise, related backend domains should remain within the same lightweight service.

## 4. Client/server trust boundary

The browser client is trusted with the user's locally available plaintext because it is the execution environment where the user explicitly unlocks private notes.

The backend is not trusted with private-note plaintext.

This distinction is fundamental:

```text
User device
  ├── plaintext private content (when unlocked)
  ├── decrypted local search index
  └── cryptographic keys required by the authorized session

NAS backend
  ├── encrypted private content
  ├── permitted metadata
  ├── authorization state
  └── cryptographic envelopes required for authorized key exchange
```

## 5. Storage model at high level

The system should use one lightweight relational persistence technology for authoritative structured server data unless a documented requirement demonstrates that another datastore is necessary.

Binary assets should not be forced into relational rows merely for architectural purity.

Private encrypted assets must remain encrypted at rest and during transport.

## 6. Local client storage

The PWA requires persistent browser storage for:

- offline notes;
- pending changes;
- synchronization state;
- locally authorized encrypted/private content;
- local search indexes where applicable.

Sensitive browser data must follow the cryptographic threat model rather than being treated as ordinary cache data.

## 7. Realtime behavior

Realtime channels are activated for active collaborative editing. The architecture must avoid maintaining a high-frequency connection for every idle user solely to provide notifications or ordinary synchronization.

## 8. Failure isolation

Failure of a non-critical feature such as notifications must not prevent reading/editing local notes.

Failure of the realtime collaboration path must degrade to ordinary synchronization where safe.

Failure of authentication prevents new sessions but must not destroy already persisted local offline work.

## 9. Resource efficiency

The architecture should minimize:

- always-on processes;
- open idle connections;
- repeated polling;
- duplicated caches;
- heavyweight search engines;
- unnecessary message brokers.

An event/message broker is not assumed for V1. It requires an ADR if later introduced.

## 10. Deployment model

Nymbus is intended to run as a small containerized stack on the user's NAS behind an existing reverse proxy or an equivalent edge service.

The architecture documentation must define:

- persistent volumes;
- service dependencies;
- startup/shutdown order;
- health checks;
- required environment variables/secrets;
- backup scope;
- upgrade strategy;
- recovery strategy.

## 11. Architecture decisions still requiring dedicated ADRs

Before implementation, dedicated decisions are required for:

- backend service boundaries;
- database selection;
- client storage strategy;
- cryptographic primitives and key hierarchy;
- password-derived keys;
- WebAuthn/passkey integration;
- recovery protocol;
- private-note sharing/re-keying;
- CRDT choice;
- realtime transport;
- attachment storage;
- search indexing;
- session/token strategy;
- backup and restore;
- observability.
