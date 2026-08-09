# Nymbus — Security Requirements

**Document type:** AFU — Security requirements and verification baseline  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

This document converts the security decisions into requirements that can later be mapped to implementation tasks and security tests.

## 1. Authentication

| ID | Requirement | Priority |
|---|---|---|
| SEC-AUTH-001 | V1 account identity must use Google as the configured external identity provider. | MUST |
| SEC-AUTH-002 | Passkeys/WebAuthn must use standards-compliant platform APIs. | MUST |
| SEC-AUTH-003 | Nymbus must never collect or store a device PIN/passcode or biometric data. | MUST |
| SEC-AUTH-004 | Authentication success must not automatically unlock private-note plaintext. | MUST |

## 2. Authorization

| ID | Requirement | Priority |
|---|---|---|
| SEC-AZ-001 | Every protected resource operation must be authorization-checked server-side where applicable. | MUST |
| SEC-AZ-002 | Client-supplied resource identifiers must never be treated as authorization proof. | MUST |
| SEC-AZ-003 | Private-note access requires both authorization and valid cryptographic access. | MUST |
| SEC-AZ-004 | Revoked users/devices must be prevented from obtaining new authorized synchronization/data. | MUST |

## 3. Encryption

| ID | Requirement | Priority |
|---|---|---|
| SEC-ENC-001 | Private note bodies must remain E2E encrypted. | MUST |
| SEC-ENC-002 | Private images and attachments must be inside the protected content boundary. | MUST |
| SEC-ENC-003 | The backend must not require private plaintext to perform ordinary storage/sync. | MUST |
| SEC-ENC-004 | Private content must not be server-side indexed in plaintext. | MUST |
| SEC-ENC-005 | Each private note must have isolated cryptographic content-key material. | MUST |

## 4. Unlock

| ID | Requirement | Priority |
|---|---|---|
| SEC-UNL-001 | First access to a protected private note must complete the configured protection setup. | MUST |
| SEC-UNL-002 | A note may use the user's master-password path or a dedicated note password. | MUST |
| SEC-UNL-003 | Private-note unlock must expire after 15 minutes according to the approved policy. | MUST |
| SEC-UNL-004 | Bulk unlock must only operate on already initialized notes. | MUST |
| SEC-UNL-005 | Metadata search must continue to work while a private note is locked. | MUST |

## 5. Recovery

| ID | Requirement | Priority |
|---|---|---|
| SEC-REC-001 | Recovery must use the configured Google-account email channel. | MUST |
| SEC-REC-002 | Recovery key validity must be exactly 10 minutes from server-side issuance. | MUST |
| SEC-REC-003 | Recovery keys must be high-entropy, single-use and account/transaction-bound. | MUST |
| SEC-REC-004 | Recovery email must never contain a permanent private decryption key. | MUST |
| SEC-REC-005 | Recovery attempts must be rate-limited and security-audited without logging secrets. | MUST |

## 6. Devices and sessions

| ID | Requirement | Priority |
|---|---|---|
| SEC-DEV-001 | A newly authenticated device must not automatically receive the account cryptographic root. | MUST |
| SEC-DEV-002 | Devices must be independently revocable. | MUST |
| SEC-DEV-003 | Revocation must prevent future authorized server operations by that device/credential. | MUST |
| SEC-SES-001 | Application session state must be separate from private-note unlock state. | MUST |
| SEC-SES-002 | Logout must terminate the authenticated session and initiate private-content lock handling. | MUST |
| SEC-SES-003 | Session credentials must not appear in URLs or logs. | MUST |

## 7. Storage

| ID | Requirement | Priority |
|---|---|---|
| SEC-STO-001 | Server storage must not contain private-note plaintext during normal operation. | MUST |
| SEC-STO-002 | Browser caches must not intentionally retain private plaintext beyond necessary lifecycle. | MUST |
| SEC-STO-003 | Backups must preserve encrypted content together with required protected key metadata. | MUST |
| SEC-STO-004 | Secrets and plaintext private content must never be written to logs. | MUST |

## 8. Sharing

| ID | Requirement | Priority |
|---|---|---|
| SEC-SHR-001 | Shared private notes must remain E2E encrypted. | MUST |
| SEC-SHR-002 | Each authorized recipient must receive a protected cryptographic access path. | MUST |
| SEC-SHR-003 | Revocation must remove server authorization immediately. | MUST |
| SEC-SHR-004 | Re-keying policy must be explicitly defined before implementing revocation-sensitive key rotation. | MUST |
| SEC-SHR-005 | Nymbus must not claim that revocation erases plaintext already extracted by a recipient. | MUST |

## 9. Security testing mapping

Each MUST requirement must eventually map to one or more automated or manual verification cases in `docs/10-testing/`.

Security-sensitive requirements must have negative tests, not only happy-path tests.

## 10. Blocking rule

No production implementation is considered complete if it satisfies the functional behavior by violating a MUST security requirement.

When requirements conflict, the conflict must be resolved through an explicit ADR rather than silently weakening security.
