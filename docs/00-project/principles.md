# Nymbus — Product and Engineering Principles

**Document status:** Baseline  
**Last updated:** 2026-08-09

These principles guide requirements, architecture, implementation, UX, and future changes. When a detailed specification does not prescribe an implementation detail, these principles should be used to choose between viable alternatives.

## P-001 — Simplicity before feature count

Nymbus should do a small set of note-taking tasks exceptionally well. A feature that adds complexity without materially improving the core note-taking experience should not be included merely because competing products offer it.

## P-002 — Privacy is an architectural property

Private-note security must not depend on a server-side promise that data will not be inspected. The architecture must make unauthorized plaintext access technically unavailable to the backend wherever the product's E2E model applies.

## P-003 — Never weaken cryptography for convenience

User experience must be built around the cryptographic model, not the other way around. If a desired interaction conflicts with secure key handling, the interaction must be redesigned or explicitly approved as a security decision.

## P-004 — Metadata and content have different confidentiality classes

Nymbus intentionally permits selected metadata of private notes to remain unencrypted so that users can find and organize locked notes. The system must maintain an explicit classification of data that may be visible to the server versus data that must remain client-side encrypted.

## P-005 — Client-first private content

Private-note plaintext should exist only on an authorized client while actively needed. Local caches, IndexedDB records, memory, clipboard behavior, previews, search indexes, and exports must be considered part of the private-data lifecycle.

## P-006 — Offline is a first-class state

Offline is not an exceptional error condition. The application must clearly communicate synchronization state and preserve user work when the network is unavailable.

## P-007 — Deterministic synchronization

Concurrent changes must converge to a deterministic result. Conflict resolution must be defined by the synchronization/CRDT specification rather than left to ad-hoc UI behavior.

## P-008 — Real-time only when useful

Persistent real-time connections should be established when collaborative activity justifies them and released when they are no longer useful. This is a core resource-saving strategy for NAS deployment.

## P-009 — Small number of meaningful services

Nymbus may use microservices, but service boundaries must be justified. The goal is independent responsibility and maintainability, not maximum container count.

## P-010 — Minimize idle resource consumption

The system should consume as little CPU, RAM, disk I/O, and network traffic as reasonably possible while remaining responsive. Background jobs should be bounded and event-driven where possible.

## P-011 — Local responsiveness

User interactions that do not require authoritative server state should feel immediate. Optimistic/local updates are preferred where they are compatible with data integrity and security.

## P-012 — Server authority for authorization

The client may optimize the UI, but it is never the authority for access control. Every protected API and synchronization operation must enforce server-side authorization.

## P-013 — Explicit security boundaries

Authentication, authorization, encryption, device trust, note protection, recovery, and sharing are separate concepts and must not be conflated in the implementation or documentation.

## P-014 — Google identity, Nymbus security

Google authenticates the Nymbus account. Nymbus private-note protection is a separate security domain. A successful Google login must not by itself disclose private-note plaintext.

## P-015 — Passkeys improve authentication, not encryption semantics

Passkeys are primarily an account-authentication mechanism. They may participate in secure local unlock flows where the platform allows it, but they must not be treated as a generic substitute for encryption keys without an explicit cryptographic design.

## P-016 — Revocation must be honest

Nymbus can control future synchronization and access through its authorization layer. It cannot guarantee destruction of information a user has deliberately copied outside the application. Documentation must distinguish enforceable revocation from impossible retrospective data destruction.

## P-017 — Data integrity over convenience

A user-visible success state must not be shown before the application has sufficient evidence that the corresponding operation has been persisted locally or accepted by the synchronization layer, according to the operation type.

## P-018 — Errors are product states

Loading, offline, synchronization pending, synchronization failed, authorization denied, locked content, revoked access, missing attachments, and recovery states must all have explicit UI behavior.

## P-019 — Accessible by default

Keyboard navigation, focus management, contrast, reduced-motion preferences, semantic markup, touch targets, screen-reader labels, and accessible editor behavior are part of the product rather than optional enhancements.

## P-020 — Responsive without separate products

The same application model should work across phones, tablets, laptops, and desktop browsers. Responsive behavior should change navigation and layout, not create incompatible feature sets.

## P-021 — Visual restraint

The UI should be modern and creative without decorative excess. Color, animation, shadows, and gradients must reinforce hierarchy rather than compete with the note content.

## P-022 — Content is the visual protagonist

The editor and note content should dominate the interface. Toolbars, navigation, metadata, and controls should remain visually subordinate.

## P-023 — Progressive disclosure

Advanced security and collaboration actions should be available when needed without cluttering the primary note-writing experience.

## P-024 — Stable contracts

APIs, event schemas, database contracts, and cryptographic formats should be versioned or evolved deliberately. Breaking changes require documentation and migration planning.

## P-025 — Test the dangerous paths

The highest test priority belongs to encryption, authorization, recovery, synchronization, offline persistence, revocation, data deletion, and concurrent editing. A visually correct application with insecure synchronization is not acceptable.

## P-026 — Documentation is part of implementation

When behavior changes, the relevant requirement, API, data model, acceptance criteria, or ADR must change with it. Code and specification must remain synchronized.

## P-027 — No silent assumptions

When a specification is incomplete, the implementation must not silently invent a security-sensitive or externally visible behavior. The ambiguity must be resolved or explicitly recorded.

## P-028 — V1 scope discipline

The absence of a feature in V1 is intentional when documented as out of scope. In particular, content import and application-level backup must not appear accidentally through generic tooling.
