# Nymbus — UX Principles

**Document type:** AFU — UX principles  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Product UX direction

Nymbus must feel like a lightweight personal knowledge tool rather than an enterprise document-management suite. Complexity belongs behind progressive disclosure, not in the primary interface.

## 2. Core principles

### UX-001 — Content first

The note and its content are always the primary visual focus. Navigation and metadata must support writing rather than compete with it.

### UX-002 — Simple by default

The first visible state should expose only actions needed for the current task. Advanced actions appear contextually.

### UX-003 — Fast interaction

Common actions such as creating a note, changing a folder, tagging, searching and formatting must require minimal interaction steps.

### UX-004 — Security without friction

Private-note security must be obvious and trustworthy but should not make ordinary note-taking feel like a security application.

### UX-005 — Never hide security state

A user must always be able to understand whether a note is normal, private/locked, private/unlocked, synchronized, pending synchronization, or in an error state.

### UX-006 — Progressive disclosure

Complex controls such as sharing, version history, export options, advanced security settings and metadata management should be available without occupying permanent primary-interface space.

### UX-007 — Predictable behavior

The same action must have the same semantic meaning throughout the product, regardless of screen size.

### UX-008 — Local confidence

When the user edits content locally, the interface should acknowledge the local save state independently from backend synchronization.

### UX-009 — Responsive adaptation

Desktop/tablet layouts should exploit space with a lateral sidebar. Small screens should move primary navigation to a fixed top area.

### UX-010 — Accessibility is structural

Accessibility must be considered in component behavior, focus, keyboard navigation, contrast and semantics rather than added after visual design.

### UX-011 — Calm visual language

Nymbus should use generous whitespace, restrained surfaces, clear hierarchy and limited decorative elements.

### UX-012 — Meaningful motion

Animation is used only to communicate state, hierarchy or continuity. Decorative motion must not slow down interaction.

### UX-013 — Error recovery

Error messages must explain what happened, what was preserved, and what the user can do next.

### UX-014 — Destructive actions are explicit

Deleting notes, revoking access, removing passkeys, discarding changes and irreversible security actions require appropriately clear confirmation.

### UX-015 — Privacy by presentation

Private plaintext must never accidentally appear in notification previews, browser-visible metadata, locked-state snippets or generic UI surfaces.

## 3. Interaction hierarchy

Primary actions:

- New note;
- Search;
- Open note;
- Edit content;
- Save/synchronize state;
- Unlock private note when relevant.

Secondary actions:

- Folder/tag management;
- Favorite;
- Share;
- Version history;
- Export;
- Attachment management.

Tertiary/advanced actions:

- Security configuration;
- Device/passkey management;
- Administrative controls;
- Advanced account settings.

## 4. UX state model

Every relevant resource should have a visually distinguishable state where applicable:

- normal;
- private locked;
- private unlocked;
- saving locally;
- pending sync;
- synchronized;
- sync conflict/reconciliation;
- offline;
- unavailable;
- permission denied;
- error.

## 5. AI implementation guidance

When a future implementation decision is ambiguous, prefer the solution that:

1. preserves the user's existing work;
2. minimizes interaction steps;
3. keeps security state explicit;
4. avoids unnecessary UI complexity;
5. works consistently across desktop and mobile;
6. does not increase NAS resource consumption merely for visual convenience.
