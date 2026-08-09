# Nymbus — Architecture Overview

**Document type:** AFU — System architecture  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Objective

Nymbus is a lightweight, self-hosted note application designed to run on a low-capacity NAS while providing offline-first operation, synchronization, collaboration and E2E-protected private notes.

## 2. Architectural shape

V1 uses a modular service-oriented backend rather than a large distributed platform. Services must be independently bounded logically, but deployment should remain lightweight and may consolidate low-load services into a small number of containers where this reduces resource consumption without weakening isolation.

The main domains are:

- web/PWA client;
- API/application service;
- authentication/session domain;
- notes/content domain;
- sharing/authorization domain;
- synchronization domain;
- search/metadata domain;
- attachment/storage domain;
- notification domain;
- administration/observability domain;
- relational database;
- persistent object/file storage.

## 3. Client-first cryptography

Private note encryption/decryption happens on the client. The backend persists and synchronizes ciphertext and permitted metadata but does not provide a private-note plaintext service.

## 4. Offline-first

The PWA must remain useful without network connectivity. Local state is authoritative for the user's current device until synchronization converges it with the server.

## 5. Server role

The server is responsible for identity/session validation, authorization, persistence, synchronization coordination, metadata search and distribution of encrypted data. It is not responsible for interpreting private plaintext.

## 6. NAS constraint

The architecture must minimize idle CPU, RAM, disk I/O and container count. Heavy infrastructure such as Kubernetes, Kafka, Redis clusters or dedicated search engines is out of scope for V1 unless a measured requirement proves otherwise.

## 7. Consistency

Ordinary collaborative note state uses the approved CRDT/synchronization model. Security-sensitive authorization changes remain server-enforced and are not made trustworthy merely because a client has converged to a CRDT state.

## 8. Source of truth

The server is the durable source of synchronized shared application state. The client is the working source of truth while offline. Convergence is achieved through versioned operations and synchronization rules.

## 9. Architectural boundaries

The architecture must preserve these boundaries:

- authentication ≠ authorization;
- authorization ≠ decryption;
- metadata search ≠ private plaintext search;
- application session ≠ private-note unlock;
- local cache ≠ secure key vault;
- attachment storage ≠ attachment interpretation;
- synchronization ≠ permission granting.

## 10. Explicit non-goals

V1 does not target multi-region deployment, horizontal service fleets, enterprise-scale tenancy, server-side private-content indexing or heavyweight distributed infrastructure.
