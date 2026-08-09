# Nymbus — Database Schema

**Status:** V1 logical schema baseline

## 1. Purpose

This specification defines the database responsibilities without locking Nymbus to a specific SQL engine.

## 2. Relational persistence

The primary metadata store should be relational because Nymbus requires:

- strong referential integrity;
- permissions queries;
- tag/folder relationships;
- transactional updates;
- synchronization cursors;
- efficient filtering and pagination.

## 3. Required table families

The physical schema must cover at least:

- users and identities;
- devices and sessions;
- notes and folders;
- tags and note_tags;
- note_versions;
- attachments;
- permissions;
- key envelopes;
- sync operations/state;
- notifications;
- trash/retention;
- recovery transactions.

## 4. Identifiers

Stable opaque IDs are preferred over exposing sequential database IDs as public resource identifiers.

## 5. Constraints

The physical schema must enforce uniqueness and foreign-key constraints wherever practical. Business authorization must remain application-enforced even when database constraints exist.

## 6. Transactions

Operations that change multiple related server-readable records must be atomic where required. Examples include permission changes, trash transitions and metadata updates.

Private-content encryption occurs client-side; the database transaction must never require plaintext note content.

## 7. Indexing

Indexes must target real access patterns: owner, folder, tags, timestamps, synchronization state, resource IDs and permission lookups.

Do not create indexes over private plaintext because it must never exist server-side.

## 8. Payload size

Large binary content should not be stored directly in ordinary metadata rows. Attachments and large encrypted content should use the storage boundary defined by `attachments.md` and `storage.md`.

## 9. Schema migrations

Every schema change must be versioned and reversible where practical. Migrations must not silently discard encrypted key metadata or synchronization state.

## 10. Resource constraints

The schema must remain efficient for a small NAS deployment. Avoid unnecessary denormalized copies and infrastructure-dependent indexing systems unless profiling demonstrates a need.
