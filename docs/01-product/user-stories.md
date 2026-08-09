# Nymbus — User Stories

**Document type:** AFU — User story catalogue  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

User stories express the desired outcome from the user's perspective. They complement, but do not replace, the formal functional requirements.

## Authentication

### US-AUTH-001
As a new user, I want to sign in with Google so that I can access Nymbus without creating another application password.

**References:** FR-AUTH-001

### US-AUTH-002
As an authenticated user, I want to register a passkey so that I can sign in quickly on a trusted device.

**References:** FR-AUTH-002, FR-AUTH-003

### US-AUTH-003
As a user with multiple devices, I want multiple passkeys so that I can authenticate from each of my trusted devices.

**References:** FR-AUTH-004

### US-AUTH-004
As a security-conscious user, I want to revoke an individual passkey so that a lost or retired device no longer provides account access.

**References:** FR-AUTH-004, FR-AUTH-005

### US-AUTH-005
As a user, I want account authentication to be separate from private-note unlocking so that logging into Nymbus does not automatically expose sensitive content.

**References:** FR-AUTH-006

## Notes and editing

### US-NOTE-001
As a user, I want to create a note quickly so that I can capture information without configuring a complex workspace.

**References:** FR-NOTE-001

### US-NOTE-002
As a writer, I want to format text using visible controls so that I do not need to remember Markdown syntax for common operations.

**References:** FR-NOTE-003

### US-NOTE-003
As a Markdown user, I want my content to preserve Markdown semantics so that I can export and reuse my notes without losing structure.

**References:** FR-NOTE-004

### US-NOTE-004
As an organized user, I want folders and nested folders so that I can structure my knowledge hierarchically.

**References:** FR-NOTE-008

### US-NOTE-005
As a user, I want free-form tags so that I can classify notes across folder boundaries.

**References:** FR-NOTE-006

### US-NOTE-006
As a user, I want frequently used tags suggested so that adding metadata is fast.

**References:** FR-NOTE-007

### US-NOTE-007
As a user, I want to favorite notes and folders so that I can access important content quickly.

**References:** FR-NOTE-009

### US-NOTE-008
As a user, I want to insert images directly into the note so that visual information stays close to the text that explains it.

**References:** FR-NOTE-012

### US-NOTE-009
As a user, I want to choose an image quality level when uploading an image so that I can balance detail against storage and transfer cost.

**References:** FR-NOTE-013

## Private notes

### US-PRIVATE-001
As a user, I want to mark a note as private so that its content is protected by end-to-end encryption.

**References:** FR-PRIVATE-001, FR-PRIVATE-002

### US-PRIVATE-002
As a user, I want the title and allowed metadata of a locked private note to remain searchable so that encryption does not make organization impractical.

**References:** FR-PRIVATE-003, FR-PRIVATE-010

### US-PRIVATE-003
As a user opening a private note for the first time, I want to be required to enter the protection password so that initial protection cannot be bypassed by a convenience login mechanism.

**References:** FR-PRIVATE-004

### US-PRIVATE-004
As a user, I want to choose between my master password and a note-specific password so that I can decide how each private note should be protected.

**References:** FR-PRIVATE-005

### US-PRIVATE-005
As a trusted-device user, I want to unlock an initialized private note using Face ID, Touch ID, Windows Hello or an equivalent platform authenticator so that secure access is fast.

**References:** FR-PRIVATE-006

### US-PRIVATE-006
As a user, I want private notes to lock automatically after 15 minutes so that leaving the application open does not leave protected content indefinitely exposed.

**References:** FR-PRIVATE-007

### US-PRIVATE-007
As a user with many private notes, I want to unlock all eligible notes at once so that I can search across them without repeatedly authenticating.

**References:** FR-PRIVATE-008

### US-PRIVATE-008
As a user, I want bulk unlock to exclude notes that have never completed their first password initialization so that convenience cannot bypass the initial security step.

**References:** FR-PRIVATE-008

### US-PRIVATE-009
As a user, I want to search the content of unlocked private notes locally so that the server never needs a plaintext private-note index.

**References:** FR-PRIVATE-009

### US-PRIVATE-010
As a user, I want to recover protected access through my Google account email when necessary so that loss of my local protection capability does not permanently prevent recovery.

**References:** FR-PRIVATE-011, FR-PRIVATE-012

### US-PRIVATE-011
As a user, I want the recovery key to expire quickly so that an intercepted recovery message has a limited useful lifetime.

