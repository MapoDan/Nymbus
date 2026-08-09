# Nymbus — Encryption Architecture

**Document type:** AFU — End-to-end encryption model  
**Status:** V1 baseline / security design  
**Last updated:** 2026-08-09

> This document defines the required security properties and key relationships. Concrete cryptographic library/API choices are intentionally deferred to implementation ADRs and must use well-reviewed standard primitives.

## 1. Objective

Private notes must remain end-to-end encrypted such that the Nymbus backend can store, synchronize and authorize them without possessing the capability to decrypt their plaintext during normal operation.

## 2. Important terminology

- **Account authentication:** proves the user's identity to Nymbus.
- **Master password:** user secret used in the private-note key hierarchy.
- **Note password:** optional dedicated secret protecting an individual note.
- **Note key:** symmetric key protecting a specific note's encrypted content.
- **Key encryption/wrapping key:** key used to protect another key rather than document content directly.
- **Recovery mechanism:** controlled process that restores access to the user's key hierarchy after the normal unlock path is unavailable.
- **Platform authenticator:** WebAuthn/device capability such as Face ID, Touch ID or Windows Hello.

## 3. Core model

The preferred model is envelope encryption:

```text
                         Account / user key hierarchy
                                  │
                     ┌────────────┴────────────┐
                     │                         │
              Master-derived             Recovery path
                protection                     │
                     │                         │
                     └──────────┬──────────────┘
                                │
                         User/key-encryption
                              material
                                │
                   ┌────────────┼─────────────┐
                   │            │             │
                Note A       Note B        Note C
                key          key           key
                   │            │             │
              encrypted      encrypted     encrypted
                content        content       content
```

The exact hierarchy must be finalized before implementation because it determines whether the requested password/passkey/recovery behaviors can coexist without weakening E2E confidentiality.

## 4. Per-note encryption

Each private note should have independent content-encryption material rather than encrypting every note directly with one reusable master-derived key.

Benefits:

- compromise of one note key does not automatically expose every note;
- sharing can be modeled at note-key level;
- revocation can trigger controlled re-keying;
- large notes can be encrypted independently;
- encrypted content can remain opaque to the server.

## 5. Metadata boundary

The following may remain server-readable where required by product behavior:

- note identifier;
- title;
- tags;
- folder association;
- favorite state;
- permission metadata;
- synchronization metadata;
- version metadata necessary for synchronization;
- timestamps required by the application.

The exact metadata classification must be finalized in the data model.

The following must remain protected for private notes:

- body plaintext;
- inline image plaintext;
- private attachments;
- private content-derived search index;
- any secret fields embedded in the note body.

## 6. Encryption at rest vs E2E

Database/storage encryption is not equivalent to E2E encryption.

A database-encryption key available to the backend protects disks/volumes but does not prevent a compromised backend from decrypting stored data.

Therefore private-note content requires client-side encryption before it reaches server persistence.

## 7. Authentication is not decryption

Google login and passkey login establish account identity. They must not by themselves be treated as proof that private-note content may be decrypted.

A successful account login therefore results in:

```text
Authenticated account
       ≠
Unlocked private-note key hierarchy
```

## 8. First unlock

For a newly initialized private note, the user must perform the required password-based initialization before convenient platform-authenticator unlock can be enabled.

This ensures that the initial cryptographic relationship is established using a user-controlled secret rather than assuming that an account session is sufficient.

## 9. Master-password option

The product requirement allows a user to protect the note using their own master password.

The architecture must avoid storing the raw master password. A memory-hard password-based key derivation function must derive cryptographic material from it.

The master password must never be sent to the backend.

## 10. Dedicated note-password option

A user may choose a dedicated password for an individual private note.

That password must protect the relevant note-key access path rather than directly encrypting the entire document with a password-derived key in a way that makes future key rotation unnecessarily difficult.

The raw note password must never be sent to the backend.

## 11. Platform-authenticator unlock

After first-time initialization, the user may authorize convenient unlock using the platform authenticator.

The architecture should use WebAuthn/platform capabilities to protect access to a locally held key-encryption capability rather than transmitting biometric information to Nymbus.

Nymbus must never receive:

- Face ID biometric data;
- Touch ID biometric data;
- Windows Hello biometric data;
- the device's biometric template.

