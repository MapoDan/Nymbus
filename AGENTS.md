# Nymbus — AI Development Instructions

## Purpose

This repository contains the product, functional, UX/UI, security, architecture, data, API, infrastructure, testing, and development specifications for **Nymbus**, a lightweight multi-user Markdown note-taking PWA designed to run on low-resource NAS hardware.

The documentation is intended to be consumed both by human developers and by coding agents such as Codex. The documentation is the source of truth for implementation decisions.

## Product definition

Nymbus is a simple, modern and lightweight note-taking application inspired by the clarity of Notion, the refinement of Apple software, and the productivity orientation of Obsidian. It is not intended to reproduce the complete feature set of any of those products.

Core V1 capabilities include:

- Markdown-based notes with a visual writing experience and formatting toolbar.
- Folders and nested folders.
- Free-form user tags.
- Favorites for notes and folders.
- Global metadata search and full-text search where technically permitted.
- Inline images and generic file attachments.
- Multi-user operation.
- Google-only initial account authentication.
- WebAuthn/passkey login after the first Google login.
- Device and passkey management.
- Private notes protected by end-to-end encryption.
- Shared private notes that remain end-to-end encrypted.
- Per-user protection choices for shared private notes.
- Offline-first operation.
- CRDT-based synchronization and hybrid real-time collaboration.
- 15-version note history.
- Notifications and Web Push.
- Markdown or PDF export.
- Light/dark/system themes.
- Docker-based deployment suitable for a low-resource NAS.

V1 explicitly does **not** include content import. Import is a possible V2 feature.

V1 does **not** implement an application-level backup system. Backup and recovery of the Nymbus data volumes are responsibilities of the NAS/infrastructure layer.

## Documentation authority

Before implementing a feature, read the applicable documentation under `/docs` and the relevant Architecture Decision Records under `/decisions`.

When documents conflict, use this priority order:

1. Security requirements and cryptographic specification.
2. Explicit product constraints and scope.
3. Functional requirements.
4. Non-functional requirements.
5. Architecture and data specifications.
6. API specifications.
7. UX/UI specifications.
8. Implementation guidance.

If an implementation detail conflicts with a higher-priority requirement, do not silently choose a workaround. Document the conflict and propose an ADR or clarification.

## Mandatory engineering principles

### Security first

- Private-note plaintext must never be sent to or stored by the backend unless a future specification explicitly changes the security model.
- Master passwords, note passwords, recovery secrets, and encryption keys must never be stored in plaintext on the server.
- Do not invent cryptographic primitives or protocols. Use established, reviewed primitives and browser/platform APIs.
- Do not weaken encryption or authentication to simplify implementation.
- Authorization must be enforced server-side even when the client hides inaccessible resources.
- A revoked user's access must be rejected by the synchronization/API layer.
- Security-sensitive changes require tests and, where applicable, an ADR.

### Privacy by design

- Metadata that is intentionally defined as non-secret may remain searchable server-side.
- Private-note content and private attachments are encrypted client-side.
- Do not expand the amount of plaintext metadata stored by the backend without an explicit product decision.

### Resource efficiency

Nymbus is intended to run on a NAS with limited CPU and RAM. Prefer:

- small services with clear responsibilities;
- few long-running processes;
- event-driven or on-demand work;
- minimal persistent connections;
- efficient database access;
- bounded memory usage;
- streaming for large attachments where practical;
- client-side work where it reduces server load without compromising security;
- simple operational dependencies.

Do not interpret “microservices” as permission to create an excessive number of containers. A service must have a meaningful independent responsibility and the benefit of separation must justify its CPU, RAM, networking, deployment, and operational cost.

### Correctness and data integrity

- Offline changes must not be silently lost.
- Synchronization must converge deterministically.
- Concurrent edits must be handled according to the documented CRDT model.
- Version history must respect the V1 retention requirement of 15 versions.
- Deletion and revocation operations must be explicit and testable.

### Scope discipline

Do not implement V2 functionality merely because a library makes it easy. In particular, V1 has no content import subsystem.

Do not add unrelated productivity features, calendars, task management, databases, kanban boards, or other Notion-like functionality unless a future specification explicitly introduces them.

## Working method for coding agents

Before changing code:

1. Identify the relevant requirement IDs.
2. Read the relevant feature specification.
3. Read related security and architecture specifications.
4. Read applicable ADRs.
5. Identify affected APIs, data models, UI flows, and tests.
6. Implement the smallest coherent change.
7. Add or update tests.
8. Update documentation when behavior or contracts change.
9. Check resource impact, especially RAM, CPU, storage, and persistent connections.

When a requirement is ambiguous, do not guess silently. Record the ambiguity and request clarification unless an existing ADR or specification resolves it.

## Definition of done

A feature is not complete merely because its UI works. It is complete only when:

- functional requirements are implemented;
- authorization and security rules are enforced;
- offline/synchronization behavior is implemented where applicable;
- API contracts are documented;
- data model changes are documented;
- automated tests cover the important behavior;
- error and empty states are handled;
- responsive behavior is implemented;
- accessibility requirements are respected;
- resource consumption is reasonable for the target NAS;
- relevant documentation and acceptance criteria are updated.

## No silent architectural decisions

If implementation requires choosing between materially different approaches for encryption, authentication, synchronization, persistence, service boundaries, or data ownership, create or update an ADR before proceeding.

## Documentation style

Requirements should be:

- explicit;
- testable;
- unambiguous;
- independently identifiable;
- free from implementation assumptions unless the implementation itself is a requirement.

Use stable IDs such as `FR-NOTE-001`, `SEC-CRYPTO-001`, `NFR-PERF-001`, `UX-EDITOR-001`, and `TEST-SYNC-001`.

When requirements are superseded, preserve their history instead of silently deleting the previous decision.
