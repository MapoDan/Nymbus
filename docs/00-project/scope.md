# Nymbus — V1 Scope

**Document status:** Baseline  
**Last updated:** 2026-08-09

## 1. Purpose

This document defines what is included in Nymbus V1 and, equally importantly, what is excluded. Scope control is mandatory because the project is intended to be developed with AI-assisted coding and deployed on low-resource NAS hardware.

## 2. In scope — V1

### 2.1 Accounts and authentication

- Multi-user accounts.
- Automatic account creation on first successful Google login.
- Google is the only account authentication provider.
- No Nymbus account password for primary account authentication.
- Passkey registration after first Google authentication.
- Multiple passkeys per user.
- Individual passkey/device revocation.
- Device list and device security management.
- Global logout/revocation capabilities as specified by security requirements.

### 2.2 Notes

- Markdown-based notes.
- Normal visual writing experience.
- Formatting toolbar.
- Headings, emphasis, lists, links, code, tables, images and other supported Markdown constructs as defined by the editor specification.
- Note title.
- Note content.
- Folder association.
- Free-form user tags.
- Author.
- Creation and modification timestamps.
- Synchronization state.
- Favorite state.
- Private/non-private state.
- Shared/private access state.
- 15 retained versions.
- Trash/deletion lifecycle.

### 2.3 Organization

- Folders.
- Nested folders.
- Folder sharing.
- Permission inheritance from folders to nested folders and contained notes according to the sharing specification.
- Favorites for notes and folders.
- Free-form tags with suggestions based on tags already used by the user.
- No group-management system in V1.

### 2.4 Search

Search must operate on permitted metadata even when private notes are locked.

Supported search dimensions include:

- title;
- tags;
- folder;
- author;
- creation/modification dates;
- private/non-private state;
- favorites;
- shared state;
- trash state;
- other explicitly defined non-secret metadata.

Normal notes may support server-side/full-text content search according to the search architecture.

Private-note plaintext may participate in full-text search only after the relevant note content has been decrypted on the authorized client.

A bulk-unlock action is supported for private notes that have already completed their first password-based initialization. Bulk unlock must never bypass the first-time password requirement.

### 2.5 Private notes and encryption

- End-to-end encrypted private-note content.
- End-to-end encrypted private-note attachments.
- Unencrypted/searchable note metadata only where explicitly allowed.
- First private-note unlock requires the note's protection password according to the security specification.
- Subsequent unlock may use the platform authenticator mechanism where supported.
- Private-note unlock state has a 15-minute timeout.
- Bulk unlock is allowed only for notes whose first-time password initialization has already occurred.
- Shared private notes remain E2E encrypted.
- Each authorized user may protect a shared private note using their own master password or a password specific to that note.
- Access revocation follows the strong revocation model defined in the security specification.
- Recovery uses the Google account email and a temporary recovery key with a 10-minute validity, followed by the defined recovery security step.

### 2.6 Sharing

- Individual-user sharing only.
- No groups in V1.
- Two basic note access levels: read and edit.
- Shared private notes remain E2E encrypted.
- Folder permissions can be inherited by nested folders and notes.
- Real-time collaboration is activated when multiple users are actively working on the same note.
- Persistent real-time connections should not be maintained unnecessarily when the note is inactive.

### 2.7 Offline and synchronization

- Offline-first PWA behavior.
- Local persistence of work required for offline operation.
- Synchronization after reconnection.
- CRDT-based concurrent-edit resolution where applicable.
- Hybrid real-time collaboration.
- Synchronization state visible to the user.
- No silent loss of accepted local changes.

### 2.8 Media and attachments

- Inline images.
- User-selected image quality/compression at upload time where applicable.
- Generic file attachments.
- No application-level file-size or per-user storage quota in V1; infrastructure/storage constraints remain external to the application.
- Private-note attachments must follow the private-note encryption model.

### 2.9 Export

- Export a single note.
- Export a folder.
- Export the account/content set where supported by the export specification.
- User chooses Markdown or PDF at export time.
- Images/assets required by Markdown exports are included according to the export format specification.
- Private content must be decrypted only on an authorized client before being exported.
- The UI must clearly warn that an export of private content creates a copy outside Nymbus's E2E protection.

### 2.10 Notifications

- In-app notification center.
- Web Push where supported.
- User-configurable notification categories/channels/threads.
- Security-critical notifications may follow mandatory delivery rules defined by the security specification.

### 2.11 Administration

- Granular permission model rather than a fixed user/admin-only authorization design.
- Administrative capabilities can include user lifecycle management, device/passkey security management, logout/revocation, and usage/storage statistics according to the authorization specification.
- Administrative access does not automatically grant access to private-note plaintext.
- No impersonation feature in V1.

### 2.12 UI

- Responsive PWA.
- Desktop/laptop/tablet sidebar navigation where screen width allows.
- Compact navigation on small screens.
- Favorites displayed in the sidebar on wide layouts and pinned at the top on small layouts.
- Light theme.
- Dark theme.
- System/automatic theme.
- No user-configurable accent color in V1.
- Visual direction combining Notion-like simplicity, Apple-like refinement, and Obsidian-like productivity orientation.

### 2.13 Deployment

- Self-hosted deployment on NAS.
- Containerized deployment.
- Lightweight services.
- Resource-aware architecture.
- Persistent application data stored in NAS-managed volumes.
- No internal application backup scheduler in V1.

## 3. Explicitly out of scope — V1

The following must not be implemented unless the scope is explicitly changed:

- Markdown import.
- Directory import.
- ZIP import.
- Notion import.
- Obsidian import.
- OneNote import.
- Evernote import.
- Application-level automated backup.
- Application-level backup retention.
- User groups.
- Comment/review permissions on notes.
- Additional productivity modules such as tasks, databases, kanban boards, calendars, CRM, project management, or wikis beyond the defined note-linking capabilities.
- Arbitrary public sharing unless a later specification introduces it.

## 4. V2 candidates

These are placeholders, not V1 requirements:

- Content import and migration tools.
- Notion/Obsidian/OneNote/Evernote import.
- Additional export formats.
- Advanced group management.
- Additional collaboration primitives.

V2 items must not influence V1 architecture unless an explicit compatibility requirement is documented.

## 5. Scope change procedure

A V1 scope change must:

1. identify the affected requirement IDs;
2. explain the reason for the change;
3. identify security, architecture, data, UX, API, and testing impacts;
4. update this document;
5. update the requirements matrix;
6. create or update an ADR when an architectural or security decision changes.

No scope change should be introduced only through code.
