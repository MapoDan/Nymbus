# Nymbus — Non-Functional Requirements

**Document type:** AFU — Non-functional analysis  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Purpose

These requirements define the quality attributes and operational constraints that Nymbus must satisfy independently of individual features.

## 2. Performance

### NFR-PERF-001 — Perceived responsiveness

The UI should respond immediately to local interactions whenever the operation does not require authoritative server confirmation.

### NFR-PERF-002 — Local-first interaction

Typing, cursor movement, formatting, navigation between locally available notes, and local metadata filtering must not depend unnecessarily on network round trips.

### NFR-PERF-003 — Search responsiveness

Search over local/private decrypted content must execute locally and must not depend on a backend request for every keystroke.

### NFR-PERF-004 — Synchronization efficiency

Synchronization must transfer only the data necessary to converge replicas rather than repeatedly transferring entire notes when the synchronization model can avoid it.

### NFR-PERF-005 — Large attachments

Attachment processing must use bounded memory and streaming/chunking where appropriate so a large file does not require loading the complete object into memory unnecessarily.

## 3. NAS resource consumption

### NFR-RES-001 — Low idle consumption

The backend must minimize CPU and RAM consumption while idle.

### NFR-RES-002 — Limited service count

Microservice boundaries must be justified. A new service must have a meaningful independent responsibility and measurable operational benefit.

### NFR-RES-003 — Bounded background jobs

Background tasks such as notifications, cleanup, indexing, retention, and synchronization maintenance must be bounded and scheduled/event-driven where possible.

### NFR-RES-004 — Efficient persistence

Database access must avoid unnecessary polling, repeated full scans, and unbounded result sets.

### NFR-RES-005 — Connection discipline

Persistent network connections must be used only where they provide meaningful functionality. Real-time collaboration connections should be activated only for actively collaborative resources.

### NFR-RES-006 — Storage efficiency

The system should avoid unnecessary duplication of note content, attachment data, indexes, and historical versions while still satisfying functional and security requirements.

## 4. Security

### NFR-SEC-001 — Confidentiality

Private-note plaintext must remain outside the backend's required trust boundary.

### NFR-SEC-002 — Cryptographic correctness

Only established cryptographic primitives and browser/platform APIs with appropriate security properties may be used. Custom cryptography is prohibited.

### NFR-SEC-003 — Key separation

Authentication credentials, account/session secrets, private-note encryption keys, master-password-derived keys, recovery material, and note-specific protection secrets must be modeled as distinct security domains.

### NFR-SEC-004 — Secret handling

Plaintext passwords, recovery keys, private keys, and encryption keys must not be logged, exposed in telemetry, persisted in server-readable application tables, or included in ordinary API responses.

### NFR-SEC-005 — Authorization

All protected backend and synchronization operations must enforce authorization server-side.

### NFR-SEC-006 — Revocation

Access revocation must affect future authorization and synchronization. Private-note sharing must use cryptographic re-keying where required to prevent future access by revoked identities.

### NFR-SEC-007 — Recovery

Recovery must not create a server-side decryption capability that did not exist before recovery.

### NFR-SEC-008 — Browser leakage prevention

Private plaintext must be considered sensitive in relation to browser caches, local storage, IndexedDB, logs, analytics, notifications, page titles, previews, clipboard operations, and crash reporting.

### NFR-SEC-009 — Secure defaults

Security-sensitive settings must default to the safest practical behavior unless the user explicitly chooses otherwise.

## 5. Privacy

### NFR-PRIV-001 — Metadata classification

The project must maintain an explicit data classification distinguishing server-visible metadata from client-encrypted content.

### NFR-PRIV-002 — Data minimization

Only data required for functionality must be collected and retained.

### NFR-PRIV-003 — No plaintext indexing on server

The server must not maintain a plaintext full-text index of private-note content.

## 6. Reliability and data integrity

### NFR-REL-001 — No silent loss

Accepted local changes must survive temporary network loss, browser refresh, and synchronization retries within the defined local-storage guarantees.

### NFR-REL-002 — Deterministic convergence

Concurrent replicas must converge according to the selected synchronization model.

### NFR-REL-003 — Idempotent retry behavior

Network retries must not create duplicate logical operations when the operation can be safely made idempotent.

### NFR-REL-004 — Graceful degradation

If a non-critical backend component fails, the application should continue providing unaffected local functionality where possible.

### NFR-REL-005 — Storage failure visibility

