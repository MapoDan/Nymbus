# Nymbus — Key Management Model

**Document type:** AFU — Cryptographic key lifecycle  
**Status:** V1 baseline / requires security validation  
**Last updated:** 2026-08-09

## 1. Purpose

This document defines the conceptual lifecycle of cryptographic keys. It intentionally avoids committing the implementation to a particular library.

## 2. Key hierarchy goals

The key hierarchy must provide:

- per-note isolation;
- master-password protection;
- optional dedicated note-password protection;
- platform-authenticator convenience;
- bulk unlock without weakening note-level isolation;
- sharing;
- revocation/re-keying;
- recovery;
- device lifecycle management;
- 15-minute unlock expiration.

## 3. Conceptual key objects

### Account root/key-encryption material

Represents the user's cryptographic root for private-note access. It must never be stored as raw plaintext on the backend.

### Master-derived key material

Derived locally from the user's master password using a memory-hard password KDF with a unique salt and documented parameters.

### Note key

A random symmetric key associated with one private note and used to protect its content/objects.

### Note-password-derived wrapping key

Derived locally from the dedicated note password and used to protect access to the corresponding note key.

### Device/platform wrapping capability

A device-bound cryptographic capability used to authorize convenient local access after first-time initialization.

### Recovery capability

A short-lived recovery mechanism that permits the user to reconstruct/replace an authorized key path without exposing permanent master secrets through email.

## 4. New account bootstrap

The exact account bootstrap must be finalized before implementation.

The desired security property is:

```text
Google authentication
       ↓
Account identity established
       ↓
User creates/establishes local cryptographic root
       ↓
Root protected by user-controlled secret path
```

Google authentication alone must not create a decryptable private-note root on the backend.

## 5. Master password lifecycle

The master password:

- exists only in user-controlled input/memory;
- is never transmitted to Nymbus backend;
- is never logged;
- is never stored directly;
- is processed through a memory-hard KDF;
- must have a defined change/recovery procedure.

## 6. Master password change

Changing the master password should not require re-encrypting every note body if the key hierarchy is correctly designed.

Instead, the account/root key-encryption material should be re-wrapped under the new password-derived protection.

This is an important reason to separate content keys from password-derived keys.

## 7. Dedicated note password lifecycle

When the user chooses a dedicated password for a note:

1. derive protection material locally;
2. wrap/protect the note key;
3. persist only the protected representation and required KDF metadata;
4. discard the raw password from application state when no longer required.

Changing the dedicated password should re-wrap the note key rather than decrypting/re-encrypting the entire note body solely because the password changed.

## 8. Platform authenticator lifecycle

After first-time note initialization, the user may enroll a supported platform authenticator.

The system must associate the platform credential with an authorized local key access mechanism without exposing raw biometric information to the application.

Removing a passkey/device must invalidate its ability to authorize future local unlock operations according to the device/key lifecycle design.

## 9. Multiple devices

Each device/browser profile must be treated as an independent cryptographic endpoint.

A new device must not automatically receive private-note plaintext or the user's root key simply because Google authentication succeeded.

The final device-enrollment flow must define how an already authorized user securely provisions cryptographic access to a new device.

## 10. Device loss

The user must be able to revoke a lost device/passkey from account security settings.

Revoking the device must prevent that credential from authorizing future operations.

The architecture must distinguish:

- authentication credential revocation;
- local cached ciphertext;
- already decrypted/cached plaintext;
- server authorization.

A remote revocation cannot reliably erase plaintext that was already extracted by a compromised/lost device while unlocked.

## 11. Bulk unlock

Bulk unlock is a convenience operation over eligible local note keys.

It must operate only after all selected notes have passed their own initialization requirements.

Bulk unlock must not create a new global key that replaces per-note keys.

## 12. Unlock timeout

At 15 minutes after successful private-note unlock activity, the client must transition protected key material to the locked state according to the final implementation strategy.

The policy must cover:

- active editor;
- background tab;
- browser suspension;
- device sleep;
- tab refresh;
- network loss.

## 13. Browser restart

The application must define whether protected key material survives a browser restart. The recommended V1 security posture is that raw decrypted key material does not survive a full browser restart without a new local authorization step.

Encrypted ciphertext may remain persisted for offline use.

## 14. Recovery model

The requested flow is:

```text
User requests recovery
        ↓
Recovery message sent to configured Google account email
        ↓
Temporary recovery key valid for 10 minutes
        ↓
User completes recovery step
        ↓
User establishes/recovers authorized local key protection
```

The recovery key must be:

- random;
- high entropy;
- short-lived;
- single-use;
- rate-limited;
- bound to the account and recovery transaction;
- unusable after expiration.

## 15. Critical recovery constraint

Email delivery of a temporary key must not mean that the email contains a permanent universal decryption secret.

If the recovery mechanism allowed anyone with access to the email to obtain an eternal copy of the account root, E2E protection would be materially weakened.

Therefore the recovery protocol must restore/re-establish an authorized key path rather than simply emailing the master password or master decryption key.

## 16. Shared private notes

The note key must support multiple authorized recipients through separate protected key-access records.

Conceptually:

```text
Private Note Key
   ├── protected for Owner
   ├── protected for User A
   └── protected for User B
```

The backend stores the protected representations, not an unprotected universal note key.

## 17. Revocation and re-keying

If User B loses access:

1. server authorization for B is revoked;
2. future key distribution to B is blocked;
3. if the security policy requires prevention of future access to evolving content, the note key is rotated;
4. authorized recipients receive the new key through protected key-access records;
5. old ciphertext/key generations remain subject to the defined historical-version policy.

The exact historical-version behavior must be explicit because version history creates a second cryptographic lifecycle.

## 18. Version history keys

Every retained private version must remain decryptable only by an authorized user.

The implementation may use one note key across versions or generation-specific keys, but the decision must be justified against:

- revocation;
- storage overhead;
- performance;
- forward/backward access requirements.

## 19. Recovery vs sharing

Recovery is an account-owner operation and must not automatically grant access to another user's shared notes unless the user's authorization state independently permits it.

## 20. Key material in memory

Plaintext keys should remain in memory only for the minimum practical period. The implementation should explicitly define cleanup/invalidation behavior and avoid unnecessary duplication of sensitive buffers.

Browser memory cannot be guaranteed to be perfectly erased by application code; this limitation belongs in the threat model.

## 21. Key identifiers

Ciphertexts and wrapped-key records may use non-secret key identifiers so the system can reference the correct encrypted material without revealing key material.

## 22. Key backup

Nymbus V1 must not implement an application-level backup scheduler.

However, the cryptographic design must document which encrypted key envelopes and account data are required for a successful NAS-level restore.

A filesystem/database backup that omits required cryptographic envelopes can make encrypted content permanently inaccessible.

## 23. Required security validation

Before implementation, a cryptography/security review must explicitly validate:

- bootstrap;
- master password derivation;
- note password derivation;
- passkey binding;
- multi-device enrollment;
- device revocation;
- recovery;
- sharing;
- re-keying;
- version history;
- browser restart;
- 15-minute timeout;
- offline mode;
- local search index protection.
