# Nymbus — Shared Encrypted Notes Security

**Document type:** AFU — E2E sharing and revocation  
**Status:** V1 baseline / cryptographic ADR required  
**Last updated:** 2026-09-16

## 1. Objective

A private note may be shared with other authorized Nymbus users without converting the note into server-readable plaintext.

## 2. Separation of concerns

Sharing consists of two independent layers:

1. server-side authorization, determining which account may access the resource;
2. cryptographic key access, determining which authorized recipient can decrypt the private content.

Both must agree before content can be accessed.

## 3. Recipient model

Conceptually:

```text
                 Private Note Key
                /       |       \
             Owner   User A    User B
               |        |        |
          protected  protected protected
          envelope   envelope   envelope
             |          |          |
          Owner      User A      User B
        master path master path master path
```

The backend stores protected key-access records rather than a plaintext universal note key.

## 4. Master-password rule

V1 private notes use one protection model only: the user's master password.

There is no dedicated password for a shared note and no temporary note password.

When a note is shared, the existing note key is made accessible through a separate protected key-access record for each authorized Nymbus user. Each recipient uses their own master-password key hierarchy to unlock that record.

The owner's master password is never transmitted to or exposed to another user.

## 5. Invitation

A user with sufficient sharing permission may invite another Nymbus account.

The invitation itself must not expose private note plaintext or an unprotected note key.

## 6. Acceptance

The recipient must authenticate as the intended Nymbus account before receiving the cryptographic capability assigned to that account.

The recipient must have an initialized master-password key hierarchy before the note key can be protected for that recipient.

## 7. Reader versus editor

Authorization and collaboration permissions are separate from cryptographic access. A reader may decrypt content without receiving edit capability.

## 8. Editor

An editor may modify the note only while both authorization and cryptographic access remain valid.

## 9. Concurrent editing

Cryptographic access does not replace collaboration/synchronization controls. Changes continue to follow the selected CRDT/synchronization model.

## 10. Revocation

Revocation immediately removes server authorization for the recipient.

For strong cryptographic revocation, the note key must also be rotated when required to prevent the revoked recipient from obtaining future content/key generations. The new key is protected only for the owner and users who remain authorized.

## 11. Important revocation limitation

Revocation cannot erase plaintext already viewed, copied or exported by the recipient while authorized.

Nymbus must not promise retroactive erasure of already extracted information.

## 12. Re-keying

When required, a new note-key generation is created and distributed only to currently authorized recipients through their protected key-access records.

The exact re-key timing and ciphertext/version migration strategy remain a security ADR decision.

## 13. Historical versions

Historical versions require explicit cryptographic policy. The system must determine whether a revoked recipient can access versions created while they were authorized and must implement that policy through key generations rather than accidental database authorization behavior.

## 14. Removed recipient offline

A recipient who was revoked while offline may still possess cached ciphertext and previously authorized key material. Server-side revocation prevents future synchronization/access decisions, but cannot magically erase locally held data.

## 15. Sharing metadata

Server-readable metadata required for collaboration may remain visible according to the metadata classification. Private content remains encrypted.

## 16. Administrator boundary

An administrator managing sharing permissions must not thereby gain the ability to decrypt the note.

## 17. Transfer of ownership

Ownership transfer, if supported, must be treated as a cryptographic operation as well as an authorization operation. V1 must not silently implement ownership transfer as a database-field change.

## 18. Security gates

Before implementation, an ADR must define:

- recipient key wrapping mechanism;
- invitation/acceptance protocol;
- re-key timing;
- historical-version access after revocation;
- ownership transfer behavior;
- multi-device recipient handling;
- offline behavior during membership changes.