Persistent storage errors must result in an explicit user-visible state rather than silently pretending that work has been saved.

## 7. Availability and offline behavior

### NFR-AVAIL-001 — Offline continuity

The application must remain usable for supported local operations when the NAS is temporarily unavailable.

### NFR-AVAIL-002 — Reconnection

Reconnection must automatically resume synchronization without requiring the user to manually reload the application unless a documented recovery condition exists.

### NFR-AVAIL-003 — Sync observability

Users must be able to determine whether local work is synchronized.

## 8. Scalability

### NFR-SCALE-001 — Multi-user isolation

The backend must isolate users' data and authorization contexts.

### NFR-SCALE-002 — Incremental growth

The architecture should allow the number of users, notes, folders, tags, attachments, and versions to grow without requiring a redesign of the core domain model.

### NFR-SCALE-003 — Resource proportionality

Resource consumption should scale primarily with active workload and stored data rather than requiring large always-on infrastructure.

## 9. Accessibility

### NFR-A11Y-001 — Keyboard operation

All core navigation and note-management actions must be operable with a keyboard.

### NFR-A11Y-002 — Focus management

Dialogs, menus, editor toolbars, navigation transitions, and unlock flows must maintain logical and visible focus.

### NFR-A11Y-003 — Contrast

Text, controls, states, and interactive elements must meet the applicable WCAG contrast requirements for the chosen accessibility target.

### NFR-A11Y-004 — Reduced motion

The UI must respect the user's reduced-motion preference.

### NFR-A11Y-005 — Semantic controls

Interactive elements must expose meaningful accessible names and states to assistive technologies.

## 10. Responsive UX

### NFR-UX-001 — Mobile usability

The application must remain fully usable on narrow touch screens without requiring horizontal scrolling for primary workflows.

### NFR-UX-002 — Tablet/laptop usability

The application should exploit available horizontal space with a sidebar and content/editor area without becoming visually dense.

### NFR-UX-003 — Consistent behavior

Responsive layouts may reorganize controls but must not introduce incompatible behavior between device classes.

## 11. Maintainability

### NFR-MAINT-001 — Documentation traceability

Architecture and implementation decisions must reference functional requirement IDs.

### NFR-MAINT-002 — Explicit contracts

Service interfaces, data contracts, events, and synchronization protocols must be explicitly documented.

### NFR-MAINT-003 — Migration discipline

Changes to persistent data structures or cryptographic formats must include a documented migration strategy before implementation.

### NFR-MAINT-004 — ADR discipline

Material architectural/security decisions must be recorded as Architecture Decision Records.

## 12. Observability

### NFR-OBS-001 — Operational logging

The backend must provide enough structured logging to diagnose operational problems without recording private plaintext or cryptographic secrets.

### NFR-OBS-002 — Correlation

Distributed operations should expose safe correlation identifiers to trace requests across services without exposing sensitive content.

### NFR-OBS-003 — Health state

Operational health endpoints/indicators should allow the NAS operator to determine whether required services are functioning.

## 13. Compatibility

### NFR-COMP-001 — Modern browsers

The application targets current major browsers supporting the required PWA, WebAuthn, IndexedDB, Web Crypto and related capabilities.

### NFR-COMP-002 — Capability detection

Features such as passkeys, Web Push, and certain PWA capabilities must be detected rather than assumed. Unsupported capabilities must degrade gracefully.

### NFR-COMP-003 — Platform authenticators

The product should support platform authenticators exposed through standards-based browser APIs, including Face ID, Touch ID and Windows Hello where available.

## 14. Security-sensitive timers

### NFR-TIME-001 — Private-note unlock timeout

The defined private-note unlock lifetime is 15 minutes.

### NFR-TIME-002 — Recovery key lifetime

The temporary recovery key lifetime is exactly 10 minutes from issuance, subject to the final security protocol defining when the validity window starts.

## 15. Backup boundary

### NFR-BACKUP-001 — Infrastructure responsibility

Nymbus V1 does not implement application-level backups. The deployment documentation must clearly identify the persistent data volumes/configuration that the NAS administrator must include in their own backup strategy.

This requirement does not mean Nymbus can assume backups exist; it means backup orchestration is outside the application boundary.

## 16. Security and quality acceptance

A release must not be considered production-ready if it meets functional requirements but fails a mandatory security, data-integrity, authorization, offline, or cryptographic requirement.
