# Nymbus — Design System

**Document type:** AFU — UI/UX and visual design system  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Design objective

Nymbus must feel lightweight, calm, modern and trustworthy. The interface should communicate that it is a focused notes application rather than an overloaded productivity suite.

Design priorities:

1. readability;
2. low cognitive load;
3. fast interaction;
4. clear security states;
5. responsive behavior;
6. accessibility;
7. visual consistency.

## 2. Brand personality

Nymbus should feel:

- intelligent;
- discreet;
- modern;
- slightly distinctive/creative;
- secure without looking intimidating;
- minimal rather than sterile.

Avoid a generic "corporate security" aesthetic, excessive gradients, glassmorphism and decorative UI that consumes space.

## 3. Color palette

### Primary

- **Nymbus Indigo:** `#6366F1`
- **Nymbus Indigo Dark:** `#4F46E5`
- **Nymbus Indigo Soft:** `#EEF2FF`

### Neutrals

- **Background:** `#F8FAFC`
- **Surface:** `#FFFFFF`
- **Surface Elevated:** `#FFFFFF`
- **Border:** `#E2E8F0`
- **Text Primary:** `#0F172A`
- **Text Secondary:** `#475569`
- **Text Muted:** `#94A3B8`

### Semantic

- **Success:** `#10B981`
- **Warning:** `#F59E0B`
- **Danger:** `#EF4444`
- **Info:** `#0EA5E9`

Semantic colors must not be the only means of communicating state.

## 4. Dark mode

Dark mode should be supported by the design system rather than implemented as a simple color inversion.

Suggested dark foundations:

- background near `#0B1120`;
- surface near `#111827`;
- elevated surface near `#1E293B`;
- border near `#334155`;
- primary text near `#F8FAFC`;
- secondary text near `#CBD5E1`.

Exact dark-mode tokens must be validated for WCAG contrast before implementation.

## 5. Typography

Use a modern system-oriented sans-serif stack for UI text to minimize bundle size and preserve native rendering.

Recommended hierarchy:

- Display: 28–32px;
- Page title: 24–28px;
- Section title: 18–20px;
- Body: 15–16px;
- Secondary: 13–14px;
- Caption: 12px.

The note editor may use a dedicated readable text style distinct from dense UI labels.

## 6. Spacing

Use a consistent 4px-based spacing scale. Components should use a small number of standardized spacing tokens rather than arbitrary values.

## 7. Radius

Use restrained rounded corners. Suggested base radius: 8px; larger containers may use 12px.

Avoid excessive pill-shaped UI except for tags, status chips and genuinely compact controls.

## 8. Elevation

Prefer borders and subtle surface contrast over large shadows. Shadows should communicate hierarchy, not decoration.

## 9. Icons

Use one coherent icon family with consistent stroke weight. Icons must be paired with labels where the meaning is not immediately recognizable.

## 10. Buttons

Primary actions use the primary color. Destructive actions use the danger semantic token and require clear confirmation where irreversible.

## 11. Inputs

Inputs must have clear labels, visible focus states and validation feedback. Placeholder text is not a replacement for a label.

## 12. Cards

Cards should be used sparingly. The note list should not become a grid of excessive cards; Nymbus should prioritize information density and scanning.

## 13. Status colors

Synchronization and security states must combine icon/text with color so users with color-vision differences can distinguish them.

## 14. Motion

Animations should be short and functional. Avoid animation on every interaction. Respect reduced-motion preferences.

## 15. Accessibility

Target WCAG 2.2 AA for normal application UI. Keyboard focus, contrast, semantics and touch targets are mandatory design considerations.
