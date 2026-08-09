# Nymbus — Note Model

**Status:** V1 baseline

## 1. Note types

Nymbus distinguishes at minimum:

- normal notes;
- private notes;
- shared encrypted notes.

A private note may also be shared according to the approved E2E sharing model.

## 2. Stable identity

Each note has a stable ID independent of title, folder, tags, encryption state and version.

## 3. Metadata

A note record contains organizational metadata such as title, tags, folder, favorite state and synchronization state. For private notes these remain searchable while locked.

## 4. Content

Normal note content may be handled directly by the application content model. Private note content is represented by encrypted content records and never by server-readable plaintext.

## 5. First protection setup

On first access to a private note, the user chooses whether its key access is protected through the user's master-password path or a dedicated password for that note, according to the security model.

A dedicated password is not persisted as plaintext.

## 6. Unlock

Private notes have a 15-minute unlock lifetime. Bulk unlock is permitted only for notes that have already completed their first protection setup.

## 7. Search

Metadata search works while private notes are locked. Content search requires local decryption and therefore operates only on authorized client-side plaintext.

## 8. Editing

Editing a private note must never require uploading plaintext to the server. The client encrypts the resulting content before persistence/synchronization.

## 9. Versions

Exactly 15 versions are retained as user-visible history in V1. The implementation may maintain additional internal synchronization information when required for convergence, but such information is not presented as additional user-visible versions.

## 10. Deletion

A deleted note enters the trash lifecycle. Permanent deletion must remove/revoke associated content, attachment and cryptographic references according to the retention policy.

## 11. Sharing

Sharing adds explicit permission and cryptographic key-access records. It does not convert private plaintext into server-readable content.
