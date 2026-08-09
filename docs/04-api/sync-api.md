# Nymbus — Synchronization API Contract

**Document type:** AFU — Synchronization API  
**Status:** V1 baseline / requires sync ADR  
**Last updated:** 2026-08-09

## 1. Purpose

The synchronization API transports client changes and server changes without requiring the server to understand private plaintext.

## 2. Sync request

A sync request conceptually contains:

- authenticated client/session context;
- last known synchronization checkpoint;
- pending client operations;
- requested resource scope;
- protocol/version information.

## 3. Sync response

The response conceptually contains:

- accepted operation IDs;
- rejected operations with safe reason codes;
- remote operations/changes the client is missing;
- new synchronization checkpoint;
- permission/security events relevant to the client.

## 4. Idempotency

Every client operation must have a stable operation ID so retries do not duplicate effects.

## 5. Batch size

Sync batches must have explicit maximum operation count and payload size to protect NAS memory/CPU.

## 6. Incremental synchronization

Clients should request only changes after their last valid checkpoint rather than downloading complete collections.

## 7. Authorization

The server evaluates authorization for each affected resource. A valid session does not grant access to arbitrary resource IDs.

## 8. Revoked access

Operations referring to resources for which the client has lost authorization are rejected. The response must instruct the client to stop retrying the operation indefinitely.

## 9. Private content

Private note operations carry encrypted content or encrypted operation payloads according to the approved E2E protocol. The server must not decrypt them to perform ordinary synchronization.

## 10. Conflict handling

The API returns sufficient causal/version information for the client merge engine to converge according to the selected CRDT/operation protocol.

## 11. Tombstones

Deleted resources remain represented long enough for active/offline clients to learn about the deletion and avoid resurrection.

## 12. Retry classes

The protocol should distinguish:

- transient server failure;
- temporary network failure;
- authentication expiration;
- authorization rejection;
- malformed operation;
- cryptographic validation failure;
- true merge/conflict condition.

## 13. Security events

Permission changes and device/security revocations may be returned as synchronization events so clients can update local state promptly.

## 14. Compaction

Acknowledged operations may be compacted/deleted only after the server can guarantee that required clients can still converge according to the defined protocol.
