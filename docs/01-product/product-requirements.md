# Nymbus — Product Requirements Specification

**Document type:** AFU — Functional/Product Analysis  
**Status:** V1 baseline  
**Audience:** Product owner, UX/UI designer, architect, security engineer, QA, Codex and other coding agents  
**Last updated:** 2026-08-09

## 1. Purpose

This document translates the product vision and decisions already made for Nymbus into explicit, testable product requirements. It is intentionally implementation-agnostic: it describes **what the product must do and why**, not application source code or a specific framework.

Implementation choices belong in the architecture documentation and ADRs.

## 2. Product context

Nymbus is a self-hosted, multi-user Markdown note application delivered as a PWA and intended to run on a low-resource NAS. It combines a minimal note-taking experience with client-side end-to-end encryption for private notes.

The product must support both ordinary notes and private notes without forcing users to understand cryptographic concepts during normal use.

## 3. Requirement conventions

Priority values:

- **MUST** — mandatory for V1.
- **SHOULD** — expected for V1 unless a documented technical constraint justifies deviation.
- **MAY** — optional V1 enhancement that must not compromise mandatory requirements.
- **OUT** — explicitly outside V1 scope.

Each requirement has a stable identifier. Identifiers must not be silently reused for a different requirement.

---

# 4. Identity and authentication

## FR-AUTH-001 — Google-only initial authentication

**Priority:** MUST

A user must authenticate to Nymbus initially exclusively through Google authentication.

Nymbus must not expose a conventional username/password registration or login flow for primary account authentication.

### Acceptance criteria

- A new user can start authentication through Google.
- A successful Google identity can create or associate the Nymbus account according to the account-linking rules.
- No Nymbus-local account password is requested as part of ordinary first login.
- Failed or cancelled Google authentication does not create an authenticated Nymbus session.

## FR-AUTH-002 — Passkey registration

**Priority:** MUST

After the first successful Google authentication, the user must be able to register one or more passkeys.

Passkeys are intended to make subsequent authentication fast on compatible devices and platforms.

## FR-AUTH-003 — Passkey authentication

**Priority:** MUST

A registered passkey must be usable for subsequent Nymbus authentication on compatible platforms supporting WebAuthn/passkeys.

The UX must allow platform authenticators such as Face ID, Touch ID, Windows Hello or equivalent mechanisms where the browser and operating system expose them through WebAuthn.

Nymbus must not claim that it directly controls Face ID, Touch ID or Windows Hello; those are platform authenticator mechanisms.

## FR-AUTH-004 — Multiple passkeys

**Priority:** MUST

A user may register multiple passkeys, for example on a phone and laptop.

The user must be able to identify and revoke individual registered credentials.

## FR-AUTH-005 — Device management

**Priority:** MUST

The account security area must provide a device/passkey management view showing sufficient information for the user to distinguish credentials/devices and revoke access.

## FR-AUTH-006 — Authentication and private-note unlock are separate

**Priority:** MUST

Successful Google/passkey authentication must not automatically reveal private-note plaintext.

Account authentication establishes the user's Nymbus identity and authorization context. Private-note unlock is a separate operation.

---

# 5. Notes

## FR-NOTE-001 — Create note

**Priority:** MUST

An authorized user must be able to create a note with a title and Markdown-capable content.

## FR-NOTE-002 — Edit note

**Priority:** MUST

An authorized user with edit permission must be able to modify note content and supported metadata.

## FR-NOTE-003 — Visual writing experience

**Priority:** MUST

The editor must allow normal writing without requiring the user to manually type Markdown syntax for common formatting operations.

A visible formatting toolbar must provide the supported formatting actions.

The underlying content model remains Markdown-compatible.

## FR-NOTE-004 — Markdown semantics

**Priority:** MUST

The note content must preserve Markdown semantics and must be exportable as Markdown.

The exact supported Markdown dialect and extensions must be defined by the editor specification.

## FR-NOTE-005 — Title

**Priority:** MUST

Every note must have a title field. The title is metadata and is not considered note body content.

## FR-NOTE-006 — Tags

**Priority:** MUST

Users must be able to assign free-form tags to notes.

Tags are user-defined and intended primarily for organization and search.

## FR-NOTE-007 — Tag reuse

**Priority:** SHOULD

The UI should suggest previously used tags to reduce repetitive entry while still allowing a new tag to be created freely.

## FR-NOTE-008 — Folders

**Priority:** MUST

A note may belong to a folder. Folders may be nested.

## FR-NOTE-009 — Favorites

**Priority:** MUST

Users must be able to mark notes and folders as favorites.

Favorite state is user-specific unless a later requirement explicitly defines shared favorite state.

## FR-NOTE-010 — Note metadata

**Priority:** MUST

