# Nymbus — Use Cases

**Document type:** AFU — Functional use-case catalogue  
**Status:** V1 baseline  
**Last updated:** 2026-09-16

This document describes the principal user/system interactions. It intentionally avoids implementation code. Technical mechanisms required to realize each use case are documented separately.

## Actors

| Actor | Description |
|---|---|
| Visitor | Unauthenticated person accessing Nymbus. |
| User | Authenticated Nymbus account holder. |
| Note owner | User who owns a note. |
| Collaborator | User with explicit or inherited access to a shared note/folder. |
| Administrator | User with user-management/administrative permissions. |
| Platform authenticator | Device/browser security mechanism such as Face ID, Touch ID or Windows Hello exposed through WebAuthn/platform APIs. |
| Google Identity Provider | External identity provider used for initial Nymbus authentication. |
| NAS operator | Person responsible for the self-hosted infrastructure and backups. |
| Nymbus backend | Server-side Nymbus services responsible for synchronization, metadata, authorization and encrypted storage. |

---

## UC-AUTH-01 — First account login and initial activation

**Primary actor:** Visitor  
**Goal:** Create/access a Nymbus account using Google and complete initial cryptographic activation when required.

### Preconditions

- Nymbus is reachable.
- The visitor has a Google account.
- If the account requires initial cryptographic activation, the administrator has initiated the activation process for that user.

### Main flow

1. Visitor opens Nymbus.
2. Visitor selects Google login.
3. Google authenticates the visitor.
4. Nymbus validates the resulting identity assertion.
5. Nymbus creates or retrieves the corresponding account.
6. Nymbus establishes an authenticated session.
7. If the user's cryptographic key hierarchy has not yet been initialized, Nymbus presents the administrator-initiated activation flow.
8. User establishes a master password locally.
9. Nymbus derives/protects the user's cryptographic root locally; the master password is never transmitted to the backend or administrator.
10. Nymbus offers passkey registration where supported.
11. Visitor completes or skips passkey registration according to the onboarding flow.

### Alternative flows

- Google authentication is cancelled → no authenticated Nymbus session is created.
- Identity validation fails → access is denied.
- Activation is required but has not been initiated/authorized → private-note cryptographic initialization remains unavailable according to the account state.
- Passkey registration is unsupported → Google authentication remains available.

### Security note

This flow authenticates the account. Establishing the master password initializes the user's private-note key hierarchy but does not unlock any particular private-note plaintext.

---

## UC-AUTH-02 — Subsequent passkey login

**Primary actor:** User  
**Goal:** Authenticate quickly using a registered passkey.

### Main flow

1. User opens Nymbus.
2. User chooses passkey authentication.
3. Browser invokes the platform authenticator.
4. User verifies using the device mechanism, e.g. Face ID, Touch ID or Windows Hello.
5. Nymbus verifies the WebAuthn assertion.
6. Authenticated session is established.

### Alternative flows

- User cancels authenticator → authentication remains incomplete.
- Credential is revoked/invalid → authentication fails.
- Browser does not support the capability → user may use Google authentication.

---

## UC-AUTH-03 — Manage passkeys

**Primary actor:** User  
**Goal:** Review and revoke registered authentication credentials.

### Main flow

1. User opens account security settings.
2. User views registered passkeys/devices.
3. User selects a credential.
4. Nymbus presents identifying metadata without exposing private credential material.
5. User revokes the credential.
6. Future authentication using that credential is rejected.

---

## UC-NOTE-01 — Create and edit a normal note

**Primary actor:** User  
**Goal:** Write a normal Markdown note.

### Main flow

1. User selects “New note”.
2. Nymbus creates a local note draft.
3. User enters a title.
4. User writes content normally.
5. User uses the formatting toolbar when desired.
6. User optionally inserts links, tables, images or attachments.
7. User optionally selects a folder.
8. User optionally adds tags.
9. Changes are persisted locally.
10. Changes are synchronized when connectivity permits.

### Expected result

The note exists as a normal note and is available according to the user's authorization context.

---

## UC-NOTE-02 — Create a private note

**Primary actor:** User  
**Goal:** Create a note whose body is E2E protected.

### Preconditions

- User has an initialized master-password key hierarchy.

### Main flow

