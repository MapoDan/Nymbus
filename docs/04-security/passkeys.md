# Nymbus — Passkeys and Platform Authentication

**Document type:** AFU — Passkey and device-authenticator behavior  
**Status:** V1 baseline / implementation security gate  
**Last updated:** 2026-08-09

## 1. Purpose

Passkeys/WebAuthn provide a convenient, phishing-resistant authentication mechanism for a Nymbus account and may be used as part of the approved local unlock experience.

They must not be treated as a direct replacement for the private-note cryptographic model.

## 2. Device code requirement

Where the platform authenticator requires local user verification, the operating system may authenticate the user using the device's configured mechanism, including device passcode/PIN or biometric authentication.

Nymbus must never request, receive or store the device PIN/passcode itself.

The distinction is mandatory:

```text
User → device authentication UI → platform authenticator → WebAuthn result → Nymbus
```

not:

```text
User → Nymbus PIN field → Nymbus stores/checks device PIN
```

## 3. Biometric boundary

Biometric data remains inside the platform authenticator/security subsystem. Nymbus receives only the cryptographic authentication result.

## 4. Registration

A passkey can be registered only after the user has authenticated to the intended Nymbus account and completed the required account/device authorization flow.

The server stores the WebAuthn credential material required for verification, not biometric data or the device passcode.

## 5. Authentication

A passkey authentication must produce a standards-compliant WebAuthn assertion bound to the expected relying-party context and challenge.

Replay of a previous assertion must fail.

## 6. Account authentication versus note unlock

A passkey proves control of an enrolled authenticator for the account. It does not imply that the backend can decrypt private notes.

If the passkey is used to enable local private-note unlock, the exact binding between the authenticator and protected key material must follow the approved cryptographic ADR.

## 7. First unlock

A passkey must not silently bypass the first protection setup of a private note. A note that has never completed its required password/protection initialization remains subject to the first-unlock flow.

## 8. Bulk unlock

Bulk unlock may use the approved local authenticator path only for notes already initialized for convenient unlock.

## 9. Registration management

Users must be able to identify and revoke registered authenticators/devices. Revocation invalidates future use of the corresponding credential but cannot erase data already exposed on the device.

## 10. Lost authenticator

Loss of a device must be recoverable through another trusted device or the approved recovery process. A lost passkey must not create a permanent server-side decryption bypass.

## 11. Browser constraints

The implementation must use the browser/platform WebAuthn APIs and must not implement custom biometric or passcode collection.

## 12. Security gate

The exact passkey-to-key-hierarchy relationship is intentionally not defined here. It requires an explicit cryptographic/device ADR before implementation.
