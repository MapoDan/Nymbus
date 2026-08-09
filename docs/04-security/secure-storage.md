# Nymbus — Secure Storage

**Document type:** AFU — Client and server storage security  
**Status:** V1 baseline / implementation gate  
**Last updated:** 2026-08-09

## 1. Principle

Storage location does not by itself make data secure. Every stored representation must be classified as plaintext metadata, protected ciphertext, transient plaintext or cryptographic key material.

## 2. Server storage

The NAS/backend may persist:

- approved searchable metadata;
- encrypted private-note payloads;
- encrypted attachments and images;
- protected key envelopes;
- synchronization/version metadata;
- audit/security events.

It must not persist private-note plaintext as part of normal operation.

## 3. Client storage

Browser storage such as IndexedDB, Cache Storage and service-worker caches must not be treated as an equivalent of a hardware secure enclave.

Private plaintext must be minimized and retained only for the operations that require it.

## 4. Local key material

Local cryptographic material must be stored only in protected/wrapped form where possible. The exact platform storage mechanism is subject to the final device/key-management ADR.

## 5. Cache policy

Application caches must not intentionally cache rendered private plaintext, private search indexes or decrypted attachments beyond their necessary lifecycle.

## 6. Service worker

The service worker may cache application assets and approved non-sensitive data, but must not create a generic offline cache of private plaintext.

## 7. Backup

NAS backups must preserve the encrypted representation and its required metadata/key envelopes. A backup process must not decrypt private content merely to create a backup.

## 8. Exported files

Markdown/PDF exports are plaintext outputs unless an independent user-selected protection mechanism is explicitly added. Exported files are outside the normal Nymbus E2E storage boundary.

## 9. Memory

The client should minimize plaintext lifetime in memory and avoid unnecessary copies of large decrypted documents/attachments.

Because browser runtimes do not provide guaranteed secure memory erasure, the product must not claim forensic zeroization.

## 10. Logging

No passwords, recovery keys, plaintext private content, raw note keys or equivalent secrets may be logged.

## 11. Upload/download handling

Private attachments must remain protected throughout storage and transfer. Temporary plaintext files created for processing should be minimized and securely cleaned according to platform capabilities.

## 12. Server filesystem

Filesystem permissions must restrict application data to the services that require it. Backup and maintenance processes must follow least privilege.

## 13. Security gate

The exact browser storage, local key persistence and service-worker strategy must be reconciled with the frontend/offline architecture before implementation.