1. User creates a note.
2. User enables private-note protection.
3. Nymbus uses the user's existing master-password key hierarchy to initialize the private note.
4. Nymbus generates the note key locally.
5. Nymbus creates the owner's protected note-key access record locally.
6. Nymbus encrypts protected content client-side.
7. Encrypted content and protected key-access metadata are synchronized/stored by the backend.
8. Permitted metadata remains available for organization/search.

### Expected result

The backend can manage the encrypted note without requiring access to plaintext.

### Security constraint

There is no dedicated password for the note and no temporary password. The note is protected exclusively through the owner's master-password key hierarchy.

---

## UC-PRIVATE-01 — First password-based unlock of a private note

**Primary actor:** User  
**Goal:** Access protected content for the first time on a local client.

### Main flow

1. User selects a locked private note.
2. Nymbus displays the protected-note unlock flow.
3. User authenticates/unlocks the local key hierarchy using the master password when the required local key material is not already available.
4. Nymbus unwraps the note key through the user's master-key path.
5. Protected content becomes available to the authorized client.
6. Nymbus marks the note as initialized for convenient local unlock where applicable.
7. The unlock timer starts according to the 15-minute policy.

### Prohibited shortcut

The first password-based initialization cannot be replaced by passkey login, biometric convenience, bulk unlock, or an already authenticated account session.

### Security constraint

V1 has no note-specific protection password. The user's master password is the sole password-based secret for private notes.

---

## UC-PRIVATE-02 — Subsequent platform-authenticator unlock

**Primary actor:** User  
**Goal:** Unlock a previously initialized private note quickly.

### Preconditions

- The note has completed the required master-password initialization on the device.
- The platform authenticator has been enrolled for the approved local key-access mechanism.

### Main flow

1. User opens a previously initialized locked note.
2. Nymbus requests the supported platform authenticator.
3. User verifies with the device mechanism.
4. Nymbus unlocks the permitted local key material through the authenticator-bound local capability.
5. Note plaintext becomes available locally.
6. The unlock timer starts/resets according to the defined 15-minute policy.

---

## UC-PRIVATE-03 — Bulk unlock

**Primary actor:** User  
**Goal:** Make multiple eligible private notes searchable at once.

### Main flow

1. User chooses “Unlock all eligible private notes”.
2. Nymbus identifies private notes that have completed master-password initialization.
3. Nymbus excludes notes that are not initialized or for which the user lacks current cryptographic access.
4. Nymbus performs the approved local unlock flow using the user's master-password key hierarchy or enrolled platform authenticator.
5. Eligible private-note content becomes locally available for the defined unlock lifetime.
6. User can perform content search across the unlocked local corpus.

### Security constraint

Bulk unlock cannot initialize a private note whose owner/recipient key-access record has never been established through the required master-password path.

---

## UC-PRIVATE-04 — Search while private notes are locked

**Primary actor:** User  
**Goal:** Find a private note without unlocking its body.

### Main flow

1. User opens global search.
2. User enters title/tag/folder or other permitted metadata.
3. Nymbus searches permitted metadata.
4. Locked private notes appear when authorized.
5. Search result identifies the note as private/locked.
6. Protected content is not shown in snippets.

---

## UC-PRIVATE-05 — Initial cryptographic activation

**Primary actor:** User / Administrator  
**Goal:** Establish the user's master-password protection path.

### Main flow

1. Administrator initiates activation for the target Nymbus user.
2. Nymbus creates an activation transaction.
3. User authenticates through the approved account flow.
4. User establishes a master password locally.
5. Nymbus derives/protects the user's cryptographic root locally.
6. Nymbus consumes the activation transaction.

### Security constraints

- Administrator must never receive the master password.
- Nymbus backend must never receive the master password.
- The activation mechanism must not create a permanent server-side decryption secret.

---

## UC-SHARE-01 — Share a note

**Primary actor:** Note owner  
**Goal:** Grant another Nymbus user access.

### Main flow

1. Owner opens note sharing.
2. Owner selects an individual Nymbus user.
3. Owner assigns read or edit permission.
4. Nymbus records the authorization.
5. For a private note, the owner/client creates a protected key-access record for the recipient using the recipient's cryptographic key-encryption capability; the plaintext note key is not exposed to the backend.
6. Recipient is notified according to notification settings.
7. Recipient can discover the resource according to permission rules.

