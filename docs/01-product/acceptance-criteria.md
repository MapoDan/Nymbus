# Nymbus — Acceptance Criteria

**Document type:** AFU — Acceptance criteria  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Purpose

Acceptance criteria define observable conditions that must be satisfied before a requirement can be considered complete. They are intentionally implementation-independent.

The eventual test specification should derive automated and manual tests from these criteria.

## 2. Authentication

### AC-AUTH-001 — Google login

Given an unauthenticated visitor, when Google authentication succeeds, then Nymbus establishes an authenticated account session.

Given Google authentication fails or is cancelled, then no authenticated Nymbus session is established.

### AC-AUTH-002 — No local primary password

The normal account-login UX must not request a Nymbus-specific username/password credential.

### AC-AUTH-003 — Passkey registration

Given an authenticated user on a compatible platform, when the user starts passkey registration and successfully completes the platform authenticator ceremony, then a passkey is registered for that account.

### AC-AUTH-004 — Passkey login

Given a registered valid passkey, when the user completes platform verification, then Nymbus establishes an authenticated session.

### AC-AUTH-005 — Passkey revocation

Given a revoked passkey, when an authentication attempt is made with that credential, then authentication is rejected.

### AC-AUTH-006 — Authentication does not unlock private notes

Given a user with locked private notes, when the user successfully authenticates with Google or a passkey, then private-note plaintext remains locked until the applicable private-note unlock operation succeeds.

## 3. Notes and editor

### AC-NOTE-001 — Note creation

A user with create permission can create a note and provide a title and content.

### AC-NOTE-002 — Formatting toolbar

The editor exposes the defined common formatting operations through visible controls and applying a formatting action changes the note according to the documented Markdown semantics.

### AC-NOTE-003 — Markdown export fidelity

Content that uses supported Markdown constructs can be exported as Markdown without losing supported semantic structure.

### AC-NOTE-004 — Folders

A user can create nested folders and move authorized notes between permitted folders.

### AC-NOTE-005 — Tags

A user can create and assign arbitrary tags and can reuse existing tags.

### AC-NOTE-006 — Favorites

A user can favorite and unfavorite notes and folders. Favorite state is reflected in the appropriate navigation surfaces.

### AC-NOTE-007 — Inline image

A user can upload an image from the editor and place it at the selected point in the note content.

### AC-NOTE-008 — Image optimization choice

When multiple image processing levels are offered, the upload flow requires or clearly presents the user's quality choice before processing the image.

### AC-NOTE-009 — Attachments

A user can attach a supported file to an authorized note and later access the attachment according to note permissions.

## 4. Private notes

### AC-PRIVATE-001 — Private content encryption boundary

A private note's protected body is never required to be sent to the backend as plaintext for ordinary storage, synchronization, search, or authorization.

### AC-PRIVATE-002 — Metadata remains discoverable

While a private note is locked, an authorized user can search its permitted title, tags and other non-secret metadata.

### AC-PRIVATE-003 — First unlock requires password

For a private note that has not completed first-time initialization, an attempt to access its content requires the defined protection password. Account authentication alone cannot satisfy this requirement.

### AC-PRIVATE-004 — First-time protection choice

After successful first access, the user is presented with the choice between protection using their master password and a dedicated note password, and the selected option is persisted according to the security model.

### AC-PRIVATE-005 — Platform authenticator unlock

For a previously initialized private note on a compatible device, successful platform authentication can unlock the note without requiring the user to re-enter the note password on every access.

### AC-PRIVATE-006 — 15-minute timeout

After the defined 15-minute private-note unlock lifetime expires, protected plaintext is no longer considered unlocked and a successful unlock operation is required again.

### AC-PRIVATE-007 — Bulk unlock eligibility

A bulk unlock operation never initializes a private note that has not completed its first password-based setup.

### AC-PRIVATE-008 — Bulk unlock

Given multiple eligible private notes, when the user explicitly requests bulk unlock and completes the required local security operation, eligible notes become available to the local client for the defined unlock lifetime.

### AC-PRIVATE-009 — Private content search locality

When private notes are locked, their protected plaintext is not submitted to a backend full-text search operation. When notes are unlocked, content search is performed using the authorized local data.

### AC-PRIVATE-010 — Recovery key expiration

A recovery key issued through the recovery flow is rejected once its 10-minute validity window has expired.

### AC-PRIVATE-011 — Recovery key single-use

A successfully consumed recovery key cannot be reused.

### AC-PRIVATE-012 — Recovery does not expose plaintext to server

Successful recovery restores the user's authorized ability to access private-note key material without making the backend capable of decrypting private-note content.

### AC-PRIVATE-013 — Private attachment protection

An attachment associated with a private note is protected according to the same confidentiality boundary as the private note.

### AC-PRIVATE-014 — Locked preview safety

A locked private note never displays protected plaintext in note previews, search snippets, notification content, or other server-generated UI surfaces.

## 5. Sharing and permissions

### AC-SHARE-001 — Individual sharing

An authorized owner can share a note with another Nymbus user.

### AC-SHARE-002 — Read permission

A recipient with read permission can view the note but cannot modify its content.

