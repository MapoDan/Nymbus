# Nymbus — Authorization Model

**Document type:** AFU — Authorization and permissions  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Purpose

Authentication answers **who is the user**. Authorization answers **what that user may do**.

Nymbus must enforce authorization server-side for every protected server operation.

## 2. Permission principles

- Default deny for protected operations.
- Least privilege.
- Explicit ownership.
- Explicit sharing.
- Revocation must take effect for future authorized operations.
- Client-side visibility is never a security boundary.

## 3. Resource ownership

Primary resources have an owner:

- note;
- folder;
- attachment where independently addressable;
- account settings.

Ownership provides the baseline authority from which sharing permissions can be derived.

## 4. Note permissions

V1 supports at minimum:

- Owner — full control.
- Editor — can modify content according to the collaboration model.
- Reader — can view content but cannot modify it.

The exact permission matrix for secondary actions such as export, share and history restoration must be explicitly documented before implementation.

## 5. Folder permissions

Folder sharing may grant access to eligible nested resources according to the documented inheritance model.

The implementation must avoid ambiguous combinations where a folder grants access while an explicitly revoked child unexpectedly becomes accessible.

A formal effective-permission algorithm is required before implementation.

## 6. Tags

Tags are classification metadata and do not independently grant access.

## 7. Favorites

Favorite state is user-specific and does not affect authorization.

## 8. Private-note authorization

Two conditions must be satisfied:

1. server-side authorization permits the user to access the note;
2. local cryptographic unlock permits decryption of protected content.

Therefore:

```text
Authorized + Locked = metadata access, protected body unavailable
Authorized + Unlocked = protected body accessible locally
Unauthorized + Locked = no protected body
Unauthorized + Unlocked = access must still be revoked at server level
```

The second case is especially important for revocation and synchronization.

## 9. Revocation

When a permission is revoked:

- new server requests from the revoked identity must fail;
- synchronization must reject unauthorized future operations;
- realtime collaboration membership must be removed;
- private-note cryptographic re-keying must occur where required by the E2E sharing model.

## 10. Already cached data

Authorization revocation cannot guarantee deletion of data already copied outside the application's control.

The product must not claim otherwise.

## 11. Sharing workflow

Sharing requires:

1. authenticated owner/editor with share permission;
2. target identity resolution;
3. explicit permission selection;
4. server authorization check;
5. cryptographic key-access setup for private notes;
6. notification where configured;
7. auditable permission change.

## 12. Export permissions

Export is a separate capability from read access unless the final product decision explicitly combines them.

The permission matrix must define whether a Reader can export a note.

For private notes, export additionally requires local decryption.

## 13. Version restore permissions

Restoring a version changes current content and therefore requires stronger permission than merely reading history.

Recommended V1 rule:

- Owner and Editor may restore;
- Reader may inspect only.

This must be reflected consistently in UI and backend authorization.

## 14. Administration

Administrative privileges are scoped to application/system operations.

An administrator must not automatically gain private-note decryption capability.

This is a deliberate separation between:

- operational authority;
- content authority;
- cryptographic authority.

## 15. Server-side checks

Every protected operation must derive effective authorization from server-controlled state.

The server must not accept arbitrary values from the client for:

- owner;
- role;
- permission;
- account ID;
- resource ownership;
- administrative status.

## 16. Object-level authorization

Authorization must be evaluated at resource level, not only at endpoint level.

For example, having access to `/notes` does not imply access to every note returned by a guessed identifier.

## 17. Cross-user access prevention

An authenticated user attempting to access another user's resource without permission must receive a generic authorization failure that does not reveal unnecessary resource existence information.

## 18. Permission changes and synchronization

Permission changes must propagate to active synchronization sessions quickly enough to prevent continued authorized server operations after revocation.

Realtime sessions must revalidate authorization when appropriate rather than assuming a session remains valid indefinitely.

## 19. Auditability

Security-sensitive authorization changes should produce non-content audit records:

- resource;
- actor;
- action;
- target identity where applicable;
- timestamp;
- result.

Private plaintext and cryptographic secrets must not appear in audit logs.

## 20. Future groups

V1 does not expose group-based sharing. If groups are introduced later, group membership must become an additional authorization input and must not silently alter existing individual permissions without a defined migration policy.
