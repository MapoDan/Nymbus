# Nymbus — Architectural Principles

## Principles

1. **Security by boundary** — plaintext private content must never cross into backend services.
2. **Client-side cryptography** — encryption and decryption of private notes are client responsibilities.
3. **Offline first** — core note work must remain available without a network connection.
4. **Server as synchronization authority** — durable shared state is coordinated by the backend.
5. **Least infrastructure** — prefer simple components with low operational and resource overhead.
6. **Modular monolith before distributed complexity** — logical service boundaries are required, but physical separation is justified only where it provides a concrete benefit.
7. **Explicit consistency** — every mutable resource must have a documented conflict/ordering model.
8. **Metadata/content separation** — searchable metadata may remain server-readable while protected content remains ciphertext.
9. **Fail closed for security** — uncertainty in authentication, authorization or cryptographic state must deny sensitive operations.
10. **Observable without secrets** — logs and metrics must describe system behavior without exposing plaintext, passwords or keys.
11. **Portable deployment** — the application must be deployable as a small Docker-based stack on a NAS.
12. **Evolution through versioning** — APIs, encrypted envelopes, synchronization operations and data formats require explicit version identifiers.

## Decision rule

When two technically valid designs exist, prefer the design with fewer components, lower idle resource consumption and fewer operational failure modes, provided security and correctness are not reduced.
