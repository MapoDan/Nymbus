# Nymbus — Logical Data Model

**Document type:** AFU — Logical data model  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Purpose

This document defines the logical entities required by Nymbus. It is intentionally technology-independent: it does not prescribe SQL schema syntax, ORM mappings or a specific database engine.

## 2. Design principles

- Structured server data uses a relational model unless an ADR proves another model necessary.
- Private content is represented as encrypted payloads, never server plaintext.
- Server-readable metadata is explicitly classified.
- User ownership and authorization are first-class relationships.
- Synchronization identifiers are distinct from human-readable names.
- Historical versions are immutable records from the application perspective.

## 3. Core entities

### User

Represents a Nymbus account.

Key attributes:

- internal user ID;
- Google subject/identity reference;
- display name/profile information required by UX;
- account status;
- timestamps;
- security configuration references.

### AuthenticationCredential

Represents a passkey/WebAuthn credential associated with a user.

Must not store biometric data.

### Session

Represents an authenticated application session/device context.

### Note

Represents a logical note.

Common metadata:

- note ID;
- owner ID;
- title;
- note type/state;
- folder ID;
- timestamps;
- favorite state;
- sync state;
- current version reference;
- private/public classification.

### NoteTag

Associates a note with a user-visible tag.

### Folder

Represents organizational hierarchy.

### Permission

Represents access granted to another user.

### NoteVersion

Represents one retained historical state/version. V1 retains exactly 15 versions per note as previously decided.

### Attachment

Represents an image or other supported asset associated with a note.

### SyncRecord

Represents server-known synchronization state/change metadata required by the selected synchronization protocol.

### Notification

Represents user-facing events such as synchronization issues or sharing/security events.

### AuditEvent

Represents security-sensitive administrative/authentication/authorization events without private content.

## 4. Private-note entities

Private notes require additional protected representations, conceptually including:

- encrypted content blob;
- encrypted attachment data;
- note-key envelope(s);
- key identifiers;
- cryptographic version/algorithm metadata;
- KDF metadata where a password-derived protection path is used.

Secret key material itself must not be stored in plaintext server fields.

## 5. Metadata classification

### Server-readable

Potentially server-readable:

- title;
- tags;
- folder;
- favorite;
- owner;
- sharing metadata;
- synchronization metadata;
- version timestamps/identifiers;
- synchronization status.

### Client-only protected

For private notes:

- body;
- inline images;
- protected attachments;
- private search index;
- content-derived metadata not explicitly approved as server-readable.

## 6. Relationships

Conceptually:

```text
User 1 ─── N Note
User 1 ─── N Folder
User 1 ─── N Tag
User 1 ─── N AuthenticationCredential
Note 1 ─── N NoteVersion
Note 1 ─── N Attachment
Note N ─── N Tag
Note N ─── N User (through Permission)
```

## 7. Folder hierarchy

Folders may form a tree with a nullable parent reference.

The system must prevent cycles and define behavior when a parent folder is deleted, moved or becomes inaccessible.

## 8. Tags

Tags are user-created metadata used for organization and search.

Tags do not independently grant access.

## 9. Sharing

A permission record must identify:

- target user;
- resource;
- role/capability set;
- grant actor;
- creation timestamp;
- revocation timestamp/state where applicable.

Private-note cryptographic key access must be represented separately from ordinary authorization metadata.

## 10. Version retention

Each note retains a maximum of 15 versions.

When a new version exceeds the retention limit, the oldest eligible version is removed according to the version-retention policy.

The policy must account for versions needed by active synchronization/concurrency operations before deletion.

## 11. Synchronization identifiers

Synchronization must use stable IDs and causal/version metadata rather than timestamps alone for conflict resolution.

Client-generated IDs should be supported where offline creation is required.

## 12. Soft deletion

The synchronization design may require tombstones for deleted resources so other offline clients can learn about deletion.

Tombstone retention must be bounded and documented.

## 13. Search metadata

Metadata search must be possible while a private note is locked, according to the agreed product requirement.

The data model therefore keeps approved metadata independently addressable from encrypted body content.

## 14. Audit data

Audit records must be append-oriented and must never contain note plaintext, passwords, recovery keys or encryption keys.

## 15. Data lifecycle

Every entity must eventually have documented behavior for:

- creation;
- modification;
- deletion;
- retention;
- synchronization;
- backup/restore;
- authorization changes.

## 16. Schema evolution

Data schema changes must be versioned and backward-compatible with the supported migration strategy. Destructive migrations require explicit review and a rollback/recovery plan.
