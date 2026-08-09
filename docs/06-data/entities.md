# Nymbus — Entities

**Status:** V1 baseline

## User

Represents the Nymbus account owner. Contains only identity/account data required by the application.

## Identity

Represents the external authentication identity. Google is the V1 identity provider.

## Device

Represents a known client device/browser installation and its security/provisioning state.

## Session

Represents an authenticated application session. Session lifetime is independent from private-note unlock lifetime.

## Note

Represents the stable logical document. Contains organizational metadata and references to content/version/key records.

## NoteVersion

Represents a user-visible or internally retained version of a note. V1 exposes exactly 15 user-visible versions.

## Folder

Represents an organizational container for notes.

## Tag

Represents a searchable classification label.

## NoteTag

Associates a tag with a note without duplicating tag information.

## Attachment

Represents an image or other file associated with a note. Private-note attachments are encrypted as part of the E2E content boundary.

## NotePermission

Represents a user's access level to a shared note.

## NoteKeyEnvelope

Represents a protected representation of note cryptographic key material for an authorized cryptographic principal. It never contains an exposed plaintext key.

## SyncOperation

Represents a deterministic mutation or synchronization operation identified by a stable operation ID.

## SyncState

Represents synchronization cursor/status required for convergence.

## Notification

Represents an actionable application notification, including items requiring user attention.

## TrashEntry

Represents a deleted resource awaiting restoration or permanent deletion according to retention policy.

## RecoveryTransaction

Represents a temporary recovery flow. It must not persist a permanent recovery secret in plaintext.

## Entity design rule

Entities are logical boundaries. Their final physical representation may use normalized tables, joined records or specialized storage where justified by performance and resource constraints.
