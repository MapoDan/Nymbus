# Nymbus — Requirements Traceability

**Document type:** AFU — Requirements traceability matrix  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Purpose

This document defines the traceability model used to ensure that every important product requirement is represented consistently across functional analysis, use cases, acceptance criteria, architecture, implementation and tests.

The final implementation must be traceable backwards from code to a documented requirement and forwards from a requirement to verification evidence.

## 2. Traceability chain

The intended chain is:

```text
Product goal
    ↓
Functional / non-functional requirement
    ↓
User story
    ↓
Use case
    ↓
Acceptance criterion
    ↓
Architecture decision / component
    ↓
API / data contract / UX specification
    ↓
Implementation unit
    ↓
Automated or manual test
```

Not every requirement needs every intermediate artifact. Security and synchronization requirements, however, should have particularly complete traceability.

## 3. Current source documents

| Layer | Document |
|---|---|
| Vision | `docs/00-project/vision.md` |
| Scope | `docs/00-project/scope.md` |
| Principles | `docs/00-project/principles.md` |
| Glossary | `docs/00-project/glossary.md` |
| Product requirements | `docs/01-product/product-requirements.md` |
| Non-functional requirements | `docs/01-product/non-functional-requirements.md` |
| Use cases | `docs/01-product/use-cases.md` |
| User stories | `docs/01-product/user-stories.md` |
| Acceptance criteria | `docs/01-product/acceptance-criteria.md` |

## 4. Traceability rules

### RT-001 — Every MUST requirement has verification

Every `MUST` requirement must have at least one acceptance criterion and at least one verification method before V1 release.

### RT-002 — Security requirements require dedicated verification

Security requirements must not be considered verified solely because a general functional test passes. They require security-focused verification where applicable.

### RT-003 — Requirement IDs are stable

Requirement IDs must not be reused for unrelated functionality.

### RT-004 — Changes propagate

If a requirement changes, affected user stories, use cases, acceptance criteria, architecture decisions, API contracts, UX behavior and tests must be reviewed.

### RT-005 — Out-of-scope requirements remain visible

Deferred features remain documented as `OUT` or V2 candidates rather than being silently removed.

## 5. High-level traceability matrix

| Domain | Requirements | User stories | Use cases | Acceptance criteria |
|---|---|---|---|---|
| Authentication | `FR-AUTH-*` | `US-AUTH-*` | `UC-AUTH-*` | `AC-AUTH-*` |
| Notes/editor | `FR-NOTE-*` | `US-NOTE-*` | `UC-NOTE-*` | `AC-NOTE-*` |
| Private notes | `FR-PRIVATE-*` | `US-PRIVATE-*` | `UC-PRIVATE-*` | `AC-PRIVATE-*` |
| Sharing | `FR-SHARE-*` | `US-SHARE-*` | `UC-SHARE-*` | `AC-SHARE-*` |
| Search | `FR-SEARCH-*` | `US-SEARCH-*` | `UC-SEARCH-*` | `AC-SEARCH-*` |
| Synchronization | `FR-SYNC-*` | `US-SYNC-*` | `UC-SYNC-*` | `AC-SYNC-*` |
| Versioning | `FR-VERSION-*` | `US-VERSION-*` | `UC-VERSION-*` | `AC-VERSION-*` |
| Export | `FR-EXPORT-*` | `US-EXPORT-*` | `UC-EXPORT-*` | `AC-EXPORT-*` |
| Notifications | `FR-NOTIF-*` | `US-NOTIF-*` | `UC-NOTIF-*` | `AC-NOTIF-*` |
| Administration | `FR-ADMIN-*` | `US-ADMIN-*` | `UC-ADMIN-*` | `AC-ADMIN-*` |
| UI | `FR-UI-*` | `US-QUALITY-*` | Cross-cutting UI cases | `AC-UI-*` |
| Infrastructure | `FR-INFRA-*` | `US-QUALITY-*` | Infrastructure/deployment cases | `AC-INFRA-*` |
| Non-functional | `NFR-*` | Quality stories | Cross-cutting | Applicable quality/security criteria |

## 6. Critical security traceability

The following areas require complete end-to-end traceability:

### Encryption boundary

```text
FR-PRIVATE-002
→ US-PRIVATE-001 / US-PRIVATE-009
→ UC-PRIVATE-01 / UC-PRIVATE-03 / UC-PRIVATE-04
→ AC-PRIVATE-001 / AC-PRIVATE-009
→ Cryptographic architecture
→ Encryption/key-management implementation
→ Security tests
```

### First unlock

```text
FR-PRIVATE-004
→ US-PRIVATE-003
→ UC-PRIVATE-01
→ AC-PRIVATE-003
→ Key initialization architecture
→ Unlock implementation
→ Negative security tests
```

### Bulk unlock

```text
FR-PRIVATE-008
→ US-PRIVATE-007 / US-PRIVATE-008
→ UC-PRIVATE-03
→ AC-PRIVATE-007 / AC-PRIVATE-008
→ Local key-management architecture
→ Bulk-unlock implementation
→ Eligibility and bypass tests
```

### Recovery

```text
FR-PRIVATE-011 / FR-PRIVATE-012
→ US-PRIVATE-010 / US-PRIVATE-011
→ UC-PRIVATE-05
→ AC-PRIVATE-010 / AC-PRIVATE-011 / AC-PRIVATE-012
→ Recovery/key-management architecture
→ Recovery implementation
→ Expiration, replay and confidentiality tests
```

### Revocation

```text
FR-SHARE-007 / FR-SHARE-008
→ US-SHARE-005
→ UC-SHARE-02
→ AC-SHARE-006 / AC-SHARE-007 / AC-SHARE-008
→ Authorization + cryptographic revocation architecture
→ Revocation implementation
→ Access and re-keying tests
```

### Offline synchronization

```text
FR-SYNC-001..007
→ US-SYNC-001..005
→ UC-SYNC-01 / UC-COLLAB-01
→ AC-SYNC-001..006
→ Synchronization architecture
→ Local persistence + CRDT + realtime implementation
→ Offline/concurrency/recovery tests
```

## 7. Verification status model

Each future requirement/test mapping should use one of these statuses:

- `NOT_STARTED` — no verification work yet.
- `SPECIFIED` — acceptance criteria exist.
- `IMPLEMENTED` — implementation exists.
- `TESTED` — verification passed.
- `BLOCKED` — verification cannot proceed due to a known dependency.
- `FAILED` — verification exists but currently fails.
- `DEFERRED` — intentionally postponed by approved scope decision.

## 8. Definition of requirement completion

A mandatory requirement is complete only when:

1. its meaning is documented;
2. its affected use cases are documented;
3. its acceptance criteria are documented;
4. its architectural impact has been resolved;
5. its implementation is complete;
6. its required tests pass;
7. security implications have been reviewed where applicable;
8. documentation remains synchronized with actual behavior.

## 9. AI development rule

A coding agent must not infer that an undocumented behavior is required merely because another application implements it.

If the agent encounters a feature necessary to implement a requirement but not yet specified, it must identify the gap and request/record a decision before introducing a behavior that affects security, persistence, authorization, synchronization, or user-visible contracts.

## 10. Future expansion

As architecture and technical documentation are added, this matrix should be extended with columns for:

- ADR;
- service/component;
- API endpoint/event;
- data entity;
- UI specification;
- automated test ID;
- security test ID;
- performance test ID;
- release/milestone.

The matrix should remain a navigational index, not a duplicate of every detailed specification.
