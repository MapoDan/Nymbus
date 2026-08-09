# Nymbus — Accessibility Guidelines

**Document type:** AFU — Accessibility specification  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Target

Nymbus V1 targets WCAG 2.2 AA principles for the core application experience.

## 2. Keyboard navigation

All primary workflows must be usable without a mouse:

- navigation;
- search;
- note creation;
- editing;
- formatting;
- dialogs;
- sharing;
- version history;
- export;
- settings;
- security flows.

## 3. Focus

Focus must always be visible and must move logically when dialogs, menus, sheets and navigation destinations open or close.

When a modal opens, focus moves into it. When it closes, focus returns to the triggering control unless the workflow has navigated elsewhere.

## 4. Screen readers

Interactive controls must expose meaningful accessible names. Icon-only controls must have accessible labels.

Dynamic states such as synchronization, locked/unlocked status and errors must be communicated through accessible semantics rather than visual color alone.

## 5. Color and contrast

The chosen palette must satisfy the applicable WCAG AA contrast requirements for normal text, large text and user-interface components.

The design system must not use color as the only distinction between:

- locked/unlocked;
- sync states;
- errors/warnings;
- selected/unselected states.

## 6. Reduced motion

The application must respect the operating system/browser reduced-motion preference.

When reduced motion is requested, non-essential transitions should be removed or minimized.

## 7. Text scaling

The interface must remain usable when text is enlarged through browser/platform accessibility settings. Content must not become clipped solely because text size increases.

## 8. Editor accessibility

The editor must provide accessible equivalents for formatting controls and must not require visual Markdown knowledge.

Tables, images and links require appropriate semantic/accessibility metadata.

## 9. Images

Users should be able to provide useful alternative text for inline images. Decorative images should be distinguishable from informative images.

## 10. Private-note accessibility

Security controls must remain understandable to users relying on assistive technology. A lock icon alone is insufficient; the accessible name/state must communicate the private/locked condition.

## 11. Error accessibility

Errors must be associated with the relevant control where possible and must not be communicated solely through transient visual effects.

## 12. Touch accessibility

Touch controls must have sufficiently large interactive areas and enough spacing to reduce accidental activation.

## 13. Accessibility and responsive behavior

Changing from sidebar navigation to top navigation must not alter the semantic navigation structure exposed to assistive technologies.

## 14. Testing

Before V1 release, accessibility verification should include:

- keyboard-only navigation;
- screen-reader smoke tests on supported major platforms;
- contrast validation;
- reduced-motion validation;
- browser zoom/text scaling;
- touch target review;
- focus-trap and focus-return tests for dialogs/sheets.
