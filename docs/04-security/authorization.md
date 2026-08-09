# Nymbus — Authorization Security

**Document type:** AFU — Authorization model  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Principle

Authentication answers who the user is. Authorization answers what that user may do.

Every protected resource operation must evaluate authorization independently of the authentication mechanism.

## 2. Resource-level authorization

Authorization is evaluated at the resource level for notes, folders, attachments, versions and shared resources.

A client-provided resource ID must never be treated as proof of access.

## 3. Permission classes

The final permission matrix must distinguish at least:

- owner;
- editor;
- reader;
- administrator/system operator.

Exact actions are defined by the feature specifications and must be enforced server-side for server-readable resources.

## 4. Private-note cryptographic authorization

For private notes, server authorization and cryptographic authorization are both required.

A server-side permission row does not give the backend the ability to decrypt content.

## 5. Shared notes

A collaborator receives only the cryptographic capability necessary for the note according to the approved E2E sharing protocol.

## 6. Revocation

When a user's access is revoked, the backend must immediately reject new unauthorized operations.

Where the cryptographic model requires it, the note key must be rotated so future encrypted content is inaccessible to the revoked user.

## 7. Existing plaintext

Revocation cannot erase plaintext that a collaborator already viewed, copied or exported while authorized. The product must not imply otherwise.

## 8. Administrative access

Administrators can manage system resources according to their administrative role but do not receive private-note plaintext solely because they have administrative privileges.

## 9. Authorization changes

Permission changes must generate synchronization/security events so active clients can converge promptly.

## 10. TOCTOU protection

Sensitive operations must validate authorization at the time of execution, not only when a UI screen was opened.

## 11. Least privilege

Services must receive only the permissions needed for their function. A service that does not need private plaintext must never be granted a decryption path for convenience.

## 12. Fail closed

Ambiguous, missing or invalid authorization state must result in denial rather than permissive fallback.
