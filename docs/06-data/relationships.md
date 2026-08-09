# Nymbus — Relationships

**Status:** V1 baseline

## 1. Core ownership

- User 1:N Note
- User 1:N Folder
- User 1:N Tag
- User 1:N Device
- User 1:N Session
- User 1:N Notification

## 2. Notes

- Folder 1:N Note
- Note 1:N NoteVersion
- Note N:M Tag through NoteTag
- Note 1:N Attachment
- Note 1:N NotePermission
- Note 1:N NoteKeyEnvelope
- Note 1:N SyncOperation
- Note 1:N TrashEntry lifecycle records as applicable

## 3. Sharing

A NotePermission identifies the recipient and role. A corresponding NoteKeyEnvelope represents the cryptographic access capability. Neither record alone is sufficient for private-note plaintext access.

## 4. Devices

A User may have multiple devices. A device may have multiple sessions over time. Device revocation must invalidate future authorized operations associated with that device/credential.

## 5. Versions

A NoteVersion belongs to exactly one Note and carries a stable version identifier. Internal synchronization state must not be confused with the 15 user-visible versions.

## 6. Attachments

Attachments belong to a note and may be referenced by a specific content/version representation. Deleting a note must not orphan attachment records.

## 7. Synchronization

SyncOperation belongs to the logical mutation stream of a resource and must contain enough causal/version information for deterministic processing without embedding private plaintext server-side.

## 8. Referential integrity

All server-readable foreign-key relationships must enforce referential integrity. Encrypted content references must also be validated before becoming reachable by authorized clients.

## 9. Sharing and deletion

Deleting a shared note must revoke active authorization and propagate deletion state through synchronization. Restoration must re-evaluate permissions rather than blindly restoring historical access.
