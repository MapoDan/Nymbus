# Nymbus — Responsive Behavior

**Document type:** AFU — Responsive UX rules  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Responsive philosophy

Nymbus is a PWA and must behave as one coherent application across phone, tablet, laptop and desktop. Responsive behavior changes layout and density, not product semantics.

## 2. Layout modes

### Compact

For narrow phone-sized viewports.

Characteristics:

- fixed top navigation;
- single primary content column;
- contextual actions in menus/sheets;
- editor optimized for touch and vertical scrolling;
- formatting controls may become horizontally scrollable or grouped.

### Medium

For larger phones and tablets.

Characteristics:

- top navigation or adaptive navigation according to available width;
- optional secondary pane where useful;
- larger editor margins;
- touch-friendly controls.

### Wide

For laptops and desktops.

Characteristics:

- persistent lateral sidebar;
- optional note-list/content split where appropriate;
- generous editor width and margins;
- persistent access to common navigation/search controls.

## 3. Breakpoint rule

Exact pixel breakpoints must be established by the implementation design system rather than hardcoded independently in individual components. Components must use shared responsive tokens.

## 4. Touch targets

Interactive controls on touch devices must provide appropriately sized hit areas even when their visual icon is small.

## 5. Editor adaptation

The editor must never become unusably narrow merely because navigation remains visible. At smaller widths, navigation and secondary controls must yield space to the content.

## 6. Images

Inline images must scale to the available content width without overflowing the viewport. Images must retain their intrinsic aspect ratio unless the user explicitly changes presentation behavior.

## 7. Tables

Markdown tables must remain readable. When a table cannot fit the viewport, the table region may scroll horizontally rather than forcing the entire page to overflow.

## 8. Dialogs and sheets

On desktop, dialogs may use centered modal presentation. On mobile, complex dialogs should become full-width sheets or full-screen flows where that improves usability and keyboard behavior.

## 9. Keyboard behavior

Responsive behavior must account for the mobile software keyboard. The active editor position and relevant controls must remain visible when the keyboard opens.

## 10. Orientation

Portrait is the primary mobile orientation. Landscape must remain usable but does not require a dedicated alternate information architecture.

## 11. Accessibility

Responsive transformations must not remove keyboard accessibility, focus visibility, semantic labels or logical reading order.
