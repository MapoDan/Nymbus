# Nymbus — Key Management Model

**Document type:** AFU — Cryptographic key lifecycle  
**Status:** V1 baseline / requires security validation  
**Last updated:** 2026-09-16

## 1. Purpose

This document defines the conceptual lifecycle of cryptographic keys. It intentionally avoids committing the implementation to a particular library.

## 2. Key hierarchy goals

The key hierarchy must provide:

- per-note isolation;
- master-password protection;
- platform-authenticator convenience;
- bulk unlock without weakening note-level isolation;
- sharing;
- revocation/re-keying;
- administrator-initiated initial user activation/recovery;
- device lifecycle management;
- 15-minute unlock expiration.

V1 does **not** provide dedicated passwords for individual notes.

## 3. Conceptual key objects

### Account root/key-encryption material

Represents the user's cryptographic root for private-note access. It must never be stored as raw plaintext on the backend.

### Master-derived key material

Derived locally from the user's master password using a memory-hard password KDF with a unique salt and documented parameters.

### Note key

A random symmetric key associated with one private note and used to protect its content/objects.

### Recipient key-access record

For a shared private note, the note key is protected separately for the owner and each authorized recipient using that user's key-encryption capability. The backend stores protected representations, not a universal plaintext note key.

### Device/platform wrapping capability

A device-bound cryptographic capability used to authorize convenient local access after first-time initialization.

### Recovery/activation capability

A controlled mechanism initiated by the administrator for a given user during initial activation, allowing the user to establish their own master password without exposing that password to the administrator or backend.

## 4. New user activation

The exact activation protocol must be finalized before implementation, but the security invariant is:

```text
Administrator initiates activation/recovery for user
                 ↓
User authenticates through approved account flow
                 ↓
User establishes master password locally
                 ↓
Root/key-encryption material is established/protected locally
```

The administrator must not receive the master password or plaintext private-note keys.

Google authentication alone must not create a decryptable private-note root on the backend.

## 5. Master password lifecycle

The master password:

- exists only in user-controlled input/memory;
- is never transmitted to Nymbus backend;
- is never logged;
- is never stored directly;
- is processed through a memory-hard KDF;
- is the sole password-based protection secret for V1 private notes;
- must have a defined change/recovery procedure.

## 6. Master password change

Changing the master password should not require re-encrypting every note body if the key hierarchy is correctly designed.

Instead, the account/root key-encryption material should be re-wrapped under the new password-derived protection.

This is an important reason to separate content keys from password-derived keys.

## 7. Private-note initialization

When a note becomes private:

1. the client must have access to the user's initialized master-password key hierarchy;
2. generate a random note key;
3. protect/wrap the note key for the owner using the owner's key-encryption capability;
4. encrypt the note content locally with the note key;
5. persist only ciphertext, protected key-access records and required non-secret metadata.

There is no dedicated note password and no temporary note password.

## 8. Private-note unlock

The first password-based unlock of a newly initialized private note uses the user's master password/key hierarchy.

After the first successful password-based initialization, a platform authenticator may be enrolled as a convenience local key-access mechanism.

Bulk unlock may use the approved local authenticator/master-key path only for notes already initialized.

## 9. Platform authenticator lifecycle

After first-time initialization, the user may enroll a supported platform authenticator.

The system must associate the platform credential with an authorized local key access mechanism without exposing raw biometric information to the application.

Removing a passkey/device must invalidate its ability to authorize future local unlock operations according to the device/key lifecycle design.

## 10. Multiple devices

Each device/browser profile must be treated as an independent cryptographic endpoint.

A new device must not automatically receive private-note plaintext or the user's root key simply because Google authentication succeeded.

The final device-enrollment flow must define how an already authorized device or approved recovery process securely provisions cryptographic access to a new device.

## 11. Device loss

The user must be able to revoke a lost device/passkey from account security settings.

Revoking the device must prevent that credential from authorizing future operations.

The architecture must distinguish:

- authentication credential revocation;
- local cached ciphertext;
- already decrypted/cached plaintext;
- server authorization.

A remote revocation cannot reliably erase plaintext that was already extracted by a compromised/lost device while unlocked.

## 12. Bulk unlock

Bulk unlock is a convenience operation over eligible local note keys.

It must operate only after all selected notes have passed their own initialization requirements.

Bulk unlock must not create a new global key that replaces per-note keys.

## 13. Unlock timeout

At 15 minutes after the defined private-note unlock inactivity/lifetime threshold, the client must transition protected key material to the locked state according to the final implementation strategy.

The policy must cover:

- active editor;
- background tab;
- browser suspension;
- device sleep;
- tab refresh;
- network loss.

## 14. Browser restart

The application must define whether protected key material survives a browser restart. The recommended V1 security posture is that raw decrypted key material does not survive a full browser restart without a new local authorization step.

Encrypted ciphertext may remain persisted for offline use.

## 15. Recovery / initial activation

The initial user activation is administrator-initiated. The activation mechanism must allow the user to establish a master password without sending it to the administrator or backend.

Any later recovery mechanism must restore/re-establish the user's master-password-protected key path and must not email the master password or a permanent universal decryption secret.

## 16. Shared private notes

The same note key is made accessible to multiple authorized users through separate protected key-access records:

```text
Private Note Key
   ├── protected for Owner master-key path
   ├── protected for User A master-key path
   └── protected for User B master-key path
```

Each recipient uses their own master password to unlock their own protected access path. The owner's master password is never shared.

## 17. Revocation and re-keying

If User B loses access:

1. server authorization for B is revoked;
2. future key distribution to B is blocked;
3. if required by the security policy, the note key is rotated;
4. the new note key is protected/distributed only to currently authorized users;
5. old ciphertext/key generations remain subject to the defined historical-version policy.

Revocation cannot erase plaintext already copied/exported by the recipient.

## 18. Version history keys

Every retained private version must remain decryptable only by an authorized user.

The final implementation must explicitly decide whether versions share a note-key generation or use generation-specific keys, with the decision justified against revocation, storage overhead, performance and historical access requirements.

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

- administrator-initiated activation;
- master password derivation;
- master password change;
- passkey binding;
- multi-device enrollment;
- device revocation;
- later recovery, if enabled;
- sharing and per-recipient key wrapping;
- re-keying;
- version history;
- browser restart;
- 15-minute timeout;
- offline mode;
- local search index protection.
