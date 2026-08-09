# Nymbus — Offline Architecture

## 1. Goal

Users must be able to read, create and edit content already available on the device without a network connection.

## 2. Local persistence

Offline state is stored in browser-local persistence. The server remains the durable shared source of truth after synchronization.

## 3. Offline operations

Each local mutation receives a stable operation identity and enough causal/version information to be synchronized later.

## 4. Sync queue

Pending operations survive temporary connectivity loss and are retried safely. The queue must be bounded and recoverable after browser restart.

## 5. Private notes

Private ciphertext and required protected key envelopes may be stored locally. Plaintext should not be persisted unnecessarily.

A locked private note remains searchable by permitted metadata but not by body content.

## 6. Unlock timeout

Private-note unlock state expires after 15 minutes according to the security model. Offline mode does not extend this timeout.

## 7. Offline authorization

A previously authorized local copy cannot be used to invent new server permissions. Sensitive permission changes require server validation when connectivity exists.

## 8. Reconnection

Upon reconnection, queued operations are uploaded, remote changes are received, conflicts are resolved according to the synchronization/CRDT rules and the local state converges.

## 9. Failure behavior

A failed synchronization must preserve the user's local work. The UI must distinguish local persistence from server synchronization status.
