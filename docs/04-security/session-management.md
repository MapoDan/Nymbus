# Nymbus — Session Management Security

**Document type:** AFU — Application session lifecycle  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Separation of sessions

Nymbus must distinguish the authenticated application session from the private-note unlock state.

A valid session does not imply unlocked private content.

## 2. Session establishment

A session is established only after successful authentication through the approved Google/OIDC or WebAuthn flow.

Session credentials must be protected using the browser/platform security mechanisms appropriate to the chosen architecture.

## 3. Session lifetime

The exact application-session lifetime is an implementation/security configuration and must not be confused with the 15-minute private-note unlock timeout.

The private-note unlock timeout is fixed at 15 minutes according to the product security decision.

## 4. Private unlock timeout

When the 15-minute private unlock period expires, private plaintext access must be locked even if the application session remains valid.

The application may continue to display public content and approved metadata.

## 5. Activity and timeout

The final UX/security policy must define whether the 15-minute period is absolute from successful unlock or extended by activity. The implementation must not invent a sliding policy without an explicit decision.

## 6. Logout

Logout terminates the authenticated application session and must trigger the client-side private-content lock lifecycle.

Local caches and transient plaintext must be cleaned according to the secure-storage policy.

## 7. Session revocation

Server-side session revocation must prevent future authenticated requests from the revoked session.

Clients must handle revoked-session responses by returning to the appropriate authentication state without exposing private plaintext through an error path.

## 8. Concurrent sessions

Multiple sessions/devices are supported by the product model. Each session/device must have independently revocable state.

## 9. Idle/browser lifecycle

The application must account for browser refresh, tab close, backgrounding, device sleep and network transitions. None of these events may silently extend private unlock beyond the approved policy.

## 10. Token handling

Authentication/session tokens must not be placed in URLs, logs or application content. The implementation must follow the selected secure cookie/token architecture.

## 11. CSRF/XSS considerations

Where cookie-based authentication is used, CSRF protections must be applied. All session-bearing requests must be protected against common web authentication attacks.

## 12. Error handling

Authentication/session errors must not cause the UI to render cached private plaintext as if it were still authorized.

## 13. Security gate

The final session transport, cookie attributes, token lifetime, refresh mechanism and revocation implementation must be specified consistently with the frontend and API architecture before implementation.
