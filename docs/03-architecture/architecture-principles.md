# Nymbus — Architecture Principles

**Document type:** AFU — Technical architecture principles  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Purpose

These principles constrain future technical decisions. They are more important than choosing a particular framework or library.

## 2. Core principles

### ARCH-001 — Lightweight by default

Nymbus targets low-capacity NAS hardware. Every service, dependency and background process must have a measurable justification.

### ARCH-002 — Few services, strong boundaries

The system uses microservices where separation provides a meaningful benefit, but must not become a collection of tiny services with disproportionate operational overhead.

### ARCH-003 — Stateless application services where possible

Application services should avoid local mutable state that cannot be reconstructed from persistent storage or distributed state.

### ARCH-004 — E2E privacy is a boundary, not a feature flag

Private-note plaintext must remain outside the backend trust boundary during normal persistence, synchronization, search and administration.

### ARCH-005 — Server authorization is mandatory

Client-side visibility or encryption does not replace server-side authorization for server resources.

### ARCH-006 — Authentication and content unlock are separate

Account authentication establishes identity/session. Private-note unlocking establishes access to cryptographic material. These concerns must not be conflated.

### ARCH-007 — Browser-native security primitives first

Where standards provide suitable capabilities, Nymbus should prefer browser/platform primitives rather than custom cryptographic or authentication mechanisms.

### ARCH-008 — Offline is a first-class state

The client must be capable of retaining authorized local work and synchronizing it when connectivity returns.

### ARCH-009 — Synchronization must be deterministic

Concurrent updates require a formally defined merge model. Silent last-write-wins behavior is not acceptable for collaborative editing unless explicitly justified for a specific data type.

### ARCH-010 — Backend cannot search private plaintext

The backend must not receive or maintain a plaintext full-text index of private note bodies.

### ARCH-011 — Secrets never enter logs

Passwords, recovery keys, plaintext private content and cryptographic secret material must never be emitted to application logs, traces or telemetry.

### ARCH-012 — Persistence must be recoverable

All persistent state required to reconstruct the application must be explicitly identified and documented for NAS-level backup/restore.

### ARCH-013 — API contracts are explicit

Inter-service and client-server contracts must be versioned/documented and must not depend on undocumented implementation behavior.

### ARCH-014 — Background work is bounded

Scheduled/background operations must have defined frequency, concurrency and resource limits.

### ARCH-015 — Security failures fail closed

When authorization, key validation or cryptographic integrity cannot be established, protected operations must fail rather than silently degrade to plaintext or unrestricted access.

### ARCH-016 — Observability without content inspection

Operational health must be observable without requiring access to private-note plaintext.

### ARCH-017 — Replaceability

Infrastructure components should be replaceable where practical. Business rules must not become inseparable from a specific database, reverse proxy or NAS vendor.

### ARCH-018 — No unnecessary cloud dependency

Core Nymbus functionality must be designed to run on the user's NAS. Google is required as the account identity provider, but application content storage remains self-hosted.

## 3. Decision hierarchy

When architectural goals conflict, use this priority order:

1. Security and privacy.
2. Data integrity.
3. Correctness of authorization.
4. User data preservation.
5. Reliability/synchronization correctness.
6. Performance and resource efficiency.
7. UX convenience.
8. Implementation simplicity.
9. Non-essential optimization.

A convenience optimization must never weaken a higher-priority property.

## 4. Anti-patterns explicitly rejected

- Microservice-per-feature architecture.
- Backend decryption of private notes for convenience.
- Server-side plaintext search over private content.
- Secrets stored in browser local storage without an explicit threat-model justification.
- Custom authentication protocol where WebAuthn/passkeys are sufficient.
- Unbounded background workers.
- Heavy search/database infrastructure solely to provide private-content search.
- Application-level backups duplicating NAS backup responsibilities without a defined reason.
