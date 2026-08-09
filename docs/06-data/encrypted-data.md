# Nymbus — Encrypted Data

**Status:** V1 baseline

## 1. Encryption boundary

Private note body, private inline media, private attachments and private document versions are stored/transferred as ciphertext.

The backend may store non-secret cryptographic metadata required to interpret ciphertext, such as version identifiers and key-envelope references.

## 2. Metadata boundary

Private note title and tags are intentionally server-readable in V1. They remain searchable while the note is locked.

Synchronization status and other explicitly approved organizational metadata are also server-readable.

## 3. Ciphertext record

A protected content record must identify:

- logical note/version ID;
- encryption scheme/version identifier;
- nonce/IV representation as required by the selected primitive;
- ciphertext;
- authentication/tag data as required by the primitive;
- key-envelope reference;
- creation/version information needed for migration.

No secret key is embedded in the ordinary content record.

## 4. No plaintext derivations

The server must not generate plaintext snippets, search indexes, previews or embeddings from private content.

## 5. Client lifecycle

Decryption occurs only in an authorized client context. Plaintext should remain in memory only for as long as required by the UI/editor operation.

## 6. Sync

Encrypted content may be synchronized while locked. A client does not need plaintext access merely to download/store ciphertext.

## 7. Backup

Backups must preserve ciphertext together with the protected key envelopes and metadata required for restoration. Backups do not decrypt private content.

## 8. Migration

Changing an encryption format must create an explicit versioned migration path. The system must retain enough metadata to identify how existing ciphertext is interpreted.

## 9. Integrity

Confidentiality alone is insufficient. Ciphertext must be authenticated so tampering results in a cryptographic failure rather than corrupted plaintext being accepted.
