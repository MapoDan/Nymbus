# Nymbus — CRDT

## 1. Purpose

CRDTs provide deterministic convergence for concurrent collaborative editing without requiring a continuously connected server.

## 2. Scope

CRDT usage is primarily for collaborative note/document content where concurrent character/block edits can occur.

CRDTs are not a universal solution for every Nymbus entity.

## 3. Server neutrality

The server persists and distributes CRDT state/operations but does not need to understand private plaintext. For private notes, CRDT payloads remain encrypted according to the approved content-encryption model.

## 4. Determinism

All participating clients must use the same versioned CRDT semantics. A CRDT schema/version identifier must accompany synchronizable state where needed.

## 5. Metadata

Titles, tags, folder relationships, favorites and permissions are not automatically CRDT-managed. Each receives a documented conflict policy appropriate to its semantics.

## 6. History

CRDT operations and compacted document snapshots must coexist with the product's 15-version user-visible note history requirement. Internal synchronization history may be retained differently from visible history.

## 7. Compaction

CRDT state must support compaction/snapshotting to prevent unbounded growth. Compaction must preserve the ability of lagging authorized clients to converge.

## 8. Security boundary

A CRDT does not provide encryption. It must never be treated as a security mechanism. Encryption is applied independently.

## 9. Resource constraints

The implementation must avoid retaining an unbounded operation graph in RAM. Old operations should be compacted according to documented safety conditions.
