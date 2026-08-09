# Nymbus — Application Layout

**Document type:** AFU — Global application layout  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Responsive strategy

Nymbus uses three conceptual layout ranges:

- small/mobile;
- tablet;
- desktop/wide.

Breakpoints must be selected from content and usability requirements rather than from device model names.

## 2. Desktop/tablet navigation

On sufficiently wide screens, Nymbus uses a persistent left sidebar containing the primary navigation.

The sidebar should expose only high-value destinations and remain visually quiet.

## 3. Mobile navigation

On narrow screens, the persistent sidebar is removed. Its functions move to a compact mobile navigation mechanism.

The formatting/editor controls that were defined as fixed top controls remain fixed at the top on small screens.

## 4. Main content

The central workspace is the primary visual focus. It should not be constrained by unnecessary cards or decorative chrome.

## 5. Note list

The note list prioritizes:

- title;
- folder/context;
- tags when useful;
- synchronization/security state;
- modified date;
- favorite state.

Private-note content snippets must not appear while locked.

## 6. Editor layout

When a note is open, the editor occupies the majority of available space. Metadata and actions remain accessible without permanently consuming large portions of the viewport.

## 7. Sidebar sections

The exact navigation may include:

- All Notes;
- Favorites;
- folders;
- tags;
- shared/collaborative area;
- settings.

Channels/threads are shown only when enabled in account settings and relevant to the user.

## 8. Search

Search must be globally reachable and visually prominent without dominating the interface.

## 9. Security state

Private-note locked/unlocked status must be immediately understandable from the note context. It must not be represented only through a tiny lock icon.

## 10. Synchronization state

A compact synchronization indicator may appear in the application shell/editor. It should provide enough information to distinguish local save, synchronization and offline conditions without becoming distracting.

## 11. Empty states

Empty states should be concise and action-oriented. They should explain what the user can do next rather than presenting decorative illustrations as the primary content.

## 12. Modals

Use modal dialogs only for focused decisions that genuinely require interruption, such as security confirmation, destructive actions or note-specific password configuration.

## 13. Panels

Non-destructive secondary information should prefer panels/sheets over full blocking modals where responsive behavior allows.

## 14. Wide-screen density

The application should take advantage of large screens without stretching text lines excessively. Editor reading width should remain comfortable while secondary navigation remains available.

## 15. Mobile ergonomics

Controls must remain reachable with one hand where practical. Destructive actions should not be placed immediately adjacent to frequent primary actions.
