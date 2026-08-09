# Nymbus — Data Model

**Document type:** AFU — Logical data model
**Status:** V1 baseline
**Last updated:** 2026-08-09

## 1. Purpose

This document defines the logical data model that all persistence, API and synchronization specifications must respect. It deliberately does not prescribe a concrete database engine or ORM.

## 2. Data domains

Nymbus data is divided into:

1. identity and account data;
2. authorization and sharing data;
3. note/document metadata;
4. encrypted private content;
5. attachments;
6. synchronization state;
7. version/history data;
8. device/session/security data;
9. notifications and administrative data.

## 3. Plaintext versus protected data

The model must explicitly classify every field as one of:

- **server-readable metadata**;
- **encrypted content/ciphertext**;
- **cryptographic envelope/key metadata**;
- **ephemeral client plaintext**.

No field may be implicitly treated as private merely because it belongs to a private note.

## 4. Core entities

The logical model includes, at minimum:

- User;
- Identity;
- Device;
- Session;
- Note;
- NoteVersion;
- Folder;
- Tag;
- NoteTag;
- Attachment;
- NotePermission;
- NoteKeyEnvelope;
- SyncOperation;
- SyncCursor/state;
- Notification;
- TrashEntry;
- RecoveryTransaction.

Additional entities may be introduced only when they represent a real persistence or lifecycle boundary.

## 5. User ownership

Every user-owned resource must have an unambiguous owner or owning scope. Ownership is distinct from sharing permissions.

## 6. Note identity

A note has a stable immutable identifier independent of its title, folder, encryption generation or version.

Moving, renaming, encrypting again or restoring a note must not create a new logical note identity.

## 7. Metadata

A note's server-readable metadata may include title, tags, folder association, favorite state, timestamps, synchronization state and other explicitly approved organizational fields.

Metadata must not accidentally include snippets or derived plaintext from private content.

## 8. Content

Plain notes may persist their content according to the normal document model. Private notes persist encrypted content and associated cryptographic metadata only.

## 9. Versioning

User-visible note history is limited to exactly 15 versions in V1. Internal synchronization state/history may have different retention rules where necessary for convergence and recovery.

## 10. Relationships

Relationships must use stable IDs rather than embedding duplicated mutable objects where this would cause synchronization ambiguity.

## 11. Soft deletion

Deletion and trash state must be modeled independently from permanent physical deletion so synchronization and restoration remain deterministic.

## 12. Auditability

Security-sensitive state changes must retain sufficient event information for audit without persisting secrets or private plaintext.

## 13. Data-model invariants

- A note cannot have two owners.
- A tag assignment cannot exist without a valid note and tag.
- A permission cannot grant access to a deleted/nonexistent note.
- A note version belongs to exactly one logical note.
- A private note's content record must never contain plaintext.
- An attachment must belong to a valid owning resource.
- A key envelope must identify the protected key generation without exposing its secret.
- Sync operations must be uniquely identifiable and replay-safe.

## 14. Evolution

Schema evolution must be versioned. Destructive migrations require explicit migration and rollback/recovery procedures.
