# Nymbus — Security Architecture

**Document type:** AFU — Security architecture and threat boundaries  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Security objectives

Nymbus must protect:

- account identity;
- authentication credentials and sessions;
- private-note plaintext;
- private-note cryptographic keys;
- attachments belonging to private notes;
- recovery mechanisms;
- sharing permissions;
- synchronization integrity.

## 2. Primary security properties

### Confidentiality

The backend must not be able to derive private-note plaintext from ordinary stored application data.

### Integrity

Tampering with encrypted content or cryptographic metadata must be detected rather than producing silently corrupted plaintext.

### Authentication

Only an authenticated account can access account-scoped server resources.

### Authorization

Authentication alone does not grant access to every note. Resource permissions are enforced server-side.

### Availability

Temporary backend failure must not destroy locally persisted authorized work.

## 3. Threat model

The V1 threat model assumes an attacker may:

- intercept network traffic without TLS protection being available;
- obtain unauthorized access to application endpoints;
- inspect server-side database records;
- inspect NAS filesystem contents;
- compromise a non-privileged backend service;
- attempt replay of authentication/recovery requests;
- manipulate synchronization messages;
- attempt unauthorized resource access by changing client requests;
- obtain a previously valid session token;
- obtain an exported copy of a private note;
- obtain a device where a user has previously unlocked content.

The threat model does **not** claim to protect plaintext while the user's endpoint itself is fully compromised by malware, keyloggers or an attacker controlling the browser runtime.

## 4. Trust boundaries

### Boundary A — Internet ↔ edge

TLS, HTTP security headers, rate limits and request validation apply.

### Boundary B — edge ↔ services

Internal traffic must not be assumed trustworthy solely because it is on a Docker network.

### Boundary C — backend ↔ private content

Private plaintext is outside the backend trust boundary.

### Boundary D — browser storage ↔ local unlock

Encrypted local state may exist while locked. Plaintext/key availability is governed by the unlock lifecycle.

### Boundary E — export

An exported file is outside Nymbus's E2E protection boundary and must be treated as a separate security object.

## 5. Security invariants

1. Private plaintext is never logged.
2. Passwords are never logged or stored as plaintext.
3. Recovery keys are never logged.
4. Cryptographic keys are never exposed through ordinary API responses unnecessarily.
5. Locked private notes do not expose body plaintext.
6. Server-side authorization is enforced independently of UI state.
7. Cryptographic integrity failures fail closed.
8. Session expiration does not silently unlock private content.
9. Export is explicitly treated as a confidentiality boundary crossing.
10. Administrators cannot decrypt private notes merely because they administer the application.

## 6. Authentication security

Google is the only primary account identity provider.

Passkeys are an additional authentication mechanism for the same Nymbus account, not an independent user identity.

The application must use standards-based WebAuthn/passkey flows rather than implementing a custom public-key authentication protocol.

## 7. Private-note security

Private-note protection is layered:

1. account authentication identifies the user;
2. note-level authorization determines whether the user may access the resource;
3. local cryptographic authorization determines whether protected content can be decrypted;
4. platform authentication may authorize access to locally protected key material.

These layers must remain conceptually separate.

## 8. Recovery security

The 10-minute recovery mechanism is a high-risk security operation.

It must be:

- short-lived;
- single-use;
- bound to the account;
- resistant to replay;
- protected against token leakage in URLs/logs where applicable;
- invalidated after successful use;
- rate-limited.

Email is considered a recovery channel, not a trusted location for permanent cryptographic secrets.

## 9. Session security

Sessions must:

- use secure transport;
- have defined expiration and renewal behavior;
- be invalidatable;
- have bounded scope;
- not contain private-note plaintext.

Private-note unlock lifetime is independent of the ordinary account session lifetime.

## 10. Authorization security

Every server operation that accesses a resource must validate the current user's effective permissions.

The server must not trust:

- hidden UI buttons;
- client-provided owner IDs;
- client-provided roles;
- client-provided permission claims without verification;
- local unlock flags.

## 11. Data protection

Sensitive data should be encrypted both in transit and at rest. E2E encryption provides an additional confidentiality boundary for private content.

Database-level encryption does not replace E2E encryption.

## 12. Security observability

Security-relevant events should be auditable without recording sensitive content.

Examples:

- successful/failed authentication;
- passkey added/removed;
- recovery initiated/completed/failed;
- permission granted/revoked;
- suspicious rate-limit events;
- security configuration changes.

Audit records must not contain private-note plaintext or cryptographic secrets.

## 13. Security failure behavior

If a protected operation cannot establish:

- identity;
- authorization;
- cryptographic integrity;
- required key access;

then the operation must fail closed.

The application must never fall back to plaintext merely because decryption or key retrieval failed.

## 14. Export threat boundary

Once a private note is exported as Markdown or PDF, the resulting file may no longer be protected by Nymbus E2E encryption. The UX must communicate this before or during export.

## 15. Security review requirements

Before V1 release, security review must cover:

- authentication flows;
- passkey lifecycle;
- recovery;
- session handling;
- cryptographic implementation;
- private-note sharing;
- revocation;
- offline storage;
- browser storage;
- synchronization integrity;
- export;
- logging;
- administrative APIs;
- rate limiting;
- dependency vulnerabilities.
