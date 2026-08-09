# Nymbus — Product Vision

**Document status:** Draft / V1 baseline  
**Document owner:** Product specification  
**Last updated:** 2026-08-09

## 1. Product identity

**Nymbus** is a lightweight, privacy-oriented, multi-user Markdown note-taking application delivered as a Progressive Web App (PWA) and designed to run on self-hosted NAS infrastructure.

The product combines the simplicity of a modern note application with an explicit security model for sensitive information. Users can create ordinary Markdown notes or private notes whose content and attachments are protected by end-to-end encryption.

Nymbus is designed to feel immediate and uncomplicated. Security should be powerful without becoming visible as unnecessary complexity during normal use.

## 2. Vision statement

> **Nymbus makes personal and collaborative knowledge capture as simple as writing a note, while giving users a practical way to protect sensitive information with true client-side encryption.**

The application should feel lightweight enough to open, write, search, synchronize, and close without the user thinking about the underlying infrastructure.

## 3. Product goals

### G-001 — Simplicity

A new user should be able to authenticate, create a note, write formatted content, organize it, and find it again without learning a complex productivity system.

### G-002 — Privacy

Sensitive note content must be protected by an end-to-end encryption architecture in which the server does not need access to private-note plaintext.

### G-003 — Cross-device convenience

Users should be able to access Nymbus from modern browsers on phones, tablets, laptops, and desktop computers. Passkeys should make subsequent authentication fast and convenient after the first Google login.

### G-004 — Offline resilience

Users should be able to continue working when connectivity to the NAS is temporarily unavailable. Changes must be persisted locally and synchronized later without silent data loss.

### G-005 — Lightweight self-hosting

The application must be practical on a low-resource NAS. The architecture must avoid unnecessary services, background work, persistent connections, and heavyweight dependencies.

### G-006 — Collaboration without sacrificing privacy

Users must be able to share notes with other users while preserving the end-to-end encrypted nature of private notes.

### G-007 — Modern product quality

The application should meet contemporary expectations for responsive design, accessibility, visual hierarchy, keyboard interaction, loading behavior, error handling, and touch interaction.

## 4. Product principles

### 4.1 Simple by default

Nymbus should expose the common action immediately and keep advanced functionality one interaction away.

### 4.2 Secure by architecture, not by warning dialogs

Security must be enforced through data ownership and cryptographic design rather than relying primarily on user behavior.

### 4.3 Local-first for private data

Private-note plaintext belongs on the authorized client. The server is responsible for storing and synchronizing encrypted data and permitted metadata, not for decrypting private content.

### 4.4 Fast perception matters

The application should render useful UI quickly and allow local interaction without waiting unnecessarily for the network.

### 4.5 Resource-aware engineering

Every persistent process and dependency has a cost on a NAS. Architectural simplicity is a feature.

### 4.6 Explicit boundaries

Nymbus should not attempt to become a general-purpose workspace platform. The V1 feature set must remain focused on notes, organization, privacy, synchronization, sharing, and the supporting account/security features.

### 4.7 Recoverable decisions

Important architectural and security choices must be recorded as ADRs so future developers and coding agents understand not only what was selected, but why alternatives were rejected.

## 5. Target experience

A typical user journey should look like this:

1. Open the Nymbus PWA.
2. Sign in with Google on first access.
3. Optionally register a passkey.
4. See a minimal home screen with recent notes, favorites, search, and navigation.
5. Create a Markdown note using a normal writing experience with formatting controls.
6. Organize the note in a folder and assign free-form tags if desired.
7. Mark important notes or folders as favorites.
8. Optionally mark a note as private and protect it with the private-note security flow.
9. Continue working offline if the NAS is temporarily unavailable.
10. Return online and allow Nymbus to synchronize changes.
11. Share a note with another Nymbus user when required.

The security mechanisms should be noticeable when necessary, but should not dominate ordinary note-taking.

## 6. What Nymbus is not

Nymbus V1 is intentionally not:

- a full Notion replacement;
- a project management suite;
- a task manager;
- a database/spreadsheet workspace;
- a calendar application;
- a password manager;
- a file synchronization service;
- a backup solution;
- a general-purpose collaboration platform;
- an import/migration platform.

Content import is explicitly deferred to a possible V2.

Application-level backup is explicitly outside the V1 scope; backup of Nymbus storage is handled by the NAS/infrastructure layer.

## 7. V1 success criteria

V1 should be considered successful when a user can:

- authenticate with Google;
- subsequently authenticate using a registered passkey;
- create, edit, organize, search, favorite, and delete notes;
- write Markdown through a normal visual editor;
- insert images and generic attachments;
- work offline and synchronize changes reliably;
- create private E2E-encrypted notes;
- unlock private notes using the defined security mechanisms;
- search private-note metadata while the note remains locked;
- unlock eligible private notes in bulk and search their decrypted content locally;
- share private notes while keeping them E2E encrypted;
- revoke another user's access according to the documented revocation model;
- review version history with the defined 15-version retention;
- export notes as Markdown or PDF;
- manage devices and passkeys;
- receive and configure in-app/Web Push notifications;
- use the application comfortably on phone, tablet, and desktop/laptop layouts.

## 8. Quality bar

Nymbus should be evaluated on four dimensions:

1. **Correctness** — data and synchronization behavior are reliable.
2. **Security** — the cryptographic and authorization model is enforced rather than implied.
3. **Usability** — ordinary note-taking requires minimal cognitive overhead.
4. **Efficiency** — the system remains practical on low-resource self-hosted infrastructure.

No single dimension should be optimized by silently compromising a higher-priority requirement.
