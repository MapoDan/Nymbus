# Nymbus — Synchronization

## 1. Model

Synchronization is operation-based and designed for intermittent connectivity. Clients exchange local changes and remote changes rather than treating every reconnect as a full database replacement.

## 2. Operation identity

Every synchronizable mutation must be uniquely identifiable so retries are idempotent.

## 3. Ordering

The synchronization layer tracks causal/version information sufficient to determine whether a remote state is newer, concurrent or already incorporated.

## 4. Conflict policy

Collaborative document content uses the approved CRDT model. Non-document metadata uses explicit last-writer or domain-specific rules documented per entity.

Security-sensitive permission changes are not resolved by CRDT semantics alone; server authorization remains authoritative.

## 5. Private notes

Synchronization transports ciphertext and permitted metadata. It must never require server-side decryption to merge or distribute private note content.

## 6. Attachments

Attachment synchronization uses content identity/version information and bounded transfers. Large objects should not be duplicated unnecessarily on retries.

## 7. Deletion

Deletes are represented in synchronization state long enough for authorized clients to converge. Permanent retention/deletion rules are defined separately by the data retention specification.

## 8. Revocation

Permission revocation must propagate independently of ordinary content convergence so that a revoked client cannot rely on stale synchronization state to regain access.

## 9. Sync status

The client must expose synchronization state at minimum as:

- synchronized;
- pending;
- synchronizing;
- offline;
- conflict/error requiring attention.

## 10. Resource constraints

Sync must use incremental batches, bounded payloads and backoff. V1 must not require a continuously running message broker.
