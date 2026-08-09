# Nymbus — Versioning

**Status:** V1 baseline

## 1. User-visible history

Nymbus retains exactly **15 user-visible note versions** in V1.

The number is fixed at 15 and is not user-configurable in V1.

## 2. Version identity

Each version has a stable identifier, creation timestamp and causal relationship to the preceding state.

## 3. Internal synchronization history

Internal CRDT operations, sync operations or compacted snapshots may exist beyond the 15 visible versions when required for convergence. They are implementation state, not additional user-visible history.

## 4. Private notes

Each retained private version remains encrypted. Restoring a private version requires the same local cryptographic authorization as accessing the note.

## 5. Restoration

Restoring a historical version creates a new current state/version rather than mutating historical data in place.

The oldest visible version may therefore be evicted according to the fixed 15-version policy.

## 6. Version deletion

Eviction of a user-visible version must respect any internal dependency required for synchronization and cryptographic migration. Physical deletion must not occur merely because a UI version disappeared.

## 7. Sharing

Shared encrypted note versions must remain decryptable by currently authorized recipients according to the applicable key generation. Revocation/re-keying must not silently make an authorized historical version unreadable without an explicit security decision.

## 8. Conflict handling

Concurrent edits resolved through CRDT convergence may generate internal states that do not map one-to-one to user-visible versions. The versioning layer must preserve deterministic current state while maintaining the 15-version presentation rule.
