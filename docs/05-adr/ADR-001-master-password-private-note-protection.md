# ADR-001 — Master-password-only private-note protection

**Status:** Accepted  
**Date:** 2026-09-16

## Context

The original V1 functional model allowed a private note to use either the user's master password or a dedicated password. The first-unlock wording also introduced ambiguity about which password was required before that choice.

This creates unnecessary key-lifecycle branches and makes sharing, recovery and implementation by coding agents ambiguous.

## Decision

Nymbus V1 uses **one password-based protection secret: the user's master password**.

There is:

- no dedicated password per note;
- no temporary note password;
- no separate password created at first unlock.

The master password is established during the user's initial application activation. The activation/recovery process is initiated by the administrator for the relevant user, but the master password is chosen and entered by the user and must never be disclosed to the administrator or sent to the backend.

All private notes owned by the user use the user's master-password key hierarchy to protect their independent per-note encryption keys.

## Private note lifecycle

When a note becomes private:

1. the user must already have an initialized master-password key hierarchy;
2. the client generates a random note key;
3. the client protects the note key through the user's key-encryption hierarchy;
4. the client encrypts note content locally;
5. the backend receives only ciphertext, protected key-access records and permitted metadata.

The first password-based unlock of a newly initialized note uses the user's master-password path. Platform authentication and bulk unlock cannot replace the initial master-password initialization.

## Sharing

A shared private note keeps one logical note-key generation while each authorized user receives a separate protected key-access record.

Conceptually:

```text
Private Note Key
   ├── protected for Owner master-key path
   ├── protected for User A master-key path
   └── protected for User B master-key path
```

Each recipient uses their own master password to unlock their own protected key-access path. The owner's master password is never shared.

## Revocation

Revoking a recipient has both an authorization and, where required, a cryptographic effect:

1. server authorization for the recipient is revoked;
2. future synchronization/access requests are denied;
3. where strong revocation is required, a new note-key generation is created;
4. the new key is distributed only through protected key-access records for currently authorized users.

Previously copied plaintext and locally retained ciphertext/key material cannot be remotely destroyed.

## Consequences

### Positive

- One password model is easier to reason about and implement.
- Master-password changes can be handled at the root/key-envelope layer instead of re-encrypting every note body.
- Sharing maps cleanly to per-recipient protected note-key records.
- Passkey convenience remains a separate local key-access mechanism rather than a second content-protection secret.

### Negative / constraints

- Loss of the master password makes recovery critical.
- Multi-device enrollment must securely provision access to the user's master-key hierarchy.
- Shared-note revocation may require note-key rotation and therefore additional ciphertext/version handling.
- The administrator cannot recover private-note plaintext merely through administrative privileges.

## Superseded requirements

Any V1 specification stating that a private note can use a dedicated password, temporary note password, or a post-first-unlock choice between master and dedicated passwords is superseded by this ADR.

The functional and security documents must use this ADR as the authoritative decision.

## Security gate

The concrete password KDF, envelope construction, platform-authenticator binding, multi-device provisioning and cryptographic revocation protocol remain subject to dedicated security ADRs/review before implementation.
