# Nymbus — Offline Architecture

**Document type:** AFU — PWA offline behavior  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Objective

Nymbus must remain useful when the NAS or network is temporarily unavailable, without weakening the security model.

## 2. Offline capabilities

The client should support, according to local authorization state:

- viewing locally available notes;
- editing locally available notes;
- creating notes;
- organizing locally cached metadata;
- queueing synchronization changes;
- searching locally available content;
- viewing synchronization status.

## 3. Private notes offline

Private notes may be accessed offline only if the necessary encrypted data and local authorization/key material are already available.

A user who has never initialized/unlocked a private note must not be able to bypass that requirement merely by going offline.

## 4. Locked offline state

While locked:

- encrypted private content may remain stored locally;
- private plaintext must not be displayed;
- private search indexes must remain inaccessible;
- approved metadata search can continue.

## 5. Unlocked offline state

After successful local unlock, the client may decrypt locally available private notes without contacting the server, subject to the 15-minute unlock lifetime and local key lifecycle.

## 6. Offline creation

New notes created offline receive client-generated stable identifiers.

When connectivity returns, they are synchronized without requiring the server to assign the identity after the fact.

## 7. Offline edits

Edits are persisted locally before being queued for synchronization.

The user must never lose an edit solely because the network request failed.

## 8. Pending queue

The pending queue must be:

- persistent across page reloads;
- bounded where practical;
- idempotent;
- recoverable after browser restart;
- associated with the relevant authenticated account/device context.

Sensitive payloads in the queue must follow the same protection rules as the underlying note.

## 9. Browser storage

Persistent browser storage must be treated as potentially inspectable by the local device user and therefore must not be considered a secure plaintext vault.

Private content stored offline should remain encrypted when not actively being used.

## 10. Storage quota

The application must monitor browser storage quota where APIs allow it and provide understandable behavior when local capacity becomes constrained.

The system must not silently delete user-created offline changes to recover space.

## 11. Reconnect

On reconnect, synchronization follows the defined sync protocol rather than replacing local data wholesale with server data.

## 12. Offline authentication

The exact offline authentication behavior must be consistent with the session and passkey architecture.

Offline access must never be used as a reason to weaken account authorization or private-note key protection.

## 13. 15-minute timeout offline

The 15-minute private unlock timeout applies even without network connectivity.

The client must not extend the timeout merely because it cannot contact the backend.

## 14. Browser suspension and device sleep

The implementation must account for background tab suspension and device sleep. Sensitive local state should be treated conservatively after prolonged suspension.

## 15. Conflict UX

When a local change cannot be automatically merged, the user must be informed without silently overwriting either version.

The conflict UI should provide enough context to choose/reconcile content without requiring technical knowledge of CRDT internals.

## 16. Offline security events

Security-sensitive local actions performed offline must be recorded locally if they require auditability, then synchronized when connectivity returns where appropriate.

## 17. Service worker

The service worker should cache only application resources and explicitly approved non-sensitive assets. It must not become an uncontrolled cache of private plaintext.

## 18. Offline export

Private-note export while offline is permitted only if all required local content and processing capabilities are available. The export security warning remains applicable.

## 19. Data clearing

Clearing site data/browser storage may permanently remove local unsynchronized work. The UX should communicate this limitation where the browser permits the application to detect relevant actions.

The server copy remains authoritative only for changes already synchronized.