### Private-note condition

The recipient must use their own master password/key hierarchy to access the shared note. The owner's master password is never shared.

---

## UC-SHARE-02 — Revoke shared access

**Primary actor:** Owner/authorized administrator  
**Goal:** Remove another user's access.

### Main flow

1. Authorized actor opens sharing management.
2. Actor selects the user.
3. Actor revokes access.
4. Nymbus immediately prevents future authorized API/synchronization access for the revoked identity.
5. For private content, the cryptographic revocation process is initiated.
6. If required by the security policy, a new note-key generation is created.
7. The new note key is protected/distributed only to remaining authorized users.
8. Revoked user loses legitimate access to future synchronized versions.

### Boundary

Nymbus cannot guarantee destruction of plaintext already copied outside the application or ciphertext/key material already held locally by the revoked user.

---

## UC-FOLDER-01 — Organize notes

**Primary actor:** User

1. User creates a folder.
2. User optionally creates nested folders.
3. User moves notes into folders.
4. User optionally shares a folder.
5. Effective permissions propagate according to the inheritance rules.

---

## UC-SEARCH-01 — Global search

**Primary actor:** User

1. User invokes global search.
2. Nymbus searches permitted server-side metadata.
3. If private content is locally unlocked, the client also searches the local decrypted corpus.
4. Results are merged according to the search UX specification.
5. Unauthorized resources are excluded.

---

## UC-SYNC-01 — Work offline

**Primary actor:** User

1. User loses connectivity to the NAS.
2. Nymbus changes to offline state.
3. User continues supported local note operations.
4. Changes are persisted locally.
5. Sync state indicates pending changes.
6. Connectivity returns.
7. Nymbus synchronizes local changes.
8. CRDT rules resolve concurrent changes.
9. Sync state returns to synchronized or displays an explicit error requiring attention.

---

## UC-COLLAB-01 — Collaborate in real time

**Primary actors:** Multiple collaborators

1. User A opens a shared editable note.
2. User B opens the same note.
3. Nymbus detects concurrent activity.
4. Real-time synchronization becomes active.
5. Each client applies local edits immediately.
6. Changes propagate with low latency.
7. CRDT logic guarantees deterministic convergence.
8. When collaboration ends, unnecessary persistent real-time resources are released.

---

## UC-VERSION-01 — Review and restore a version

**Primary actor:** User with suitable permission

1. User opens version history.
2. Nymbus presents the retained versions.
3. User selects a version.
4. User previews the historical state.
5. User chooses restore.
6. Nymbus creates the resulting current state according to the versioning rules.
7. The previous state remains represented according to the 15-version retention policy.

---

## UC-EXPORT-01 — Export content

**Primary actor:** Authorized user

1. User selects a note or folder.
2. User chooses Export.
3. User chooses Markdown or PDF.
4. Nymbus checks authorization.
5. If content is private, the authorized client performs the necessary local decryption.
6. Nymbus generates the selected representation.
7. User receives the export.
8. For private content, the UI warns that the exported copy is outside the E2E protection boundary.

---

## UC-NOTIF-01 — Configure notification channels

**Primary actor:** User

1. User opens account settings.
2. User opens notifications.
3. User sees available channels/threads.
4. User enables or disables configurable channels.
5. Nymbus applies the preferences to future notifications.
6. Mandatory security notifications remain governed by the security policy.

---

## UC-ADMIN-01 — Perform administrative management

**Primary actor:** Administrator

1. Administrator opens the administration area.
2. Nymbus checks granular permissions.
3. Administrator performs an allowed operational/account/device action.
4. Nymbus enforces authorization server-side.
5. Nymbus records a safe audit event where required.
6. Private-note plaintext remains inaccessible unless a future explicit security design states otherwise.

---

# Cross-cutting acceptance rules

The following apply to all use cases:

1. Authorization is evaluated server-side.
2. Private plaintext must not be exposed through logs, notifications, search snippets, telemetry, or unauthorized API responses.
3. Offline changes must not be silently discarded.
4. Error states must be explicit.
5. User-visible success must correspond to a persisted local state or accepted synchronized operation according to the relevant workflow.
6. Security-sensitive operations must be auditable without logging secrets.
7. The product must remain usable within the defined low-resource NAS constraints.
