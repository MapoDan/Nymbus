# Nymbus — Storage Architecture

**Document type:** AFU — Persistence and storage architecture  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Goals

Storage must be:

- lightweight for a low-capacity NAS;
- reliable;
- backup-friendly;
- recoverable;
- suitable for concurrent users;
- compatible with encrypted private content;
- efficient for note metadata and synchronization.

## 2. Relational database

V1 should use one lightweight relational database as the authoritative store for structured application data unless an ADR demonstrates a concrete need for another database technology.

The database should contain:

- users;
- authentication metadata;
- notes and metadata;
- folders;
- tags;
- permissions;
- versions;
- synchronization metadata;
- notifications;
- audit events;
- references to stored assets;
- encrypted note/key envelopes as appropriate.

## 3. Object/blob storage

Images and binary attachments should be stored outside normal relational rows where their size and access pattern make that preferable.

The relational database stores references and metadata.

For private notes, stored assets must be encrypted according to the note's cryptographic boundary before persistent server storage.

## 4. NAS filesystem

The deployment must define explicit persistent volumes for:

- database;
- application-required persistent state;
- attachment/blob storage;
- backup/export staging if applicable.

Temporary processing data must use bounded storage and have cleanup rules.

## 5. No unnecessary distributed storage

V1 does not require:

- Elasticsearch/OpenSearch;
- Redis solely as a cache;
- Kafka/RabbitMQ;
- distributed object storage;
- separate analytics database.

Each would increase memory, operational complexity and backup requirements.

## 6. Database encryption

Database/storage-level encryption may be used as defense in depth, but it does not replace client-side E2E encryption of private content.

## 7. Transactions

Operations modifying multiple authoritative relational records must use appropriate transaction boundaries.

A transaction should not be held open while waiting for a slow external network operation unless unavoidable.

## 8. Asset lifecycle

Assets must have defined states such as:

- pending;
- active;
- orphaned;
- deleted.

Cleanup must be safe in the presence of interrupted uploads and offline synchronization.

## 9. Orphan handling

If an upload completes but the note reference is never committed, the asset must eventually become eligible for cleanup without deleting assets that are still referenced by an offline client.

## 10. Backup

The application must not perform autonomous backup scheduling.

NAS-level backup procedures must include every persistent component required to restore:

- relational data;
- encrypted note content;
- encrypted private attachments;
- key envelopes;
- synchronization state required for consistency;
- application configuration required for restoration.

## 11. Restore

A restore is not considered successful merely because the database starts.

A valid restore test must verify that:

1. normal notes are readable;
2. private-note ciphertext is present;
3. required cryptographic envelopes are present;
4. authorized users can recover private-note access;
5. synchronization state is internally consistent.

## 12. Capacity management

The system should expose or document storage consumption by:

- database;
- attachments;
- versions;
- encrypted content;
- temporary files.

Version retention and asset cleanup must prevent uncontrolled growth.

## 13. Database maintenance

Maintenance jobs must be bounded and lightweight. They should not continuously consume CPU on the NAS.

## 14. Concurrency

The chosen database must safely support multiple simultaneous users and synchronization workers without requiring a heavyweight distributed deployment.

## 15. Migration strategy

Database migrations must be explicit, versioned and repeatable. An application upgrade must never assume that an empty database is the only valid starting state.
