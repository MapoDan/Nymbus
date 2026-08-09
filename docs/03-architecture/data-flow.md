# Nymbus — Data Flow Specification

**Document type:** AFU — System data flows  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Purpose

This document describes what categories of information move between Nymbus components. It intentionally does not define concrete API syntax yet.

## 2. Normal note creation

```text
User input
   ↓
Browser editor
   ↓
Local persistence
   ↓
Synchronization queue
   ↓
Backend authorization
   ↓
Core persistence
```

The client should be able to confirm local persistence before backend synchronization completes.

## 3. Normal note synchronization

The client sends authenticated changes containing the minimum information required by the synchronization protocol.

The server validates:

- identity;
- authorization;
- resource existence;
- operation validity;
- version/causal information required by the sync model.

The server then persists/distributes the operation and returns synchronization state.

## 4. Private note creation

```text
User creates note
       ↓
Client generates/obtains note cryptographic material
       ↓
Client encrypts protected content
       ↓
Client persists local protected state
       ↓
Backend receives metadata + encrypted payload/envelope
```

The backend must not need the note password or private plaintext.

## 5. Private note reading

```text
Backend
  ↓ encrypted content + permitted metadata
Browser
  ↓ local key authorization
Platform authenticator/password
  ↓
Local key material
  ↓
Local decryption
  ↓
Editor
```

## 6. Private content search

### Locked

```text
Search query
 ↓
Backend metadata search
 ↓
Authorized metadata results
```

No private body plaintext is involved.

### Unlocked

```text
Search query
 ↓
Local metadata + local decrypted private index/content
 ↓
Combined authorized results
```

The client may combine server metadata results with local private-content matches.

## 7. Synchronization of private notes

The synchronization layer transfers encrypted content/operations or cryptographically protected representations as defined by the final encryption/CRDT architecture.

The server must not require decryption to order, authorize or persist opaque protected content where the protocol permits otherwise.

## 8. Image upload

For normal notes:

```text
Image selected
 ↓
User selects processing/quality level
 ↓
Client processing where appropriate
 ↓
Upload
 ↓
Asset storage
 ↓
Note references asset
```

For private notes, the asset must follow the private-note confidentiality boundary.

## 9. Sharing a normal note

```text
Owner
 ↓
Share request
 ↓
Core authorization
 ↓
Permission record
 ↓
Recipient can discover/access according to permission
```

## 10. Sharing a private note

Sharing must transfer or wrap cryptographic access rather than transfer server plaintext.

The exact key-wrapping/re-keying protocol is specified separately in `encryption-architecture.md` and `key-management.md`.

## 11. Revocation

Revocation changes server authorization immediately for new operations and triggers the cryptographic process required to prevent access to future private content where applicable.

Already-copied plaintext cannot be recalled. The UX and security documentation must never promise otherwise.

## 12. Recovery

The recovery flow uses the configured Google account email to deliver a short-lived recovery mechanism.

The recovery mechanism must:

- expire after 10 minutes;
- be single-use;
- be bound to the intended account/session/context;
- not contain private-note plaintext;
- not expose long-lived master secrets in email.

## 13. Export

For normal notes, the backend may generate exports when authorization permits.

For private notes, export requires access to decrypted content and therefore should occur in the trusted client environment where practical. The final technical architecture must explicitly decide whether PDF generation is client-side or server-assisted and how private plaintext is protected during that process.

## 14. Logging flow

Operational telemetry may contain:

- request identifiers;
- timing;
- status codes;
- service health;
- non-sensitive operational events.

It must not contain:

- private-note plaintext;
- note passwords;
- master passwords;
- recovery keys;
- encryption keys;
- authentication tokens.

## 15. Data minimization

Every data flow must be evaluated against the question:

> Does this component need this data to perform its responsibility?

If the answer is no, the data must not cross the boundary.
