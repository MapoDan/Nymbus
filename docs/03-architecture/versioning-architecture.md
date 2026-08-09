# Nymbus — Versioning Architecture

**Document type:** AFU — Note history and version retention  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Product rule

Nymbus retains a fixed maximum of **15 user-visible versions per note** in V1.

This is a product constraint, not an instruction to retain only 15 internal synchronization operations.

## 2. Distinction between versions and operations

The architecture distinguishes:

- synchronization operations needed for convergence;
- user-visible historical versions.

A CRDT may require more internal operation history than the UI exposes.

## 3. Version creation

A new version should represent a meaningful persisted content state rather than every keystroke.

The exact versioning trigger must be defined in the editor specification and synchronization ADR.

## 4. Version metadata

A version should have:

- version identifier;
- note identifier;
- creation timestamp;
- author/device context as permitted;
- synchronization/causal reference;
- content representation or encrypted payload;
- deletion/retention state where needed.

## 5. Private versions

Historical versions of private notes must remain encrypted under the same security model as the current note.

Opening an old private version requires the same local cryptographic authorization as the current content.

## 6. Restore

Restoring a historical version must create a new current version rather than mutating history destructively.

Example:

```text
V12 current
V11
V10

Restore V10
   ↓
V13 = content of V10
```

The original V10 remains historical until normal retention removes it.

## 7. Retention algorithm

When a new user-visible version causes the count to exceed 15:

1. identify versions eligible for deletion;
2. preserve any state required for active synchronization/reconciliation;
3. remove the oldest eligible user-visible version;
4. clean associated encrypted assets only when no longer referenced.

## 8. Concurrent editing

A version must not be deleted solely because it is old if the synchronization engine still requires its underlying state for safe convergence.

## 9. Version display

The UI should show a concise history list with:

- date/time;
- author where relevant;
- version number/identifier;
- restore action where authorized.

## 10. Version comparison

V1 should support a practical comparison experience where feasible. The implementation must avoid requiring the backend to decrypt private note versions for comparison.

For private notes, comparison should occur client-side after authorized decryption.

## 11. Version deletion

Automatic retention is distinct from manual deletion. If manual version deletion is exposed, the UX must clearly communicate permanence.

## 12. Storage efficiency

Versions should avoid unnecessary full duplication when the underlying encrypted/document representation can safely support delta storage. However, complexity must not compromise recoverability or E2E correctness.

## 13. Exporting a version

Exporting a historical private version requires local authorization and follows the same export security boundary as the current note.

## 14. Offline history

The client may expose locally cached historical versions while offline. Missing server versions must not be represented as if they were locally available.
