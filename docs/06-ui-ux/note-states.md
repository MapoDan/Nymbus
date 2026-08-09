# Nymbus — Note States UI

**Document type:** AFU — Note state and security UX  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. State model

A note can be represented by independent state dimensions:

- normal/private;
- locked/unlocked;
- local/server synchronization state;
- permission level;
- selected/favorite state.

The UI must not collapse these dimensions into one ambiguous status.

## 2. Private locked note

The user can see approved metadata such as title and tags and can find the note through metadata search.

The body is not rendered in plaintext and no body snippet is shown.

## 3. Unlock action

Opening a locked private note presents the appropriate unlock mechanism according to the note's configured protection.

## 4. First unlock

The first unlock of a private note follows the security flow already defined in the cryptographic specification. If the note was protected using the user's master password, the user can subsequently choose the permitted note-specific protection mode.

## 5. Biometric/passkey-like local unlock

Platform authentication such as Face ID, Touch ID or Windows Hello is used as a local user-verification mechanism. Nymbus must not treat biometric data itself as an application secret.

## 6. Unlock timeout

Unlocked private-note access expires after **15 minutes** according to the product decision.

The timeout should be extended according to the approved activity policy rather than indefinitely keeping plaintext access alive.

## 7. Bulk unlock

The user can request bulk unlocking of eligible private notes.

Bulk unlock applies only to notes for which the necessary protection setup has already been established. It must not bypass first-unlock password requirements.

## 8. Lock action

The user can manually lock private content before the 15-minute timeout.

When locked, plaintext views and private search indexes must become inaccessible according to the client cryptographic lifecycle.

## 9. Expiration while editing

If the unlock timeout expires during editing, Nymbus must protect the document state before allowing further plaintext interaction. Pending edits must remain recoverable without silently losing work.

## 10. Permission revocation

If access is revoked while a note is open, the UI must transition to an unauthorized state and stop further authorized synchronization.

## 11. Sync states

Use clear non-color-only states:

- **Synced:** current local state is acknowledged by server;
- **Syncing:** operations are being transferred;
- **Offline:** changes are stored locally and waiting;
- **Needs attention:** synchronization requires user action.

## 12. Read-only state

A user with read-only permission sees the content but cannot access editing controls. The reason should be understandable without exposing internal authorization rules.

## 13. Missing local key

If the server returns an authorized encrypted note but the client lacks the required local key material, the UI must clearly distinguish this from "no permission".

## 14. Security copy

Security-related messaging should be concise and factual. Avoid alarming language when an ordinary lock/timeout occurs.