The product must maintain, at minimum, the following note metadata:

- identifier;
- title;
- author/owner;
- folder;
- tags;
- creation timestamp;
- last modification timestamp;
- synchronization state;
- favorite state;
- private/non-private state;
- sharing state;
- deletion/trash state;
- version information.

Additional metadata may be introduced when required by a documented feature.

## FR-NOTE-011 — Attachments

**Priority:** MUST

A note may contain generic file attachments.

Attachments must respect the note's authorization and, for private notes, the private-note encryption model.

## FR-NOTE-012 — Inline images

**Priority:** MUST

Users must be able to place images inline within note content.

Image upload must not require the user to leave the editor.

## FR-NOTE-013 — Image quality choice

**Priority:** MUST

When an image is uploaded, the user must be able to choose the appropriate image quality/optimization level when the product offers multiple processing profiles.

The purpose is to balance visual detail against storage/network consumption because Nymbus targets NAS hardware with limited resources.

The chosen setting applies to the uploaded representation; original preservation, if offered, must be explicitly documented rather than assumed.

---

# 6. Private notes and protection

## FR-PRIVATE-001 — Enable private-note protection

**Priority:** MUST

An authorized user must be able to convert/create a note as a private note.

Once private protection is active, the note body must follow the E2E encryption model.

## FR-PRIVATE-002 — Private content confidentiality

**Priority:** MUST

The backend must not require access to private-note plaintext in order to store, synchronize, authorize, or manage the note.

## FR-PRIVATE-003 — Private metadata availability

**Priority:** MUST

The following metadata may remain available for server-side organization/search while a private note is locked:

- title;
- user-assigned tags;
- folder association;
- author/ownership information;
- timestamps;
- favorite state;
- sharing state;
- synchronization state;
- private/locked state;
- other metadata explicitly classified as non-secret by the security specification.

The backend must not expose protected note body content through these metadata operations.

## FR-PRIVATE-004 — First unlock

**Priority:** MUST

The first time a user accesses the protected content of a private note, the application must require the note's protection password according to the defined private-note security flow.

This first password-based operation is mandatory and cannot be bypassed by passkeys, device unlock, bulk unlock, or convenience features.

## FR-PRIVATE-005 — User protection choice

**Priority:** MUST

After the first access to a private note, the user must be able to choose whether the note remains protected using:

1. the user's own master password; or
2. a dedicated password defined specifically for that note.

This choice affects the local key-protection mechanism, not the underlying E2E nature of the note.

## FR-PRIVATE-006 — Platform authenticator unlock

**Priority:** MUST

Where the platform/browser supports an appropriate authenticator flow, the user may unlock a previously initialized private note using the device's platform authentication mechanism, such as Face ID, Touch ID or Windows Hello.

The system must not treat biometric data itself as an application secret. The platform authenticator performs the user verification.

## FR-PRIVATE-007 — Unlock timeout

**Priority:** MUST

A private-note unlocked state must automatically expire after 15 minutes of inactivity/defined unlock lifetime according to the security specification.

After expiration, access to private plaintext must require another successful unlock.

## FR-PRIVATE-008 — Bulk unlock

**Priority:** MUST

The user must be able to explicitly request unlocking of all eligible private notes simultaneously.

Bulk unlock is permitted only for private notes that have already completed the first-time password initialization.

Bulk unlock must not bypass the first unlock requirement of a newly initialized private note.

## FR-PRIVATE-009 — Private-note content search

**Priority:** MUST

When private notes are locked, content search must not require the server to decrypt them.

After the user unlocks eligible private notes, content search may be performed locally over decrypted content.

The UI must make clear that the searchable private-note corpus corresponds only to notes currently available to the local client.

## FR-PRIVATE-010 — Metadata search while locked

**Priority:** MUST

Private-note metadata search must remain available even while the note itself is locked.

A user must be able to locate a locked private note by title, tags, folder and other permitted metadata without decrypting its body.

## FR-PRIVATE-011 — Recovery

**Priority:** MUST

The defined recovery flow must allow the user to initiate recovery through the Google account email address associated with Nymbus.

A temporary recovery key is delivered through that email flow.

The temporary recovery key must expire after 10 minutes.

After successful validation, the user must complete the additional security step defined by the recovery specification.

## FR-PRIVATE-012 — Recovery does not weaken E2E

**Priority:** MUST

Recovery must restore authorized key access without introducing a backend capability to decrypt private-note content.

## FR-PRIVATE-013 — Private attachments

**Priority:** MUST

Attachments belonging to private notes must be protected consistently with the note's private-data security model.

## FR-PRIVATE-014 — Locked-note UX

**Priority:** MUST

A locked private note must be visually distinguishable from an ordinary note while still exposing permitted metadata.

