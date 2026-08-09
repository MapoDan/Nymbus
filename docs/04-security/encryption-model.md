# Nymbus — Encryption Model

**Document type:** AFU — End-to-end encryption model  
**Status:** V1 baseline / security validation required  
**Last updated:** 2026-08-09

## 1. Objective

Private-note content must be encrypted end-to-end so that the Nymbus backend and NAS persist and synchronize ciphertext without requiring access to plaintext.

## 2. Encryption boundary

For a private note, encryption covers at minimum:

- Markdown/document content;
- inline images;
- attachments;
- encrypted version payloads;
- any future content explicitly classified as private.

Title, tags and other explicitly approved metadata remain outside this boundary because they are required for server-side organization and metadata search.

## 3. Per-note isolation

Every private note has independent symmetric content-key material. Compromise of one note key must not automatically decrypt unrelated notes.

## 4. Envelope model

The conceptual model is:

```text
Plain document
    ↓
Note content encryption
    ↓
Ciphertext + non-secret encryption metadata
    ↓
Server/NAS storage
```

The note key is protected separately through the approved key-wrapping hierarchy.

## 5. Authenticated encryption

The implementation must use an established authenticated-encryption construction providing confidentiality and integrity. Custom cryptographic algorithms are prohibited.

The exact primitive, nonce/IV construction and library are an ADR/security-review decision.

## 6. Integrity

Tampering with ciphertext, wrapped keys or relevant cryptographic metadata must be detected before plaintext is accepted.

Authentication failure must fail closed; the application must not display partially decrypted or fallback plaintext.

## 7. Nonce/IV management

Nonce/IV generation must follow the selected cryptographic primitive's requirements. Reuse conditions must be explicitly prevented and tested.

## 8. Key separation

Password-derived keys must not be reused directly as document encryption keys.

Password-derived material is used to protect/wrap key material, while random content keys protect note data.

## 9. Master password

The master password never leaves the client. A memory-hard password KDF with a unique salt and documented parameters derives protection material locally.

The final KDF and parameter set require an explicit security ADR.

## 10. Dedicated note password

A dedicated note password protects the corresponding note-key access path. Changing that password should normally require re-wrapping the note key, not re-encrypting the complete note content.

## 11. Platform authentication

Face ID, Touch ID, Windows Hello and compatible platform authentication are treated as local authorization mechanisms. Biometric templates are never handled by Nymbus.

The exact binding between WebAuthn/platform credentials and encrypted key envelopes requires an explicit ADR.

## 12. Metadata leakage

The encryption model intentionally does not hide approved metadata. This means a backend/storage operator may observe titles, tags and other fields classified as searchable metadata.

This is a known and accepted V1 trade-off.

## 13. Search

Server-side search operates only on permitted metadata. Private plaintext search is client-side after authorized decryption.

## 14. Attachments

Private attachments use the same cryptographic trust boundary as the note body. Binary data must not be uploaded as plaintext and encrypted later by the backend.

## 15. Versions

Each retained private version must remain cryptographically protected. The versioning design must define whether keys are shared between versions or rotated per generation.

## 16. Sharing

A shared private note uses the same underlying note-content confidentiality model. Sharing adds protected key-access records for authorized recipients; it does not make the backend a decryption authority.

## 17. Re-keying

Key rotation may be required after membership changes. Re-keying must not require unnecessarily re-encrypting all historical data when the selected model can avoid it safely.

## 18. Export boundary

When a private note is exported to Markdown or PDF, the resulting file is plaintext output and is outside Nymbus's E2E storage boundary unless the user separately protects it.

## 19. Recovery boundary

Recovery must restore an authorized key path without transmitting permanent private keys or passwords through email.

## 20. Prohibited designs

The implementation must not:

- encrypt private notes only at the database level and call that E2E;
- send master passwords to the server;
- derive note ciphertext directly from a user password without proper key separation;
- store raw note keys on the backend;
- provide an administrator decryption endpoint;
- index private plaintext server-side;
- silently fall back to plaintext when decryption fails.
