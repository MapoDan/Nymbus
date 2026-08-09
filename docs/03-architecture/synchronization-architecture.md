# Nymbus — Synchronization Architecture

**Document type:** AFU — Offline-first synchronization architecture  
**Status:** V1 baseline / requires implementation ADR  
**Last updated:** 2026-08-09

## 1. Objective

Nymbus must allow users to work reliably despite intermittent connectivity while preserving data integrity and avoiding silent loss.

## 2. Client-first model

Local persistence is the first durable checkpoint for user edits.

```text
User edit
   ↓
Local durable state
   ↓
Pending synchronization operation
   ↓
Backend
   ↓
Acknowledged/synchronized state
```

A network failure after local persistence must not discard the user's edit.

## 3. Synchronization states

At minimum, a note/change can be:

- Local only;
- Pending;
- Synchronizing;
- Synchronized;
- Conflict/needs resolution;
- Failed/retry pending.

The UI must communicate meaningful state without overwhelming the user.

## 4. Change identity

Every offline-capable change requires a stable identifier independent of the server's database auto-increment identifiers.

The protocol must also track sufficient causal/version information to detect concurrent changes.

## 5. Conflict model

Nymbus must not rely on naive last-write-wins for collaborative note editing because it can silently discard user content.

The final implementation should use a CRDT or another formally defined merge model appropriate to the editor's document representation.

The exact CRDT technology requires an ADR before implementation.

## 6. Why CRDT

The product requirements include channels/threads and collaborative behavior, while the PWA must remain usable offline.

A mergeable operation model is therefore preferable to a central-locking model.

## 7. Scope of CRDT

The CRDT should apply to the smallest useful collaborative document unit. Metadata such as favorite state or folder assignment may use simpler conflict rules if independently defined.

Do not force every database field through the document CRDT.

## 8. Server responsibilities

The server must:

- authenticate the sender;
- authorize the resource;
- validate operation structure;
- persist operations/state;
- distribute changes to eligible clients;
- provide enough information for clients to reconcile offline changes.

For private notes, these operations must remain compatible with the E2E model and must not require plaintext.

## 9. Client responsibilities

The client must:

- persist edits locally;
- queue pending operations;
- retry safely;
- detect acknowledgements;
- merge remote operations;
- maintain local document state;
- handle reconnects;
- protect private content/indexes locally.

## 10. Idempotency

Retrying an operation must not create duplicate logical content or duplicate side effects.

The protocol must include operation identifiers or equivalent idempotency mechanisms.

## 11. Ordering

The system must not rely solely on wall-clock timestamps for causal ordering.

Client clocks may be incorrect, skewed or intentionally manipulated.

## 12. Reconnection

When connectivity returns:

1. client authenticates/refreshes session if necessary;
2. client sends pending operations;
3. server acknowledges accepted operations;
4. server returns missing remote changes;
5. client merges changes;
6. local state becomes synchronized.

## 13. Partial failure

If operation N fails while operations N+1 onward are already locally stored, the client must not discard subsequent edits.

The protocol must define whether dependent operations can continue or require rebase/reconciliation.

## 14. Multi-device synchronization

A user may have multiple active devices. Each device is an independent synchronization participant.

Private cryptographic state must be handled separately from ordinary document synchronization.

## 15. Permission changes

Permission changes are authoritative server events.

A client must not continue uploading protected changes after its permission has been revoked.

For private notes, synchronization must also respect cryptographic key availability.

## 16. Deleted resources

Deletion requires tombstones or an equivalent mechanism so offline clients do not accidentally resurrect deleted content.

## 17. Version history interaction

The 15-version product requirement must be reconciled with the synchronization model.

A retained version represents a user-visible historical state, not necessarily every internal CRDT operation.

The implementation must distinguish:

- operation history needed for synchronization;
- user-visible note versions.

## 18. Network efficiency

Synchronization should be delta/operation based rather than repeatedly uploading entire notes when only a small change occurred.

The design must balance network efficiency with cryptographic overhead for private content.

## 19. Background synchronization

The client should synchronize when the platform/browser allows it but must not require a permanently running high-frequency connection.

## 20. Realtime collaboration

Realtime connections are primarily for active editing/collaboration. Idle users should not require a continuous high-frequency channel.

## 21. Sync error UX

The UI must distinguish:

- no network;
- authentication expired;
- authorization revoked;
- temporary server failure;
- actual content conflict;
- cryptographic integrity failure.

These states must not be collapsed into a generic "sync failed" message.

## 22. Resource constraints

The synchronization implementation must be designed for a low-capacity NAS:

- bounded queues;
- bounded retry frequency;
- no busy polling;
- minimal persistent auxiliary services;
- cleanup of acknowledged operations where safe.

## 23. Implementation gate

Before coding, an ADR must define:

- document representation;
- CRDT choice;
- operation persistence;
- compaction;
- tombstone retention;
- realtime transport;
- encryption compatibility;
- offline storage format;
- conflict presentation strategy.