### AC-SHARE-003 — Edit permission

A recipient with edit permission can modify the note according to the collaboration rules.

### AC-SHARE-004 — Folder inheritance

A permission granted at folder level is reflected in the effective permissions of eligible nested resources according to the inheritance model.

### AC-SHARE-005 — Private shared note remains encrypted

Sharing a private note does not require storing its plaintext on the backend.

### AC-SHARE-006 — Revocation

After a user's access is revoked, new authorized API and synchronization operations from that identity are rejected.

### AC-SHARE-007 — Private-note re-keying

For shared private notes, the documented cryptographic re-keying operation occurs when required by revocation so that a revoked user cannot legitimately obtain future key material.

### AC-SHARE-008 — Revocation limitation disclosure

Product documentation and UX do not claim that revocation can destroy plaintext that a user has already copied outside Nymbus.

## 6. Search

### AC-SEARCH-001 — Global search

A user can initiate search from the primary application shell.

### AC-SEARCH-002 — Metadata search across lock state

The same metadata search can return authorized normal notes and locked private notes without decrypting private bodies.

### AC-SEARCH-003 — Local private content search

After private notes are unlocked locally, their supported content can participate in local full-text search.

### AC-SEARCH-004 — Permission filtering

A search result never reveals a resource that the current user is not authorized to discover.

## 7. Offline and synchronization

### AC-SYNC-001 — Offline editing

When the backend is unreachable, supported local note edits can continue and are persisted locally.

### AC-SYNC-002 — Offline state visibility

The UI clearly indicates that changes are pending synchronization or that the application is offline.

### AC-SYNC-003 — Reconnection

When connectivity returns, pending changes are automatically submitted to the synchronization mechanism without requiring a full application restart.

### AC-SYNC-004 — Concurrent edits

Concurrent edits from multiple replicas converge according to the selected CRDT model.

### AC-SYNC-005 — No silent loss

A synchronization failure does not silently discard a locally accepted change.

### AC-SYNC-006 — Real-time collaboration lifecycle

A real-time collaboration channel is active while concurrent editing requires it and can be released when the collaboration session is no longer active.

## 8. Version history

### AC-VERSION-001 — Fifteen-version retention

A note never retains more than 15 versions under the V1 retention policy.

### AC-VERSION-002 — Historical inspection

An authorized user can inspect retained versions without modifying the current version.

### AC-VERSION-003 — Restore

An authorized user can restore an eligible retained version, and the resulting current state is synchronized according to normal note rules.

### AC-VERSION-004 — Private history

Historical states of private notes remain within the private encryption boundary.

## 9. Export

### AC-EXPORT-001 — Format selection

Before export, the user can choose Markdown or PDF.

### AC-EXPORT-002 — Authorization

An unauthorized user cannot export a note or folder they cannot access.

### AC-EXPORT-003 — Private export warning

When exporting private content, the user is informed that the resulting copy is outside Nymbus's E2E protection boundary.

### AC-EXPORT-004 — Folder export

An authorized user can export an eligible folder according to the documented inclusion and asset rules.

## 10. Notifications

### AC-NOTIF-001 — Notification center

The user can view relevant in-app notifications from a centralized notification area.

### AC-NOTIF-002 — Configurable channels

The user can enable or disable supported notification channels/threads from account settings.

### AC-NOTIF-003 — Security notifications

Security-critical events follow the mandatory notification policy even when optional informational channels are disabled.

## 11. Administration

### AC-ADMIN-001 — Permission enforcement

An administrative action succeeds only when the acting user has the required explicit permission.

### AC-ADMIN-002 — No client-only authorization

Changing UI state or calling an endpoint directly without the required authorization does not grant access to protected resources.

### AC-ADMIN-003 — Administrative privacy boundary

Administrative capabilities do not expose private-note plaintext.

## 12. Responsive UI

### AC-UI-001 — Wide layout

On sufficiently wide screens, the primary navigation is presented through the lateral sidebar model.

### AC-UI-002 — Narrow layout

On small screens where the sidebar is not appropriate, primary navigation is presented through the compact fixed top navigation model.

### AC-UI-003 — Theme selection

The user can select light, dark or system theme behavior.

### AC-UI-004 — Accessibility

Core workflows can be completed using keyboard navigation and accessible controls, and focus remains visible and logical.

## 13. Infrastructure and resource efficiency

### AC-INFRA-001 — Containerized deployment

The documented deployment model can run Nymbus as containers on the target NAS environment.

### AC-INFRA-002 — No unnecessary service proliferation

Every deployed service has a documented responsibility and architectural justification.

### AC-INFRA-003 — No internal backup scheduler

V1 contains no application-level backup scheduling mechanism.

### AC-INFRA-004 — Sensitive data absent from operational logs

Operational logging does not contain private-note plaintext, passwords, recovery keys, or cryptographic secrets.

## 14. Scope control

### AC-SCOPE-001 — No V1 import

No V1 user workflow permits importing external note collections. Any future import capability requires an explicit scope change.

### AC-SCOPE-002 — No V1 groups

V1 sharing does not expose group creation or group-based sharing.