The UI must not render protected plaintext through previews, search snippets, notifications, browser metadata, or other accidental channels.

---

# 7. Sharing and permissions

## FR-SHARE-001 — Individual-user sharing

**Priority:** MUST

A note or folder may be shared with individual Nymbus users.

V1 does not include user groups.

## FR-SHARE-002 — Read permission

**Priority:** MUST

A shared user may receive read-only access where the owner grants read permission.

## FR-SHARE-003 — Edit permission

**Priority:** MUST

A shared user may receive edit access where the owner grants edit permission.

## FR-SHARE-004 — Folder inheritance

**Priority:** MUST

Permissions granted at folder level may be inherited by nested folders and contained notes according to the permission model.

The effective permission of a resource must be determinable from explicit grants, inheritance, and revocations.

## FR-SHARE-005 — Private shared notes remain encrypted

**Priority:** MUST

Sharing a private note must not convert it into a server-readable note.

The cryptographic model must allow authorized users to access the note without providing the backend with the note plaintext.

## FR-SHARE-006 — Per-user protection of shared private notes

**Priority:** MUST

A user accessing a shared private note may use their own master password protection or define a password specific to that note, according to the private-note key-management specification.

The owner's password must not be transmitted to or exposed to another user.

## FR-SHARE-007 — Access revocation

**Priority:** MUST

The owner or authorized administrator must be able to revoke a user's access according to the authorization model.

Revocation must prevent future access and synchronization by the revoked identity.

## FR-SHARE-008 — Strong private-note revocation

**Priority:** MUST

For shared private notes, revocation must include the cryptographic re-keying behavior required to prevent the revoked user from obtaining future versions through legitimate synchronization.

The product must clearly distinguish this from impossible destruction of plaintext or copies that a user may already have exported or otherwise copied.

---

# 8. Search and discovery

## FR-SEARCH-001 — Global search

**Priority:** MUST

Nymbus must provide a global search entry point available from the primary application shell.

## FR-SEARCH-002 — Metadata search

**Priority:** MUST

Search must support permitted metadata across both normal and private notes.

## FR-SEARCH-003 — Private content search locality

**Priority:** MUST

Private-note plaintext search must occur on an authorized client after decryption and must not upload a plaintext search index to the server.

## FR-SEARCH-004 — Permission filtering

**Priority:** MUST

Search results must contain only resources the current user is authorized to discover.

## FR-SEARCH-005 — Locked state representation

**Priority:** MUST

Search results for locked private notes must indicate that the result is private/locked without revealing protected content.

---

# 9. Offline and synchronization

## FR-SYNC-001 — Offline operation

**Priority:** MUST

The PWA must support defined note-taking operations while offline.

## FR-SYNC-002 — Local persistence

**Priority:** MUST

Offline edits must be persisted locally before being considered safely accepted by the UI.

## FR-SYNC-003 — Reconnection synchronization

**Priority:** MUST

When connectivity returns, locally persisted changes must be synchronized with the backend.

## FR-SYNC-004 — CRDT-based convergence

**Priority:** MUST

Concurrent edits must use the selected CRDT strategy defined by the architecture specification so that replicas converge deterministically.

## FR-SYNC-005 — Synchronization status

**Priority:** MUST

The user must be able to understand whether the current state is synchronized, pending synchronization, offline, or affected by a synchronization error.

## FR-SYNC-006 — Real-time collaboration

**Priority:** MUST

When multiple users are actively editing the same note, Nymbus should provide low-latency collaborative synchronization.

The real-time mechanism must be resource-aware and must not maintain unnecessary long-lived connections when no active collaboration requires them.

## FR-SYNC-007 — No silent data loss

**Priority:** MUST

A synchronization conflict, server failure, or temporary network loss must not silently discard a user's locally accepted changes.

---

# 10. Version history

## FR-VERSION-001 — Version history

**Priority:** MUST

Nymbus must maintain note history according to the defined versioning model.

## FR-VERSION-002 — Fixed retention

**Priority:** MUST

V1 retains a maximum of **15 versions** per note.

The retention policy must be deterministic and documented. When a 16th retained version would otherwise be created, the oldest eligible version is removed according to the versioning rules.

## FR-VERSION-003 — Version restore

**Priority:** MUST

An authorized user with appropriate note permissions must be able to inspect retained versions and restore an eligible version according to the versioning specification.

## FR-VERSION-004 — Private version history

**Priority:** MUST

Versions of private notes remain within the private-note encryption model.

The server must not require private plaintext merely to retain version history.

---

# 11. Export

## FR-EXPORT-001 — Note export

**Priority:** MUST

An authorized user must be able to export a note.

## FR-EXPORT-002 — Folder export

**Priority:** MUST

An authorized user must be able to export a folder and its eligible contents according to the export specification.

