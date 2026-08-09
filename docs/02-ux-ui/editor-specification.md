# Nymbus — Editor Specification

**Document type:** AFU — Editor behavior and UX  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Editor objective

The Nymbus editor must allow a user to write naturally while retaining Markdown as the underlying content representation. The interface should feel closer to a lightweight document editor than a raw code editor.

## 2. Editing model

The editor must support:

- plain text entry;
- headings;
- emphasis;
- strong emphasis;
- strikethrough;
- ordered lists;
- unordered lists;
- task/check lists;
- links;
- block quotes;
- code/inline code;
- tables;
- images;
- attachments where supported;
- standard undo/redo.

The final supported Markdown dialect must be documented in the technical editor specification before implementation.

## 3. Formatting controls

A visible formatting toolbar provides common operations. The toolbar must not expose every advanced operation as a permanent button.

Recommended primary controls:

- heading/style;
- bold;
- italic;
- strikethrough;
- lists;
- checklist;
- link;
- quote;
- code;
- table;
- image;
- undo/redo.

Less common operations can be grouped into an overflow menu.

## 4. Toolbar behavior

### Wide screens

The toolbar may be persistent near the editor header.

### Small screens

The toolbar should adapt to limited width through grouping, horizontal scrolling, or contextual controls. It must not consume a disproportionate amount of vertical screen space.

## 5. Markdown semantics

Formatting actions must generate valid, deterministic Markdown semantics according to the selected Markdown specification. The UI must not create ambiguous syntax for equivalent operations.

## 6. Headings

Heading level selection should be exposed as a semantic style selector rather than requiring users to type Markdown markers.

## 7. Tables

The editor must provide a guided table creation experience rather than requiring manual Markdown syntax for common table creation.

Table editing should allow:

- adding/removing rows;
- adding/removing columns;
- editing cell content;
- preserving Markdown-compatible structure.

## 8. Images

The editor must support inline image insertion.

The insertion workflow must allow the user to choose an image quality/processing level at upload time because the appropriate trade-off depends on the image use case.

The UI should show upload progress for sufficiently large files and must not block unrelated editor interactions unnecessarily.

## 9. Image behavior

Images should:

- render within the document flow;
- respect content width;
- preserve aspect ratio by default;
- provide an appropriate placeholder while unavailable;
- expose useful alt-text controls for accessibility.

## 10. Attachments

Attachments are represented as document objects distinct from inline images when they are not intended to render directly in the note body.

## 11. Autosave

The editor must persist accepted local changes continuously enough to prevent normal interaction from risking data loss. The UI must distinguish local persistence from server synchronization.

## 12. Undo/redo

Undo/redo is an editor operation and should not be confused with version history. Undo reverses editing operations within the current editing session; version history provides persisted historical states.

## 13. Cursor and selection

Formatting operations must preserve cursor/selection context where technically possible. Applying a style to selected text should not unexpectedly move the cursor to another document position.

## 14. Links

Creating/editing a link should provide a clear URL input and optional display-text behavior. Link validation and safe external navigation behavior must be specified separately.

## 15. Private-note mode

When editing a private note after successful unlock:

- the UI clearly indicates the private state;
- plaintext remains within the local confidentiality boundary;
- synchronization status remains visible without exposing content;
- automatic relock after the defined timeout is enforced;
- after relock, protected editor content must not remain visibly available.

## 16. Locked private-note editor

A locked private note must not render protected body content. Instead it shows:

- title;
- permitted metadata;
- private/locked state;
- unlock action;
- safe metadata actions.

## 17. Editor errors

If local persistence fails, the UI must explicitly warn the user and avoid claiming that work is safely saved.

If synchronization fails, the note remains locally available according to offline rules and the sync state becomes visible.

## 18. Editor performance

The editor must remain responsive for normal notes without triggering unnecessary backend operations for each keystroke. Large documents and attachments must be processed incrementally where practical.

## 19. Keyboard shortcuts

Common desktop shortcuts should be supported where they are standard and do not conflict with browser behavior. Every shortcut must have an equivalent accessible UI action.

## 20. Future editor extensions

V1 should not introduce arbitrary block types merely because the underlying editor framework supports them. New content types require an explicit product decision and documentation update.
