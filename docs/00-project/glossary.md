# Nymbus — Domain Glossary

**Document status:** Baseline  
**Last updated:** 2026-08-09

This glossary defines terminology that must be used consistently in requirements, code, API contracts, UI copy, tests, and documentation.

| Term | Definition |
|---|---|
| Account | The Nymbus identity associated with a Google account and its Nymbus data. |
| User | A person represented by a Nymbus account. |
| Device | A browser/device installation registered for an authenticated Nymbus user. |
| Passkey | A WebAuthn credential registered by a user for subsequent authentication after the initial Google login. |
| Session | An authenticated period during which a client is authorized to use Nymbus services. |
| Note | A Markdown document managed by Nymbus. |
| Private note | A note whose content is protected by Nymbus's E2E encryption model. |
| Normal note | A note that is not protected by the private-note E2E model. |
| Note metadata | Non-content properties such as title, tags, folder, author, dates, favorite state, synchronization state, and other explicitly permitted attributes. |
| Plaintext | Decrypted human-readable note content or other protected data before encryption. |
| Ciphertext | Encrypted representation of protected data. |
| E2E / E2EE | End-to-end encryption in which the protected content is encrypted/decrypted on authorized clients and the backend does not require plaintext access. |
| Master Password | A user-controlled secret used by the Nymbus private-note security model as defined by the cryptographic specification. It is distinct from Google account authentication. |
| Note Password | A user-controlled secret dedicated to protecting one private note. |
| First unlock | The initial private-note access operation that establishes the user's local protection choice and required cryptographic state. |
| Unlock | The operation that makes the plaintext/key material of a protected note available to an authorized client. |
| Bulk unlock | An explicit user action that attempts to unlock multiple eligible private notes at once. It applies only to notes that have already completed first-time password initialization. |
| Auto-lock | Automatic expiration of a private-note unlocked state. V1 uses a 15-minute timeout. |
| Key wrapping | A cryptographic technique used to protect an encryption key with another key or secret. Exact use is defined by the key-management specification. |
| Recovery key | A temporary secret used during the defined account/private-note recovery flow. V1 requires a 10-minute validity period for the temporary recovery key. |
| Folder | A logical container for notes and/or nested folders. |
| Shared folder | A folder whose access is granted to one or more individual users and may be inherited by nested resources. |
| Tag | A free-form user-defined label attached to a note for organization and search. |
| Favorite | A user-specific marker that promotes a note or folder for quick access. |
| Attachment | A generic file associated with a note, such as a PDF, document, archive, or text file. |
| Inline image | An image rendered as part of the note body rather than only as a downloadable attachment. |
| Export | A user-initiated conversion of Nymbus content into Markdown or PDF. |
| Version | A retained historical state of a note. V1 retains 15 versions according to the versioning specification. |
| Trash | The logical deletion state used before permanent deletion according to the retention/deletion specification. |
| Synchronization | The process of exchanging locally persisted changes and authoritative server state between clients and the backend. |
| Sync state | User-visible state describing whether local changes are synchronized, pending, conflicted, failed, or otherwise require attention. |
| CRDT | Conflict-free Replicated Data Type used to support deterministic convergence of concurrent/offline edits according to the synchronization specification. |
| Offline | A client state in which the application cannot currently communicate with the Nymbus backend but can continue supported local operations. |
| Real-time collaboration | Low-latency synchronization activated when multiple users are actively working on the same note. |
| Revocation | Removal of a user's authorization to access a resource or device. |
| Strong revocation | V1 sharing revocation behavior that includes authorization invalidation, key rotation for remaining authorized users where required, and best-effort removal of the revoked user's local Nymbus copy after reconnection. |
| RBAC | Role-based access control. In Nymbus, authorization is granular rather than restricted to a fixed user/admin pair. |
| Permission | An explicit authorization capability assigned to a user or role. |
| Web Push | Browser push notification mechanism used by Nymbus where supported. |
| Notification channel/thread | A configurable category of notifications that the user may enable or disable from account settings. |
| PWA | Progressive Web App: a web application installable and usable through supported browser/platform capabilities. |
| NAS | Network Attached Storage device hosting the self-hosted Nymbus deployment. |
| Service | A logical backend component with a defined responsibility. Service boundaries must be justified by architecture requirements. |
| Resource budget | The expected CPU, RAM, storage, network, and process footprint permitted for a low-resource NAS deployment. |
| V1 | The first defined production scope described in `docs/00-project/scope.md`. |
| V2 | Future functionality not included in V1. |

## Terminology rules

1. Use **private note**, not “encrypted note”, in user-facing terminology unless discussing the implementation.
2. Use **private-note plaintext** for decrypted private content and **ciphertext** for its stored/transmitted encrypted representation.
3. Do not call Google authentication a “master login” or “encryption unlock”. They are separate concepts.
4. Do not describe passkeys as encryption keys.
5. Do not promise irreversible remote deletion of information already copied outside Nymbus.
6. Use **folder inheritance** for permissions inherited from a parent folder.
7. Use **sync state** for synchronization status shown to the user.
8. Use **bulk unlock** for the explicit operation that unlocks multiple eligible private notes.
