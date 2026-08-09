# Nymbus — Editor Interaction Specification

**Document type:** AFU — Editor interaction behavior  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Typing

Typing must feel immediate. Network latency must never be part of the typing path.

## 2. Autosave

Changes are persisted locally continuously according to an implementation-defined debounce/batching strategy. The user does not need to manually save.

## 3. Synchronization indicator

The editor exposes a compact state such as:

- Saved locally;
- Syncing;
- Synced;
- Offline;
- Sync issue.

The indicator must not imply that "saved locally" means "stored on the server".

## 4. Formatting selection

Formatting commands apply to the current selection or current block according to standard editor behavior.

## 5. Empty selection

For inline formatting without a selection, the editor should toggle the mark for subsequent typing.

## 6. Keyboard shortcuts

At minimum, standard shortcuts for bold, italic, undo and redo should follow platform conventions.

## 7. Enter behavior

Enter creates the next logical block. In lists/checklists, Enter creates a new item; pressing Enter on an empty list item exits the list.

## 8. Paste

Pasted rich text is normalized into the supported document model. Unsupported formatting is discarded only when it has no safe semantic equivalent.

## 9. Paste images

When the clipboard contains an image, the editor offers/initiates inline image insertion using the attachment flow.

## 10. Drag and drop

Desktop drag-and-drop may support image insertion and, where defined, text content. Files must pass the same validation as file-picker uploads.

## 11. Links

Pasting a URL into selected text should provide conventional link behavior. Pasting a standalone URL may remain a plain URL unless the editor's smart-link behavior explicitly converts it.

## 12. Tables

Arrow keys, Tab and Enter must behave predictably inside tables. The implementation should avoid trapping keyboard navigation indefinitely inside a table.

## 13. Code blocks

Typing inside a code block preserves whitespace and does not apply normal Markdown formatting shortcuts unexpectedly.

## 14. Undo boundaries

User-visible undo should group high-level operations sensibly. For example, inserting an image should be one logical action rather than dozens of internal upload/serialization operations.

## 15. Mobile keyboard

The editor must remain usable when the on-screen keyboard is open. Fixed controls must not obscure the active line or selection.

## 16. Long documents

The editor must avoid rendering unnecessarily expensive hidden content. Virtualization/lazy rendering may be used if needed, but not at the expense of cursor/selection reliability.

## 17. Locked private note

A locked private note must not expose an editable plaintext editor. The UI instead presents the protected/locked state and the appropriate unlock action.

## 18. Unlock during editing session

If the 15-minute unlock session expires while the user is editing a private note, the application must protect the content before permitting further plaintext interaction. Unsynchronized edits must be retained securely and not discarded.

## 19. Permission changes during editing

If edit permission is revoked while the editor is open, the client must stop accepting/synchronizing unauthorized changes and provide a clear state to the user. Previously authorized local work must not be silently discarded.

## 20. Accessibility

Keyboard users must be able to enter, edit, format and exit the editor without requiring pointer interaction.
