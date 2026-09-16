# Nymbus — Encryption Architecture

**Document type:** AFU — End-to-end encryption model  
**Status:** V1 baseline / security design  
**Last updated:** 2026-09-16

> This document defines the required security properties and key relationships. Concrete cryptographic library/API choices are intentionally deferred to implementation ADRs and must use well-reviewed standard primitives.

## 1. Objective

Private notes must remain end-to-end encrypted such that the Nymbus backend can store, synchronize and authorize them without possessing the capability to decrypt their plaintext during normal operation.

## 2. Important terminology

- **Account authentication:** proves the user's identity to Nymbus.
- **Master password:** the user's cryptographic secret for the private-note key hierarchy. It is established during initial application activation through the approved recovery/activation process initiated by an administrator for the user.
- **Note key:** symmetric key protecting a specific private note's encrypted content.
- **Key encryption/wrapping key:** key used to protect another key rather than document content directly.
- **Recovery mechanism:** controlled process used during initial activation and, where defined, later recovery to establish or restore the user's master-password-protected key hierarchy.
- **Platform authenticator:** WebAuthn/device capability such as Face ID, Touch ID or Windows Hello.

**Removed concept:** V1 has no dedicated per-note password and no temporary note password. All private notes are protected through the user's master-password key hierarchy.

## 3. Core model

The preferred model is envelope encryption:

```text
                         User cryptographic hierarchy
                                  │
                         Master-password path
                                  │
                         User key-encryption
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

For a shared private note, the same note key is made accessible through separate protected key-access records for the owner and every authorized recipient. The backend must never receive the plaintext note key as a universal server-readable secret.

## 4. Per-note encryption

Each private note must have independent content-encryption material rather than encrypting every note directly with one reusable master-derived key.

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

Database encryption protects stored data from disk/volume compromise but does not prevent a compromised backend from decrypting data for which the backend possesses the relevant keys.

Therefore private-note content requires client-side encryption before it reaches server persistence.

## 7. Authentication is not decryption

Google login and passkey login establish account identity. They must not by themselves be treated as proof that private-note content may be decrypted.

A successful account login therefore results in:

```text
Authenticated account
       ≠
Unlocked private-note key hierarchy
```

## 8. Master password establishment

The master password is established once during the user's initial Nymbus application activation. The activation/recovery process is initiated by the Nymbus administrator for the relevant user and must be explicitly authenticated and authorized.

The master password:

- is chosen/entered by the user through the approved client flow;
- is never sent to the Nymbus backend;
- is never logged or stored in plaintext;
- is processed through the approved memory-hard password KDF;
- protects the user's root/key-encryption material through the defined envelope hierarchy.

The administrator must not receive the master password or any plaintext private-note key as part of the activation process.

## 9. Private-note initialization and first unlock

A private note is always initialized against the user's existing master-password key hierarchy. There is no separate note password.

When a note becomes private:

1. the client must have an authorized local master-password-derived key path available;
2. the client generates the note key locally;
3. the client creates the protected note-key envelope for the owner;
4. private content is encrypted locally using the note key;
5. ciphertext and protected key-access metadata are synchronized to the backend.

The first password-based access to a newly initialized private note must use the user's master password. A passkey/platform authenticator, bulk unlock or authenticated account session must not replace the required master-password initialization path.

## 10. Master password and note keys

The master password must not directly encrypt note bodies. A memory-hard password KDF derives the required cryptographic material locally. The resulting hierarchy protects individual randomly generated note keys.

Changing the master password must re-wrap the user's root/key-encryption material rather than re-encrypting every note body, provided the approved key hierarchy permits this.

## 11. Platform-authenticator unlock

After the user's master-password key hierarchy has been initialized and the relevant private note has completed its first password-based initialization, the user may authorize convenient local unlock using a platform authenticator.

The implementation must bind the authenticator to a locally held key-access capability. The authenticator must not become a server-side substitute for the user's cryptographic root.

Nymbus must never receive:

- Face ID biometric data;
- Touch ID biometric data;
- Windows Hello biometric data;
- the device's biometric template;
- the device PIN/passcode.

## 12. Critical browser limitation

Web applications do not receive raw biometric data from Face ID, Touch ID or Windows Hello. Therefore the implementation must not describe the feature as Nymbus storing or reading biometrics.

## 13. Unlock timeout

Private-note plaintext/key availability is limited to the configured 15-minute unlock lifetime.

When the lifetime expires:

- private content must become inaccessible;
- locally retained key material must be invalidated/evicted according to the key-management policy;
- the user must perform the required unlock operation again.

The account session may remain valid independently.

## 14. Bulk unlock

Bulk unlock is permitted only for notes whose private protection has already been initialized.

Bulk unlock must not:

- bypass master-password initialization;
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

Sharing a private note grants each authorized Nymbus user access to the note key through a separate protected key-access record.

Conceptually:

```text
Private Note Key
   ├── protected for Owner master-key path
   ├── protected for User A master-key path
   └── protected for User B master-key path
```

The owner's master password is never transmitted to recipients. Each recipient uses their own master-password key hierarchy to access their protected copy/wrapping of the note key.

## 19. Revocation

Revocation has two distinct meanings:

1. server authorization revocation — the user can no longer request future protected operations;
2. cryptographic revocation — future content/key generations prevent the revoked user from legitimately obtaining new access.

For a shared private note, revoking a recipient must remove their server authorization and, where required to prevent access to future content, rotate the note key and distribute the new key only to remaining authorized users.

Nymbus cannot retroactively erase plaintext that a recipient already copied, exported or otherwise extracted while authorized.

Historical-version access after revocation must follow the explicit version/re-key policy defined by the security ADR.

## 20. Recovery

The initial activation/recovery mechanism may be initiated by the administrator for a user and must result in the user establishing their own master password without the administrator learning it.

Any later recovery mechanism must not email the master password or a permanent universal decryption key. The exact later recovery protocol must be validated by security review before implementation.

## 21. Cryptographic integrity

All encrypted private content must use authenticated encryption or an equivalent construction providing confidentiality and integrity.

Tampered ciphertext must fail authentication and must never be presented as valid plaintext.

## 22. Key separation

Keys used for different purposes must be cryptographically separated. Authentication/session keys must not be reused as content-encryption keys.

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

Implementation must not begin for the cryptographic key hierarchy until the following questions have explicit answers in `key-management.md` and the relevant ADRs:

1. What exactly is established during initial administrator-initiated user activation?
2. What exactly is derived from the master password?
3. How is a private note key protected for its owner?
4. How is a shared note key protected for each recipient's master-key path?
5. How is platform-authenticator access bound to the local key hierarchy?
6. What survives browser restart?
7. What survives device change?
8. What does later recovery restore, if supported?
9. How is a shared note re-keyed after revocation?
10. What happens when one device is lost?
11. How are all local private indexes invalidated after timeout?
