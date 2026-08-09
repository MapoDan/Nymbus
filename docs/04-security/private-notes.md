# Nymbus — Private Notes Security

**Document type:** AFU — Private-note protection behavior  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Definition

A private note is a note whose content is protected by Nymbus's E2E encryption model.

## 2. Protected content

The protected content includes the complete document body, inline images, private attachments and encrypted version payloads.

## 3. Visible metadata

The following remain intentionally available for ordinary organization/search, subject to the final metadata classification:

- title;
- user-assigned tags;
- synchronization status;
- other explicitly approved non-content metadata.

No private body snippet may be generated server-side.

## 4. Locked state

A locked private note may appear in lists and metadata search, but its body must not be rendered in plaintext.

The UI must clearly distinguish locked content from unavailable/unauthorized content.

## 5. First unlock

The first unlock requires the note's configured protection secret/path. This is the security bootstrap for that note.

After initialization, the user may configure the approved convenient local unlock mechanism.

## 6. Protection modes

A private note can use:

1. the user's master-password protection path; or
2. a dedicated password for that note.

The dedicated password protects the note-key access path and does not replace the note content encryption key.

## 7. Unlock session

Successful local unlock grants plaintext access for a maximum of 15 minutes according to the approved activity policy.

The client must lock the note again when the unlock period expires.

## 8. Bulk unlock

The user can request bulk unlock for private notes whose protection has already been initialized.

Notes requiring first-time password setup cannot be silently included.

## 9. Search

Metadata search works while notes are locked.

Content search is available only after the relevant private content has been decrypted locally and indexed/searchable according to the client-side security model.

## 10. Editing

While unlocked, the editor operates on plaintext locally. Synchronization transmits only the protected representation for private content.

## 11. Offline

Previously initialized encrypted notes may be available offline if the required ciphertext and local key-access material exist. Offline operation does not disable the 15-minute unlock policy.

## 12. Timeout during editing

If the timeout occurs while editing, Nymbus must preserve unsynchronized work securely before transitioning the private content to the locked state. The user must not lose work solely because the timeout occurred.

## 13. Permission loss

If authorization is revoked while a note is open, the client must stop further authorized synchronization and transition to the appropriate unavailable state.

## 14. Export

Exporting a private note creates plaintext output and therefore represents an explicit security-boundary transition. The user should receive a clear indication that the exported file is no longer protected by Nymbus's storage encryption.

## 15. Copy/paste

The browser may expose plaintext through normal user actions while a note is unlocked. Nymbus should not claim to prevent screenshots, clipboard copying or other endpoint-level extraction.

## 16. Cache

Application caches must not intentionally retain plaintext private content beyond the minimum necessary editing/search lifecycle.

## 17. Deletion

Deleting a private note must remove its server-readable metadata according to the trash/retention policy and make its encrypted content inaccessible through normal authorization paths. Cryptographic destruction rules belong to the retention specification.
