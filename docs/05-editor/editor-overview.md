# Nymbus — Editor Overview

**Document type:** AFU — Editor functional specification  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Objective

The Nymbus editor must feel as immediate and simple as a modern lightweight note application while preserving Markdown semantics, inline images, offline editing and the E2E boundary of private notes.

## 2. Editing philosophy

The editor is content-first:

- the user writes normally;
- formatting controls are visible and understandable;
- Markdown is the underlying document representation;
- technical Markdown syntax should not obstruct ordinary writing;
- advanced Markdown remains possible where supported.

## 3. Supported content

V1 supports:

- paragraphs;
- headings;
- bold;
- italic;
- strikethrough;
- inline code;
- links;
- ordered lists;
- unordered lists;
- task/checkbox lists;
- block quotes;
- code blocks;
- tables;
- horizontal separators;
- inline images.

Unsupported Markdown constructs must be handled safely rather than silently interpreted as arbitrary HTML.

## 4. Document model

The editor must operate on a structured document model that can be serialized to Markdown and synchronized efficiently.

The implementation must not treat raw Markdown text as the only internal representation if doing so would make collaborative editing or inline objects unnecessarily fragile.

## 5. Source of truth

The document model is the editing source of truth. Markdown is an interchange/rendering representation.

This distinction is important for tables, images, checkboxes and future rich content.

## 6. Autosave

Editing must persist locally without requiring the user to press Save.

The UI may expose synchronization status but should not interrupt typing with save dialogs.

## 7. Undo/redo

Undo and redo must be local-editor operations and must remain intuitive during normal typing and formatting.

The implementation must avoid turning every synchronization operation into an individual visible undo step.

## 8. Selection preservation

Applying formatting must preserve the user's selection/caret whenever technically possible.

## 9. Clipboard

Paste behavior must distinguish:

- plain text;
- rich text;
- images;
- links.

Pasted HTML must be sanitized and converted into the supported document model rather than inserted as arbitrary executable HTML.

## 10. Keyboard and pointer

Every toolbar action should be usable by mouse/touch and have a keyboard shortcut where the underlying platform convention supports one.

## 11. Mobile

The editor must remain usable on narrow screens without requiring horizontal scrolling for normal content.

The formatting toolbar uses the mobile-specific placement defined by the UI specification.

## 12. Desktop/tablet

On wider screens, the editor can use the persistent/sidebar layout defined by the global UI specification.

## 13. Accessibility

Editor controls require:

- accessible names;
- keyboard navigation;
- visible focus state;
- sufficient contrast;
- screen-reader-compatible state for toggles and dialogs.

## 14. Security

The editor must never execute arbitrary HTML/scripts supplied through note content.

Links and embedded content must be sanitized and normalized according to the security specification.

## 15. Collaboration

The editor must expose a document representation compatible with the selected synchronization/CRDT model. The final implementation technology is an ADR decision, not an assumption of this document.

## 16. Private notes

The editor operates on decrypted content only after the client has authorized access to the private note. The backend API must never be required to render or edit private plaintext.
