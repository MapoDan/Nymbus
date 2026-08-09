# Nymbus — Key Management Security Specification

**Document type:** AFU — Cryptographic key lifecycle  
**Status:** V1 baseline / mandatory security ADR before implementation  
**Last updated:** 2026-08-09

## 1. Key hierarchy

Nymbus must maintain separation between:

- account-level cryptographic root material;
- password-derived protection material;
- per-note content keys;
- device/platform key-access capabilities;
- recovery capabilities;
- recipient-specific wrapped key records for shared notes.

The backend must store protected key envelopes, never an unprotected universal private-note key.

## 2. Account root

The account cryptographic root is generated client-side. Google authentication must not be sufficient to reconstruct it from backend data.

The root must be protected by a user-controlled key-access mechanism according to the final approved hierarchy.

## 3. Master password

The master password is entered locally and never transmitted.

A unique salt and memory-hard password KDF derive key-encryption material. KDF parameters must be versioned so the derivation can evolve without ambiguity.

## 4. Master password change

Changing the master password should re-wrap account/root key material rather than re-encrypting every note body.

The operation must be atomic from the user's perspective: either the new protection is fully established or the old protection remains valid.

## 5. Note key

Every private note receives a cryptographically random content key.

The note key protects the note's encrypted content and private objects. It is never used as a password-derived key.

## 6. Note-specific password

If the user selects a dedicated note password, the client derives a wrapping key from that password and stores only the protected note-key representation plus required derivation metadata.

The dedicated password must not be stored server-side.

## 7. First unlock

The first unlock of a private note requires the configured note protection secret/path. Once initialized, a platform-assisted local unlock can be enrolled according to the approved authenticator design.

A first-unlock requirement must never be bypassed by bulk unlock.

## 8. Platform authenticator

A platform authenticator provides a device-bound authorization mechanism. It must not expose biometric material to Nymbus.

The exact mechanism used to authorize access to encrypted key material must be defined by a dedicated passkey/device ADR.

## 9. Bulk unlock

Bulk unlock is an operation over already initialized note-key envelopes. It must not introduce a global plaintext master key replacing individual note isolation.

## 10. Multi-device provisioning

A new device starts with identity/session access but no automatic access to private-note plaintext or the account root.

Cryptographic provisioning must be explicitly authorized from an existing trusted context or through the approved recovery process.

## 11. Device revocation

Revoking a device/passkey blocks future authorization through that credential. It does not guarantee deletion of plaintext already extracted by the device.

The server must maintain enough state to reject revoked device operations.

## 12. Sharing

For a shared private note, each authorized recipient receives a separate protected key-access record. The backend never receives the plaintext note key solely because sharing is enabled.

## 13. Re-keying

When access is revoked, the system must distinguish:

- revoking server authorization immediately;
- preventing future key distribution;
- rotating keys for future content when required;
- historical versions that remain protected under previous generations.

The exact re-key strategy is a mandatory ADR.

## 14. Recovery

The temporary recovery key is high-entropy, single-use, account-bound and valid for 10 minutes. It must authorize a recovery protocol rather than represent a permanent copy of the account root.

## 15. Versioned key metadata

All cryptographic envelopes must carry enough non-secret version/algorithm/KDF identifiers for deterministic interpretation after upgrades.

## 16. Key rotation

Rotation mechanisms must exist independently of password changes. A password change should not be confused with content-key rotation.

## 17. Memory lifecycle

Raw key material should remain in application memory for the shortest practical period. The implementation should avoid unnecessary copies and invalidate references after use.

Browser memory clearing is best-effort and cannot be treated as a hard cryptographic erasure guarantee.

## 18. Backup/restore

A NAS backup must preserve encrypted content and all required protected key envelopes. A restore procedure that restores ciphertext without its corresponding key metadata is considered incomplete.

## 19. Security gates

Before coding, an ADR must define and justify:

- exact key hierarchy;
- cryptographic primitives;
- KDF and parameters;
- wrapping format;
- passkey/device binding;
- multi-device provisioning;
- recovery protocol;
- shared-note re-keying;
- historical version handling;
- key rotation/version migration.
