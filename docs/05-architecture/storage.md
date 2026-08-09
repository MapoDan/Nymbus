# Nymbus — Storage Architecture

## 1. Durable server storage

V1 uses a relational database for structured application state and filesystem/object storage for large binary objects.

## 2. Database responsibilities

The database stores users, permissions, note metadata, folders, tags, synchronization state, version references, attachment metadata, notification state and protected cryptographic envelopes.

Private note plaintext must never be stored as a database field.

## 3. Binary storage

Attachments and other large objects are stored outside normal relational rows. For private notes, stored objects are encrypted ciphertext.

## 4. Local browser storage

The PWA uses browser-local persistence for cached metadata, encrypted content, synchronization queues and approved protected key material.

## 5. Backup

Backups must include database state, encrypted binary storage and all protected key-envelope metadata required for restoration.

## 6. Atomicity

Database metadata and object lifecycle operations must define recovery behavior when one side succeeds and the other fails.

## 7. Retention

Storage retention is governed by version-history, trash and retention requirements rather than by arbitrary cleanup jobs.

## 8. NAS optimization

Storage operations should minimize unnecessary duplication, temporary files and repeated full-object rewrites. Large objects should be streamed where practical.