## FR-EXPORT-003 — Format choice

**Priority:** MUST

The user chooses the export format at export time:

- Markdown;
- PDF.

## FR-EXPORT-004 — Private export warning

**Priority:** MUST

Exporting a private note creates a representation outside the application's E2E protection boundary. The UI must make this security implication clear before completing the export.

## FR-EXPORT-005 — Authorization

**Priority:** MUST

Export must enforce the same content authorization rules as viewing the content.

A user without access to a note must not be able to export it.

---

# 12. Notifications

## FR-NOTIF-001 — Notification center

**Priority:** MUST

Nymbus must provide an in-app notification center for actionable and informational events.

## FR-NOTIF-002 — Web Push

**Priority:** SHOULD

Where supported by the browser/platform, Nymbus should provide Web Push notifications.

## FR-NOTIF-003 — Notification channels

**Priority:** MUST

The account settings must allow the user to enable or disable notification channels/threads according to the notification specification.

## FR-NOTIF-004 — Security-sensitive notifications

**Priority:** MUST

Security-sensitive events such as device/passkey changes, recovery actions, and relevant access changes must follow mandatory notification rules defined by the security specification and must not be silently disabled when the event is classified as security-critical.

---

# 13. Administration and authorization

## FR-ADMIN-001 — Granular authorization

**Priority:** MUST

Nymbus must use granular permissions rather than relying solely on a binary “normal user vs administrator” model.

## FR-ADMIN-002 — Administrative management

**Priority:** MUST

Authorized administrative users must be able to perform the account/device/security/operational management actions explicitly granted by their permissions.

## FR-ADMIN-003 — No plaintext access by administrators

**Priority:** MUST

Administrative privileges must not automatically provide access to private-note plaintext.

## FR-ADMIN-004 — Server-side authorization

**Priority:** MUST

Every protected backend operation must independently verify authorization.

Client-side visibility controls are not an authorization mechanism.

---

# 14. UI and responsive behavior

## FR-UI-001 — Responsive PWA

**Priority:** MUST

The application must provide a coherent experience on phones, tablets, laptops, and desktop browsers.

## FR-UI-002 — Wide-screen navigation

**Priority:** MUST

On sufficiently wide screens, the application must provide a persistent lateral sidebar for primary navigation.

## FR-UI-003 — Small-screen navigation

**Priority:** MUST

On narrow screens where the sidebar is not practical, primary navigation must move to a fixed top navigation area or equivalent compact navigation pattern.

## FR-UI-004 — Favorites placement

**Priority:** MUST

Favorites must be available in the sidebar on wide layouts and pinned prominently at the top on small layouts.

## FR-UI-005 — Themes

**Priority:** MUST

The application must support:

- light theme;
- dark theme;
- system/automatic theme.

## FR-UI-006 — Visual language

**Priority:** MUST

The interface must be simple, modern, clean, linear, and creative, with the content remaining visually dominant.

Detailed visual tokens are defined separately in the UX/UI specification.

---

# 15. Deployment and resource constraints

## FR-INFRA-001 — NAS deployment

**Priority:** MUST

Nymbus must be deployable on a self-hosted NAS using a containerized deployment model.

## FR-INFRA-002 — Lightweight architecture

**Priority:** MUST

Backend services must be lightweight and fast enough for low-capacity NAS hardware.

## FR-INFRA-003 — Multi-user server model

**Priority:** MUST

The backend must support multiple independent Nymbus users and enforce isolation and authorization between them.

## FR-INFRA-004 — Resource-aware background work

**Priority:** MUST

Background processing must be bounded and must avoid unnecessary continuous CPU/RAM consumption.

## FR-INFRA-005 — No application backup system

**Priority:** MUST

Nymbus V1 must not implement an internal backup scheduler or backup repository. Backup responsibility belongs to the NAS/infrastructure layer.

---

# 16. Explicit exclusions

## FR-SCOPE-001 — No import in V1

**Priority:** OUT

Nymbus V1 must not include content import from Markdown files, directories, ZIP archives, Notion, Obsidian, OneNote, Evernote or other external note systems.

Import is a V2 candidate.

## FR-SCOPE-002 — No groups in V1

**Priority:** OUT

V1 sharing is based on individual users. Group management is deferred.

## FR-SCOPE-003 — No unrelated productivity modules

**Priority:** OUT

V1 does not include task management, kanban, calendar, CRM, database/spreadsheet modules, or other broad workspace functionality unless separately approved.

---

# 17. Requirement traceability

Every implementation feature must be traceable to one or more requirement IDs in this document or a more detailed child specification.

A requirement must not be considered complete until the associated acceptance criteria and test strategy have been defined.

The architecture documentation must reference these identifiers when explaining how a requirement is fulfilled.
