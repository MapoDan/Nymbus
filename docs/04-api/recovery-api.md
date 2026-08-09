# Nymbus — Recovery API Contract

**Document type:** AFU — Account/private-key recovery API  
**Status:** V1 baseline / security review required  
**Last updated:** 2026-08-09

## 1. Scope

Recovery provides a controlled mechanism for a user who cannot use the normal private-note unlock path.

## 2. Initiation

The authenticated user initiates recovery for their own account/security context.

The system sends a temporary recovery mechanism to the Google-configured account email.

## 3. Recovery key

The temporary recovery key must be:

- cryptographically random;
- high entropy;
- single-use;
- valid for exactly 10 minutes from issuance according to server time;
- rate limited;
- associated with one recovery transaction/account.

## 4. Expiration

After 10 minutes, the key must be rejected regardless of whether the user has previously viewed the email.

Used keys are invalid immediately after successful consumption.

## 5. Email content

The email must never contain:

- master password;
- permanent decryption key;
- private note plaintext;
- reusable account secret.

## 6. Recovery result

A successful recovery authorizes the user to perform the documented recovery step that re-establishes a valid local cryptographic protection path.

The exact key-recovery protocol must be finalized by the cryptographic security ADR before implementation.

## 7. Rate limiting

Recovery requests and key verification attempts must be aggressively rate limited and protected against brute force.

## 8. Enumeration protection

Recovery endpoints must not unnecessarily disclose whether an arbitrary email/account exists.

## 9. Audit

Recovery initiation, delivery and successful/failed consumption should produce security events without recording the recovery secret itself.

## 10. Session behavior

A successful recovery may require re-authentication or session renewal according to the final security model. It must not silently grant unrelated administrative privileges.

## 11. Recovery and shared notes

Recovery of the owner's key path does not automatically bypass resource authorization or grant access to notes owned by another user.

## 12. Security gate

This API must not be implemented until the final cryptographic recovery protocol has undergone explicit review. A superficially convenient implementation that emails a universal decryption secret is prohibited.
