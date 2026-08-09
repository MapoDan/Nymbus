# Nymbus — Retention

**Status:** V1 baseline

## 1. Purpose

Retention defines how long logical records, versions, synchronization information and deleted content remain available.

## 2. User-visible note history

Exactly 15 note versions are retained as the visible history in V1.

## 3. Trash

Deleted notes and attachments enter a trash state before permanent deletion according to the product retention configuration. The exact user-facing trash duration must be defined in the feature specification before implementation.

## 4. Permanent deletion

Permanent deletion must remove the logical resource and, after all synchronization/restore safety conditions are satisfied, physically remove associated stored content and cryptographic references that no longer have a legitimate retention reason.

## 5. Security logs

Security/audit events should be retained only for the period necessary for security operations, troubleshooting and accountability. Secrets and private plaintext must never be retained in audit logs.

## 6. Synchronization data

Internal sync operations may require retention beyond visible note history so lagging clients can converge. Compaction is allowed once the documented convergence safety conditions are met.

## 7. Private encrypted content

Retention does not permit decryption. A backup or retained historical ciphertext remains protected by the encryption model.

## 8. Recovery transactions

Recovery transactions are temporary and must expire after their defined validity window. The recovery key is valid for 10 minutes and is single-use.

## 9. Backups

Backup retention is an infrastructure concern and must preserve the relationship between ciphertext, metadata and protected key envelopes. A backup policy must not create an unencrypted copy of private content.

## 10. Deletion versus revocation

Deletion removes a resource. Revocation removes a user's future authorization. They are distinct operations and must not be conflated.

## 11. Future policy decision

Exact durations for trash, audit logs, sync-operation retention and infrastructure backups remain configurable/documented at the relevant feature/infrastructure layer and must not be invented by the implementation.
