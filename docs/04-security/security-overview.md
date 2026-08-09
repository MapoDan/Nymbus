# Nymbus — Security Overview

**Document type:** AFU — Security requirements and boundaries  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Purpose

Nymbus must provide strong protection for private notes while remaining lightweight enough to run on a low-capacity NAS.

Security is based on a strict separation between account authentication, resource authorization and private-content cryptography.

## 2. Core security properties

Nymbus V1 must provide:

- authenticated multi-user access;
- authorization per resource;
- end-to-end encryption for private note content;
- no server-side plaintext private-note indexing;
- protected private attachments and versions;
- local platform-assisted unlock through WebAuthn/passkey-capable mechanisms where supported;
- controlled recovery;
- secure session lifecycle;
- auditability of security-sensitive server events without logging secrets.

## 3. Trust boundaries

The system consists of distinct trust domains:

1. user/browser device;
2. Nymbus API/backend;
3. NAS storage;
4. Google identity provider;
5. browser/platform authenticator;
6. email delivery provider used by recovery.

The backend/NAS is trusted to enforce authorization and persist ciphertext, but must not be treated as capable of decrypting private note plaintext.

## 4. Authentication versus decryption

Google login and passkey authentication establish account identity/session access.

The private-note cryptographic hierarchy is a separate security concern. Possession of a valid application session must not by itself imply possession of private-note decryption keys.

## 5. Private content boundary

For private notes, the following are protected by the E2E model:

- note body;
- inline images;
- attachments;
- encrypted document versions;
- other content explicitly classified as private.

Approved metadata such as title and tags remains server-readable and searchable.

## 6. Metadata boundary

Private-note metadata is intentionally not encrypted in V1 because it is required for ordinary server-side discovery and organization.

This is an explicit privacy trade-off: server administrators can potentially observe metadata but must not obtain private body plaintext through normal backend operation.

## 7. Unlock lifecycle

Private content can be unlocked locally after the required cryptographic authorization. The normal private-content unlock lifetime is 15 minutes.

The client must lock/protect private content when the unlock context expires.

## 8. First unlock and subsequent unlock

The first access to a protected private note requires the note protection setup defined by the key-management model.

After initialization, the user may use the approved local/platform authentication path for convenient subsequent access.

Bulk unlock is allowed only for notes whose protection has already been initialized; it must never bypass first-unlock requirements.

## 9. Recovery

Recovery uses a temporary, single-use recovery mechanism delivered to the Google-configured account email. The recovery key is valid for 10 minutes.

Recovery must not become an email-delivered permanent copy of the private decryption hierarchy.

## 10. Administrator boundary

A Nymbus administrator may manage users, permissions and system configuration but must not automatically gain plaintext access to private notes.

## 11. Security by failure

Security failures must fail closed where practical. In particular:

- invalid authentication must not create a session;
- invalid authorization must not expose resource data;
- invalid cryptographic validation must not result in plaintext fallback;
- expired unlock state must not silently remain valid;
- revoked access must stop further authorized synchronization.

## 12. Resource constraints

Security mechanisms should avoid heavyweight infrastructure. V1 should prefer well-reviewed standard cryptographic/browser primitives and relational persistence over dedicated security clusters or services.

## 13. Security gates

The following require explicit security/ADR approval before implementation:

- final key hierarchy;
- exact passkey-to-key-hierarchy integration;
- multi-device key provisioning;
- recovery key reconstruction/re-wrapping protocol;
- shared encrypted note re-keying;
- secure local key persistence.

No implementation may invent a weaker substitute merely to satisfy a functional flow.
