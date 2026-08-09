# Nymbus — Notes API Contract

**Document type:** AFU — Notes resource API  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Scope

The notes API manages note metadata and encrypted/private content transport.

## 2. List notes

The list operation returns only notes the authenticated user is authorized to see.

The response may include:

- note ID;
- title;
- tags;
- folder;
- favorite state;
- privacy state;
- timestamps;
- synchronization state;
- current version metadata;
- sharing summary where permitted.

It must not return private plaintext to an unauthorized or locked client.

## 3. Read note

For a public/non-encrypted note, the API may return its content according to authorization.

For a private note, the API returns the encrypted representation and required non-secret cryptographic metadata. Decryption occurs client-side.

## 4. Create note

The client may create a note locally first and synchronize it later.

The create contract must support client-generated IDs and idempotent retry.

## 5. Update note metadata

Metadata updates are independently synchronized from document content where practical.

The server validates ownership/permission before applying the update.

## 6. Update private content

The API receives encrypted content/operations according to the synchronization protocol.

The endpoint must never require plaintext private content.

## 7. Delete note

Deletion requires authorization and creates synchronization state/tombstone as required.

Permanent physical deletion may be deferred while historical versions or pending operations reference the resource.

## 8. Favorite

Favorite state is a user-specific metadata property and does not affect note authorization.

## 9. Tags

Tag associations are managed through the note/tag API model and remain server-readable metadata.

## 10. Locked private note behavior

The API does not know whether a client has locally unlocked the note. It returns the encrypted data permitted by server authorization. Local decryption remains entirely client-side.

## 11. Optimistic concurrency

Updates must include the relevant synchronization/version context so stale clients cannot silently overwrite newer state.

## 12. Sharing

Sharing operations are separate from ordinary note updates because they alter authorization and potentially cryptographic key distribution.

## 13. Audit

Creation, deletion, sharing and security-sensitive operations may produce audit events. Content bodies must never be copied into the audit event.