**References:** FR-PRIVATE-011

## Sharing

### US-SHARE-001
As a note owner, I want to share a note with an individual user so that I can collaborate without creating a group.

**References:** FR-SHARE-001

### US-SHARE-002
As a note owner, I want to choose read or edit access so that I can control what a collaborator can do.

**References:** FR-SHARE-002, FR-SHARE-003

### US-SHARE-003
As a folder owner, I want permissions to inherit into nested content so that I do not have to configure every note individually.

**References:** FR-SHARE-004

### US-SHARE-004
As a private-note owner, I want shared private content to remain E2E encrypted so that collaboration does not remove its privacy guarantees.

**References:** FR-SHARE-005

### US-SHARE-005
As an owner, I want to revoke a collaborator so that the collaborator cannot continue accessing future content.

**References:** FR-SHARE-007, FR-SHARE-008

## Search and organization

### US-SEARCH-001
As a user, I want one global search so that I can find notes without remembering which folder contains them.

**References:** FR-SEARCH-001

### US-SEARCH-002
As a user, I want metadata search to work for locked private notes so that I can locate protected information without unlocking it.

**References:** FR-SEARCH-002, FR-SEARCH-005

### US-SEARCH-003
As a user, I want search results to respect my permissions so that I never see resources I am not allowed to discover.

**References:** FR-SEARCH-004

## Offline and collaboration

### US-SYNC-001
As a user, I want to continue writing when my NAS is temporarily unreachable so that a network interruption does not stop my work.

**References:** FR-SYNC-001, FR-SYNC-002

### US-SYNC-002
As a user, I want changes to synchronize automatically when connectivity returns so that I do not have to manually manage files.

**References:** FR-SYNC-003

### US-SYNC-003
As a user, I want a clear synchronization state so that I know whether my latest work has reached the backend.

**References:** FR-SYNC-005

### US-SYNC-004
As a collaborator, I want concurrent edits to converge predictably so that another user's changes do not silently overwrite mine.

**References:** FR-SYNC-004, FR-SYNC-007

### US-SYNC-005
As a collaborator, I want low-latency updates while another person is actively editing the same note so that collaboration feels real-time.

**References:** FR-SYNC-006

## Versioning and export

### US-VERSION-001
As a user, I want to inspect previous versions so that I can understand how a note changed over time.

**References:** FR-VERSION-001, FR-VERSION-002

### US-VERSION-002
As a user with edit permission, I want to restore a previous version so that accidental changes can be reversed.

**References:** FR-VERSION-003

### US-EXPORT-001
As a user, I want to export a note as Markdown or PDF so that I can use the information outside Nymbus.

**References:** FR-EXPORT-001, FR-EXPORT-003

### US-EXPORT-002
As a user, I want to export a folder so that I can take a structured collection of notes with me.

**References:** FR-EXPORT-002

### US-EXPORT-003
As a private-note user, I want a clear warning before exporting protected content so that I understand that the exported copy is outside Nymbus's E2E boundary.

**References:** FR-EXPORT-004

## Notifications and administration

### US-NOTIF-001
As a user, I want to configure notification channels so that Nymbus does not interrupt me with events I do not consider useful.

**References:** FR-NOTIF-003

### US-NOTIF-002
As a user, I want security-critical events to remain visible even when ordinary notification channels are disabled so that important account activity is not silently hidden.

**References:** FR-NOTIF-004

### US-ADMIN-001
As an administrator, I want granular permissions so that operational responsibilities can be separated without giving every administrator unrestricted authority.

**References:** FR-ADMIN-001

### US-ADMIN-002
As an administrator, I want to manage devices and accounts without being able to decrypt private notes so that operational administration respects E2E privacy.

**References:** FR-ADMIN-002, FR-ADMIN-003

## Product quality

### US-QUALITY-001
As a NAS operator, I want Nymbus to consume minimal resources while idle so that it does not interfere with other services running on my NAS.

**References:** NFR-RES-001, NFR-RES-002

### US-QUALITY-002
As a mobile user, I want the application to adapt to a small screen without losing core functionality so that I can use Nymbus away from my desk.

**References:** FR-UI-001, FR-UI-003, NFR-UX-001

### US-QUALITY-003
As a user who prefers dark interfaces, I want light, dark and system themes so that Nymbus fits my environment.

**References:** FR-UI-005
