# Nymbus — Threat Model

**Document type:** AFU — Threat model  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Threat-model objective

The threat model defines what Nymbus protects, from whom, and which attacks must be considered before implementation.

## 2. Assets

Primary assets:

- private note plaintext;
- private images and attachments;
- private note encryption keys;
- account/session credentials and tokens;
- passkey/WebAuthn credentials;
- recovery transactions;
- authorization data;
- user metadata;
- synchronization state;
- version history.

## 3. Actors

### 3.1 Ordinary user

A legitimate account holder with access to their own notes.

### 3.2 Unauthorized account user

A valid Nymbus user attempting to access another user's resources.

### 3.3 Removed collaborator

A user who previously had access to a shared note but whose permission has been revoked.

### 3.4 Malicious content author

A collaborator or attacker able to submit malicious Markdown, links, images or clipboard content.

### 3.5 Compromised NAS/backend operator

An actor with database/storage/backend access but without the user's private cryptographic material.

### 3.6 Network attacker

An attacker able to observe or manipulate network traffic outside correctly configured TLS boundaries.

### 3.7 Compromised client

Malware or a compromised browser/device capable of observing plaintext after the user unlocks it. Nymbus cannot guarantee confidentiality against a fully compromised endpoint.

## 4. Key security assumption

E2E encryption protects private content from the backend/NAS, but it does not protect plaintext that is already exposed to a compromised authorized client.

This limitation must be documented rather than implying absolute protection.

## 5. Threats and required mitigations

| Threat | Required mitigation |
|---|---|
| Backend reads private note | E2E encryption; no server decryption path |
| Database theft | Ciphertext for private content; protected key material |
| Unauthorized note access | Per-resource authorization |
| Revoked collaborator continues sync | Permission revocation + sync enforcement + re-key where required |
| Session theft | Secure cookies/tokens, expiry, revocation and CSRF protections where applicable |
| XSS through Markdown | Sanitization, safe renderer, CSP |
| Malicious image | MIME/size validation, safe decoding, controlled serving |
| Brute-force recovery | High-entropy keys, rate limits, 10-minute expiry, single use |
| Metadata leakage | Explicitly documented metadata boundary; minimize unnecessary metadata |
| Offline bypass | Local key availability and timeout enforcement |
| Replay of sync operation | Operation IDs, causal/version checks and idempotency |
| Resource exhaustion | Payload limits, image limits, bounded sync batches and rate limits |
| Secret leakage in logs | Structured logging with secret redaction |
| Malicious administrator | Backend cannot decrypt private content by normal administrative access |

## 6. Private-note search threat

Server-side search must never index private plaintext. Client-side private search is performed only after local decryption and authorization.

## 7. Recovery threat

Email recovery is an additional trust dependency. A recovery mechanism must not expose a permanent universal decryption secret through email.

## 8. Sharing threat

Sharing must grant access to cryptographic material only through an authorized recipient mechanism. A database row granting permission is not equivalent to possession of the private note key.

## 9. Export threat

Export can intentionally move plaintext outside the protected application boundary. The UI must make this boundary understandable.

## 10. Residual risks

The following remain inherent or deployment-dependent:

- compromised endpoint after unlock;
- screenshots or manual copying of plaintext;
- compromised Google account;
- compromised recovery email account;
- malicious NAS administrator combined with endpoint compromise;
- metadata visibility by backend/storage administrators.

## 11. Security review rule

Any feature that creates a new path from backend data to private plaintext must trigger a threat-model review before implementation.
