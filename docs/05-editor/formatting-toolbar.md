# Nymbus — Formatting Toolbar

**Document type:** AFU — Editor toolbar specification  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Principle

Formatting controls must expose the most useful actions without turning the editor into a complex word processor.

## 2. Primary controls

The toolbar should expose, according to available space:

1. text style/heading;
2. bold;
3. italic;
4. strikethrough;
5. link;
6. unordered list;
7. ordered list;
8. checklist;
9. quote;
10. code/inline code;
11. table;
12. image;
13. additional actions menu where necessary.

## 3. Contextual behavior

Controls that require a selection should visibly reflect whether they are applicable.

For example, link insertion may use the selected text; heading applies to the current block.

## 4. Heading control

The heading control must support the heading levels approved by the document model and expose a readable label such as "Heading 1" rather than relying only on an icon.

## 5. Link control

The link action opens a compact input for URL and, when applicable, link text. URLs are sanitized/normalized before being stored.

## 6. Image control

The image action opens the platform file picker and then the image processing/quality choice required by the attachment specification.

## 7. Table control

Table insertion should be lightweight. The UI may offer a simple grid to select initial rows/columns. Subsequent rows/columns can be added through contextual table controls.

## 8. Code

Inline code and code block are separate operations.

Code blocks should support a plain-language/code-language selection only if that capability is actually useful to rendering/export; avoid unnecessary editor complexity in V1.

## 9. Undo/redo

Undo and redo should be available through standard keyboard shortcuts and optionally visible controls where platform layout permits.

## 10. Responsive placement

Wide layouts may place the toolbar in the editor/header region or persistent UI area.

On small screens the formatting toolbar must remain fixed at the top as previously defined for Nymbus mobile UI, without covering the active editing region unnecessarily.

## 11. Overflow

Less frequently used controls may be grouped under an overflow menu. The primary toolbar must remain stable and predictable rather than dynamically reshuffling after every keystroke.

## 12. Accessibility

Icon-only controls require accessible labels/tooltips. Tooltips must not be the only accessible name.

## 13. State indication

Active formatting states must be visually distinguishable, e.g. when the caret is inside bold text or a selected block is a heading.

## 14. Touch targets

Controls must use touch targets appropriate for mobile interaction and avoid accidental adjacent activation.