The platform authenticator only signals successful local user verification and performs cryptographic operations according to the platform/WebAuthn model.

## 12. Critical browser limitation

Web applications do not receive raw biometric data from Face ID, Touch ID or Windows Hello. Therefore the implementation must not describe the feature as Nymbus storing or reading biometrics.

The correct conceptual model is:

```text
Nymbus asks platform to authenticate
          ↓
Platform verifies user locally
          ↓
Platform/browser performs approved credential operation
          ↓
Nymbus receives cryptographic/authentication result
```

## 13. Unlock timeout

Private-note plaintext/key availability is limited to the configured 15-minute unlock lifetime.

When the lifetime expires:

- private content must become inaccessible;
- locally retained key material must be invalidated/evicted according to the key-management policy;
- the user must perform the required unlock operation again.

The account session may remain valid independently.

## 14. Bulk unlock

Bulk unlock is permitted only for notes that have already completed first-time password initialization.

Bulk unlock must not:

- bypass a note's initial password requirement;
- upload plaintext to the backend;
- permanently convert note-specific keys into one global plaintext key;
- remove note-level authorization.

The client may temporarily obtain access to multiple note keys for the 15-minute unlock session.

## 15. Private search

When locked, private plaintext is unavailable to the search subsystem.

After unlock, the client may construct/use a local search index. That index is sensitive data and must be protected with the same local key lifecycle as private plaintext.

The private search index must not be uploaded to the server as plaintext.

## 16. Inline images and attachments

Private inline images and private attachments are encrypted independently or as authenticated encrypted objects associated with the note-key hierarchy.

The backend may store ciphertext and metadata but must not require plaintext to serve the authorized client.

## 17. Version history

Historical versions of private notes are encrypted under the private-note cryptographic boundary.

A version must not become plaintext merely because it is old or because an administrator accesses the database.

## 18. Sharing

Sharing a private note requires granting the recipient access to the note's cryptographic key material through an authenticated, authorized key exchange/wrapping mechanism.

The backend may coordinate the exchange but must not learn the plaintext key in a form that defeats the E2E model.

## 19. Revocation

Revocation has two distinct meanings:

1. server authorization revocation — the user can no longer request future protected operations;
2. cryptographic revocation — future content/key generations prevent the revoked user from legitimately obtaining new access.

Nymbus cannot retroactively erase plaintext that a recipient already copied.

Where immediate cryptographic revocation is required, the system must re-key affected content or use an equivalent forward-secrecy/key-rotation mechanism defined by the final protocol.

## 20. Recovery

Recovery is the most delicate part of the model.

A 10-minute recovery key delivered by email must not itself be a permanent copy of the user's master password or a long-lived universal decryption key.

The preferred design direction is a short-lived, authenticated recovery capability that allows the user to establish a new local key-encryption path while preserving the E2E property.

The exact recovery protocol must be validated by security review before implementation.

## 21. Cryptographic integrity

All encrypted private content must use authenticated encryption or an equivalent construction providing confidentiality and integrity.

Tampered ciphertext must fail authentication and must never be presented as valid plaintext.

## 22. Key separation

Keys used for different purposes should be cryptographically separated. Authentication/session keys must not be reused as content-encryption keys.

## 23. Cryptographic primitives

The implementation must use established, well-reviewed primitives available through maintained platform/library APIs.

The project must not invent custom encryption algorithms, custom password hashing algorithms or custom authenticated-encryption schemes.

The exact primitive suite must be selected through a security ADR before implementation.

## 24. Explicit non-goals

V1 does not claim protection against:

- a fully compromised endpoint while plaintext is displayed;
- a malicious browser extension with access to page content;
- screenshots or photographs of decrypted notes;
- a user intentionally exporting/copying plaintext;
- an attacker controlling the user's device at the OS level.

## 25. Security review gate

Implementation must not begin for the cryptographic key hierarchy until the following questions have explicit answers in `key-management.md`:

1. What is the root of trust for a new account?
2. What exactly is derived from the master password?
3. How is a dedicated note password represented?
4. How is platform-authenticator access bound to the local key hierarchy?
5. What survives browser restart?
6. What survives device change?
7. What does recovery restore?
8. How is a shared note re-keyed after revocation?
9. What happens when one device is lost?
10. How are all local private indexes invalidated after timeout?
