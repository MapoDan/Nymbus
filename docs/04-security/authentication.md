# Nymbus — Authentication Security

**Document type:** AFU — Authentication security  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Account login

Nymbus uses Google as the only external account identity provider in V1.

Nymbus must not introduce an independent username/password login system.

## 2. Google identity

The Google identity establishes which Nymbus account is being accessed. Nymbus stores the minimum identity attributes required for account operation.

## 3. Passkeys

Passkeys/WebAuthn are used as a fast authentication mechanism associated with the user's Nymbus account. They are not a replacement cryptographic copy of private note plaintext.

## 4. Authentication versus private-note unlock

A successful Google/passkey login does not automatically decrypt private notes.

The application session and private-content key hierarchy are separate security domains.

## 5. Session establishment

After successful authentication, the backend creates a normal Nymbus authenticated session. Session material must be protected against theft and fixation.

## 6. Account linking

A passkey may be registered only by an already authenticated/authorized account context and must be associated with that account.

## 7. Authentication events

Security-relevant events such as new passkey registration, passkey removal and suspicious authentication failures should be auditable without recording authenticator secrets.

## 8. Google account compromise

If the Google account is compromised, an attacker may be able to authenticate as the user. This does not by itself make the attacker capable of decrypting private notes if the private key hierarchy remains separately protected.

## 9. Logout

Logout terminates the application session. The private-note cryptographic lifecycle is governed separately by the local unlock/lock policy.

## 10. Failure handling

Authentication failures must not reveal unnecessary information about account existence or internal identity-provider state.

## 11. PWA/browser constraints

Authentication flows must work within supported browser/PWA security constraints and must not require exposing OAuth client secrets to the frontend.

## 12. Security gate

The final implementation must use standards-compliant OAuth/OIDC and WebAuthn/passkey mechanisms rather than custom authentication cryptography.
