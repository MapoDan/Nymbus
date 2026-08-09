# Nymbus — Information Architecture

**Document type:** AFU — Information architecture  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Primary information model

Nymbus is organized around five primary concepts:

1. Notes — the central content object.
2. Folders — hierarchical organization.
3. Tags — cross-cutting classification.
4. Favorites — fast access to important resources.
5. Shared content — resources available through explicit permissions.

Account/security and administration are secondary domains and must not compete with note navigation.

## 2. Main navigation

The primary navigation contains:

- All notes;
- Favorites;
- Folders;
- Tags, where enabled by the final UI decision;
- Shared with me;
- Shared by me, where useful;
- Search;
- Settings.

The exact label set may be refined during visual design, but the information hierarchy must remain stable.

## 3. Note hierarchy

A note may belong to a folder and may have zero or more tags. Folder membership and tags are independent concepts.

```text
Workspace
├── Favorites
├── All Notes
├── Folders
│   ├── Folder
│   │   ├── Subfolder
│   │   │   └── Notes
│   │   └── Notes
│   └── Folder
├── Shared
└── Settings
```

## 4. Private-note information boundary

Private notes expose only permitted metadata while locked. At minimum this includes:

- title;
- user-assigned tags;
- organizational metadata required by the product;
- synchronization state.

Protected body content, protected inline images and protected attachments remain outside the server-readable metadata model.

## 5. Search information model

Search has two conceptual sources:

### Server-visible metadata

Searchable while a private note is locked, subject to authorization.

### Local decrypted content

Searchable only when the corresponding private note content is locally unlocked.

The UI must clearly distinguish a metadata result from a content match without exposing private plaintext while locked.

## 6. Account area

Account settings should be grouped by task rather than exposing a long flat list.

Suggested groups:

- Account;
- Security & passkeys;
- Privacy;
- Notifications;
- Appearance;
- Application preferences.

## 7. Administration area

Administration is separate from normal note navigation and visible only to users with the required permissions.

Suggested groups:

- Users;
- Devices/security;
- Operational status;
- Audit/security events;
- System configuration.

Administrative navigation must never imply access to private-note plaintext.

## 8. Editor information hierarchy

Within an open note, the hierarchy is:

1. Note title.
2. Context/metadata such as folder, tags and privacy state.
3. Editor content.
4. Contextual editor controls.
5. Secondary note actions such as share, history and export.

The editor content must dominate the available space.

## 9. Mobile hierarchy

On small screens, the primary navigation moves from the sidebar to a fixed top navigation mechanism. Secondary actions should move into contextual menus rather than creating persistent toolbars that consume content area.

## 10. Information architecture rules

- Do not create separate navigation destinations for every feature.
- Avoid duplicate representations of the same resource unless they serve a clear access pattern.
- Keep security controls close to the resource they protect while retaining account-wide security controls in Settings.
- Do not expose locked private-note content in any navigation surface.
- Search should be globally accessible.
