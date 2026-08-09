# Nymbus — System Context

**Document type:** AFU — System context  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Actors

### Nymbus user

Uses the PWA to create, edit, search, organize, protect and share notes.

### Administrator

Performs authorized operational/account-management activities without automatic access to private-note plaintext.

### Google

External identity provider used exclusively for primary account authentication and account recovery email delivery.

### Platform authenticator

Device/browser capability used for passkey authentication and, where supported by the cryptographic design, convenient private-note unlock authorization. Examples include Face ID, Touch ID and Windows Hello.

### NAS operator

Owns/operates the infrastructure hosting Nymbus.

### Browser/PWA runtime

Executes the client-side application, local persistence, editor, encryption/decryption and synchronization logic.

## 2. External systems

```text
                    Google Identity
                         ▲
                         │ OAuth/OIDC + recovery email
                         │
User ───────► Browser/PWA ───────► Nymbus Backend ───────► NAS Storage
   │                │                     │
   │                │                     │
   └──── Platform Authenticator           └──── Realtime/Sync
```

## 3. Trust relationships

| Component | Trust level for private plaintext |
|---|---|
| User device/client | Trusted while user has explicitly unlocked content |
| Nymbus backend | Must not receive private plaintext |
| NAS storage | Stores encrypted/private data and permitted metadata |
| Reverse proxy | Transport/edge component; must not need plaintext |
| Google | Identity provider; must not receive note content or note encryption keys |
| Administrator | Operational authority; not automatically cryptographic authority |

## 4. Google boundary

Google is used for identity, not content storage.

The application must not make Google Drive, Gmail storage or other Google content services part of the Nymbus data plane.

The recovery flow may use the user's Google-associated email address to deliver a temporary recovery mechanism, but the email must not contain private-note plaintext or long-lived cryptographic secrets.

## 5. Browser boundary

The browser holds sensitive information only when necessary for the user's authorized local session. Sensitive material must be protected according to the cryptographic architecture and must be cleared/invalidated when the unlock session expires.

## 6. Network boundary

All client-server communication must use authenticated encrypted transport. The application must not assume that LAN traffic is inherently trusted.

## 7. NAS boundary

The NAS is the primary hosting and persistence environment. Nymbus must assume that the NAS operator can inspect container files, volumes and server-side memory. Therefore E2E confidentiality cannot depend solely on filesystem permissions.

## 8. Context-level invariants

1. Google authenticates identity; it does not decrypt notes.
2. Backend authorization controls access to server resources.
3. Private-note plaintext remains client-side.
4. The client may search private plaintext only after local unlock.
5. Locked private metadata remains available where explicitly allowed.
6. Administrative access does not imply private-note decryption access.
7. Offline local work must survive temporary backend unavailability.
