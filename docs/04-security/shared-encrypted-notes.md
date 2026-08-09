# Nymbus — Shared Encrypted Notes Security

**Document type:** AFU — E2E sharing and revocation  
**Status:** V1 baseline / cryptographic ADR required  
**Last updated:** 2026-08-09

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
```

The backend stores protected key-access records rather than a plaintext universal note key.

## 4. Invitation

A user with sufficient sharing permission may invite another Nymbus account.

The invitation itself must not expose private note plaintext or an unprotected note key.

## 5. Acceptance

The recipient must authenticate as the intended Nymbus account before receiving the cryptographic capability assigned to that account.

## 6. Reader versus editor

Authorization and collaboration permissions are separate from cryptographic access. A reader may decrypt content without receiving edit capability.

## 7. Editor

An editor may modify the note only while both authorization and cryptographic access remain valid.

## 8. Concurrent editing

Cryptographic access does not replace collaboration/synchronization controls. Changes continue to follow the selected CRDT/synchronization model.

## 9. Revocation

Revocation immediately removes server authorization for the recipient.

The cryptographic lifecycle must additionally determine whether a re-key is required to prevent future access to newly generated ciphertext.

## 10. Important revocation limitation

Revocation cannot erase plaintext already viewed, copied or exported by the recipient while authorized.

Nymbus must not promise retroactive erasure of already extracted information.

## 11. Re-keying

When required, a new note-key generation is created and distributed only to currently authorized recipients.

The exact choice between lazy re-keying, immediate re-encryption and generation-specific keys is a security ADR.

## 12. Historical versions

Historical versions require explicit cryptographic policy. The system must determine whether a revoked recipient can access versions created while they were authorized.

The answer must be consistent with the product's version-history policy and must not be left to an accidental implementation detail.

## 13. Removed recipient offline

A recipient who was revoked while offline may still possess cached ciphertext and previously authorized key material. Server-side revocation prevents future synchronization/access decisions, but cannot magically erase locally held data.

## 14. Sharing metadata

Server-readable metadata required for collaboration may remain visible according to the metadata classification. Private content remains encrypted.

## 15. Administrator boundary

An administrator managing sharing permissions must not thereby gain the ability to decrypt the note.

## 16. Transfer of ownership

Ownership transfer, if supported, must be treated as a cryptographic operation as well as an authorization operation. V1 must not silently implement ownership transfer as a database-field change.

## 17. Security gates

Before implementation, an ADR must define:

- recipient key wrapping mechanism;
- invitation/acceptance protocol;
- re-key timing;
- historical-version access after revocation;
- ownership transfer behavior;
- multi-device recipient handling;
- offline behavior during membership changes.
