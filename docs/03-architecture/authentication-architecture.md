# Nymbus — Authentication Architecture

**Document type:** AFU — Authentication and passkey architecture  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Authentication model

Nymbus supports exactly one primary identity provider: Google.

Passkeys are supported as a fast authentication mechanism associated with the Nymbus account. They do not create a second identity provider.

## 2. Google authentication

The application must use a standards-based OAuth/OIDC flow appropriate for a browser PWA.

The application must validate the identity response according to the provider's documented security requirements and must not trust arbitrary client-provided identity claims.

## 3. Account creation

A successful Google identity can create or associate the corresponding Nymbus account.

Account identity data should be minimized. Nymbus does not need to replicate unnecessary Google profile information.

## 4. No local account password

Nymbus does not expose a conventional username/password login for the application account.

The master password used by the private-note security model is a cryptographic/content-protection secret and must not be confused with the account login credential.

## 5. Passkey registration

An authenticated user can register one or more passkeys from account security settings.

The user must complete the platform's user-verification ceremony where required.

Nymbus receives the WebAuthn credential information necessary for authentication but never receives biometric data.

## 6. Passkey authentication

The login sequence is conceptually:

```text
User selects Nymbus
      ↓
Browser requests passkey authentication
      ↓
Platform authenticator verifies user
      ↓
WebAuthn assertion
      ↓
Nymbus verifies assertion
      ↓
Authenticated account session
```

## 7. Passkey lifecycle

Users must be able to:

- register a passkey;
- see registered credentials in a recognizable form;
- revoke/remove a credential;
- understand when a credential was registered/last used where available;
- recover account access through Google if all passkeys are unavailable.

## 8. Passkey vs private-note unlock

Successful passkey login does **not** automatically imply access to all private notes.

The private-note unlock mechanism remains subject to the local cryptographic key hierarchy.

Where the user has previously enrolled a platform authenticator for convenient private-note unlock, the same device may be able to authorize local key access. This is a separate security operation even if the UX feels seamless.

## 9. Session lifecycle

The architecture must define:

- session duration;
- refresh behavior;
- revocation;
- idle handling;
- logout;
- multiple devices;
- token storage.

Tokens must not be stored in insecure browser mechanisms merely for implementation convenience.

The final mechanism must be selected through an architecture/security ADR.

## 10. Logout

Logout ends the application account session.

Logout must also invalidate or clear local authentication state according to the security policy.

It must not silently delete encrypted offline note data unless the user explicitly requests data removal.

## 11. Private-note lock lifecycle

Account session and private-note unlock are independent state machines.

```text
ACCOUNT
Authenticated ───────────────► Logged out

PRIVATE CONTENT
Locked ──► Unlocked ──► Timeout/Lock
```

Logging out must never leave private plaintext accessible without a valid local security operation.

## 12. Recovery

If a user cannot use their normal private-note unlock path, the account recovery flow can send a temporary recovery mechanism to the Google-configured email address.

The temporary recovery key is valid for 10 minutes and single-use.

The recovery process must not send a plaintext master password or permanent universal decryption key by email.

## 13. Account enumeration

Authentication and recovery endpoints should avoid revealing whether arbitrary email/account identifiers exist beyond what is necessary for the authenticated Google identity flow.

## 14. Rate limiting

Authentication, passkey registration, recovery requests and recovery-key verification must be rate limited.

Rate limits must be designed so that legitimate users can recover access without enabling brute-force attacks.

## 15. Authentication errors

Authentication errors should be understandable but should not reveal sensitive internal details such as database state, credential existence or cryptographic internals.

## 16. Multi-device behavior

Every successful login represents an authenticated device/session, not an automatic copy of the private cryptographic root.

The cryptographic architecture must separately define whether and how a trusted existing device can authorize a new device.

## 17. Google account change

Changing the Google identity associated with an account is a security-sensitive operation. It must require re-authentication and must not silently break or replace the private-note cryptographic root.

The final account-transfer/rebinding procedure must be explicitly documented before implementation.
