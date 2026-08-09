# Nymbus — Authentication API Contract

**Document type:** AFU — Authentication/session API  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Scope

The authentication API supports Google identity authentication, application sessions and WebAuthn/passkey operations.

## 2. Google identity flow

The API must support a standards-compliant Google OIDC/OAuth browser flow.

Conceptually:

```text
Browser → Google
Google → Browser / callback
Backend validates identity response
Backend establishes Nymbus session
```

The backend must validate issuer, audience, signature/claims and required temporal constraints according to the selected OIDC library/provider documentation.

## 3. Session establishment

A successful identity verification creates or resolves a Nymbus account and establishes an authenticated session.

The session must not expose Google provider tokens to application code unless a documented feature requires them.

## 4. Passkey registration

Authenticated users can register a passkey.

The API must support the WebAuthn registration ceremony and store only the credential information required for future verification.

Biometric data is never sent to the API.

## 5. Passkey authentication

The API verifies a WebAuthn authentication assertion and establishes an authenticated Nymbus session.

Replay, origin, challenge and credential validation must follow WebAuthn requirements.

## 6. Credential management

Authenticated users can list/revoke their registered passkeys. The list must expose safe identifying information only, such as user-assigned name, registration date and last-use information when available.

## 7. Logout

Logout invalidates the current application session. Other sessions remain active unless the user chooses a broader security action.

## 8. Session revocation

Security settings should provide a way to revoke active sessions/devices where supported by the final UX.

Revocation affects account sessions and must not be confused with cryptographic deletion of locally cached ciphertext.

## 9. Private-note unlock separation

No authentication endpoint accepts a master password or note password.

Those secrets belong to the client-side cryptographic subsystem and must never be transmitted to the backend.

## 10. Error handling

Authentication errors use safe generic responses. Detailed provider/cryptographic diagnostics belong only in protected server logs.

## 11. Rate limits

Login, passkey operations and suspicious repeated failures require rate limiting.

## 12. Security events

Successful/failed authentication, passkey registration/revocation and recovery actions may create security audit events without recording secret values.
