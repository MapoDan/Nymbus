# Nymbus — Private Notes Security

**Document type:** AFU — Private-note protection behavior  
**Status:** V1 baseline  
**Last updated:** 2026-09-16

## 1. Definition

A private note is a note whose content is protected by Nymbus's E2E encryption model.

## 2. Protected content

The protected content includes the complete document body, inline images, private attachments and encrypted version payloads.

## 3. Visible metadata

The following remain intentionally available for ordinary organization/search, subject to the final metadata classification:

- title;
- user-assigned tags;
- folder association;
- ownership/sharing metadata;
- synchronization status;
- timestamps and version metadata required by the application.

No private body snippet may be generated server-side.

## 4. Locked state

A locked private note may appear in lists and metadata search, but its body must not be rendered in plaintext.

The UI must clearly distinguish locked content from unavailable/unauthorized content.

## 5. Master-password protection

All private notes are protected exclusively through the user's master-password key hierarchy.

V1 has no dedicated per-note password and no temporary note password.

The master password is established during the user's initial application activation through the administrator-initiated activation/recovery process. The administrator must never receive the master password.

## 6. First unlock / initialization

A newly initialized private note requires the user's master-password-based protection path before convenient platform-authenticator unlock can be enabled.

The client generates the note key locally and protects the note key using the user's key-encryption hierarchy. The master password is never transmitted to the backend.

## 7. Shared private notes

When a private note is shared, the note key becomes accessible through separate protected key-access records for the owner and every authorized recipient.

Each recipient accesses the note using their own master password. No recipient receives the owner's master password.

## 8. Unlock session

Successful local unlock grants plaintext access for a maximum of 15 minutes according to the approved activity policy.

The client must lock the note again when the unlock period expires.

## 9. Bulk unlock

The user can request bulk unlock for private notes whose protection has already been initialized.

Notes requiring first-time initialization cannot be silently included.

## 10. Search

Metadata search works while notes are locked.

Content search is available only after the relevant private content has been decrypted locally and indexed/searchable according to the client-side security model.

The local private search index is itself sensitive and must follow the same local protection lifecycle.

## 11. Editing

While unlocked, the editor operates on plaintext locally. Synchronization transmits only the protected representation for private content.

## 12. Offline

Previously initialized encrypted notes may be available offline if the required ciphertext and local key-access material exist. Offline operation does not disable the 15-minute unlock policy.

## 13. Timeout during editing

If the timeout occurs while editing, Nymbus must preserve unsynchronized work securely before transitioning the private content to the locked state. The user must not lose work solely because the timeout occurred.

## 14. Permission loss and revocation

If authorization is revoked while a shared private note is open, the client must stop further authorized synchronization and transition to the appropriate unavailable state.

For strong cryptographic revocation, the note key must be rotated when required by the security policy so the revoked recipient cannot legitimately obtain future content generations.

Revocation cannot erase plaintext already viewed, copied or exported.

## 15. Export

Exporting a private note creates plaintext output and therefore represents an explicit security-boundary transition. The user should receive a clear indication that the exported file is no longer protected by Nymbus's E2E storage model.

## 16. Copy/paste

The browser may expose plaintext through normal user actions while a note is unlocked. Nymbus should not claim to prevent screenshots, clipboard copying or other endpoint-level extraction.

## 17. Cache

Application caches must not intentionally retain plaintext private content beyond the minimum necessary editing/search lifecycle.

## 18. Deletion

Deleting a private note must remove its server-readable metadata according to the trash/retention policy and make its encrypted content inaccessible through normal authorization paths. Cryptographic destruction rules belong to the retention specification.
