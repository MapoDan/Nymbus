# Nymbus — Design System & Visual Guidelines

**Document type:** AFU — Visual design specification  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Visual direction

Nymbus should communicate:

- privacy without looking paranoid;
- simplicity without looking generic;
- technical quality without looking overly technical;
- creativity without visual noise.

The visual language should sit between modern productivity tools and premium security products.

## 2. Brand personality

Keywords:

**Calm · Private · Lightweight · Intelligent · Modern · Precise · Human**

Avoid:

- heavy enterprise aesthetics;
- excessive gradients;
- glassmorphism as a dominant pattern;
- oversized illustrations;
- excessive shadows;
- neon/cybersecurity clichés;
- dense dashboard layouts.

## 3. Color strategy

Nymbus uses a neutral foundation with a distinctive blue-violet brand accent. The accent should communicate technology and privacy while remaining calm enough for a note-taking application.

### Core palette

| Token | Value | Purpose |
|---|---|---|
| `brand-500` | `#6366F1` | Primary brand/accent |
| `brand-600` | `#4F46E5` | Primary interactive emphasis |
| `brand-700` | `#4338CA` | Hover/strong emphasis |
| `brand-100` | `#E0E7FF` | Soft brand surface |
| `brand-50` | `#EEF2FF` | Very subtle brand surface |
| `ink-950` | `#0F172A` | Primary text |
| `ink-700` | `#334155` | Secondary text |
| `ink-500` | `#64748B` | Muted text |
| `ink-300` | `#CBD5E1` | Borders/dividers |
| `ink-200` | `#E2E8F0` | Soft borders |
| `surface-0` | `#FFFFFF` | Primary surface |
| `surface-50` | `#F8FAFC` | Application background |
| `surface-100` | `#F1F5F9` | Secondary surface |
| `success-500` | `#10B981` | Success/synchronized |
| `warning-500` | `#F59E0B` | Warning/pending |
| `danger-500` | `#EF4444` | Error/destructive |
| `info-500` | `#0EA5E9` | Informational |

## 4. Dark theme

Dark mode must not simply invert light colors.

Recommended dark foundation:

| Token | Value | Purpose |
|---|---|---|
| `dark-bg` | `#0B1020` | Application background |
| `dark-surface` | `#111827` | Primary surface |
| `dark-surface-2` | `#172033` | Elevated/secondary surface |
| `dark-border` | `#273449` | Borders |
| `dark-text` | `#F8FAFC` | Primary text |
| `dark-text-secondary` | `#CBD5E1` | Secondary text |
| `dark-text-muted` | `#94A3B8` | Muted text |

Brand accent remains in the indigo family but must be adjusted for contrast where necessary.

## 5. Color semantics

Color must communicate state rather than decoration.

- Green = successful/synchronized/positive.
- Amber = attention/pending/warning.
- Red = destructive/error/security failure.
- Blue = informational.
- Indigo = Nymbus brand/action.

Never rely on color alone to communicate state; use icons, labels or shape/state changes as well.

## 6. Typography

The primary UI font should be a modern system sans-serif stack to avoid bundling unnecessary font files and to keep the PWA lightweight.

Recommended hierarchy:

- Display: 28–32 px, semibold.
- Page title: 24–28 px, semibold.
- Section heading: 18–20 px, semibold.
- Body: 15–16 px, regular.
- Secondary: 13–14 px.
- Caption: 12–13 px.

The final implementation should use shared typography tokens rather than component-specific arbitrary sizes.

## 7. Editor typography

The editor should optimize long-form reading rather than match the application chrome exactly.

Recommended characteristics:

- comfortable line height;
- readable measure, avoiding excessively long lines;
- clear heading hierarchy;
- restrained inline color;
- code blocks visually distinct but not visually dominant.

A dedicated readable serif font may be considered for rendered note content in a future visual variant, but V1 should prefer a single lightweight family unless usability testing demonstrates a benefit.

## 8. Spacing

Use a consistent 4 px base grid.

Recommended spacing tokens:

`4 · 8 · 12 · 16 · 20 · 24 · 32 · 40 · 48 · 64`

Primary application spacing should generally use 8/12/16/24 px increments.

## 9. Border radius

Use moderate rounding rather than extreme pill-shaped UI.

Recommended tokens:

- small controls: 6 px;
- inputs/cards: 8–10 px;
- dialogs: 12–16 px;
- pills/status chips: 999 px only when semantically appropriate.

## 10. Elevation

Prefer borders and subtle surface contrast over heavy shadows.

Suggested elevation levels:

- Level 0: no elevation;
- Level 1: subtle shadow for floating controls;
- Level 2: dialogs/popovers;
- Level 3: exceptional overlays.

Most application surfaces should remain flat.

## 11. Icons

Use a single coherent outline icon family with consistent stroke weight. Icons must be understandable without being overly decorative.

Icon-only controls require accessible labels and tooltips where appropriate.

## 12. Buttons

### Primary

Used for the main action in a workflow, e.g. New Note, Save/confirm security action.

### Secondary

Used for normal supporting actions.

### Tertiary/ghost

Used for low-emphasis contextual actions.

### Destructive

Used for deletion/revocation/irreversible actions and requires appropriate confirmation.

## 13. Inputs

Inputs must have:

- visible focus state;
- clear label or accessible name;
- predictable error state;
- sufficient touch area;
- non-color-only validation cues.

## 14. Cards and surfaces

Cards should be used selectively. The note editor should not be placed inside an unnecessary card if doing so reduces the feeling of an open writing canvas.

## 15. Private-note visual language

Private notes should have a subtle but unmistakable security indicator.

Recommended visual language:

- lock icon;
- restrained indigo/violet accent;
- optional “Private” label;
- clear locked/unlocked state.

Avoid alarmist red styling for a normally functioning private note.

Red should indicate a security failure, not simply the existence of encryption.

## 16. Synchronization visual language

The synchronization indicator should be compact and persistent enough to build user confidence.

Suggested states:

- synchronized — subtle positive indicator;
- syncing — animated but restrained indicator;
- offline/pending — amber or neutral warning state;
- error — explicit error indicator.

## 17. Empty states

Empty states should explain what the user can do next rather than display decorative artwork by default.

Example structure:

```text
Title
Short explanation
Primary action
Optional secondary action
```

## 18. Modals and sheets

Use dialogs for focused decisions. Use sheets for mobile contextual workflows.

Security-sensitive dialogs should clearly state:

- what is being protected/unlocked;
- why authentication is required;
- what will happen after confirmation.

## 19. Toasts and transient feedback

Use transient messages for low-risk confirmations. Do not use transient feedback as the only indication of a security-critical state or failed persistence.

## 20. Loading states

Prefer skeletons for page-level content and inline progress indicators for localized operations. Avoid full-screen spinners for operations that can preserve useful existing UI.

## 21. Motion

Motion should generally be short and subtle. Respect `prefers-reduced-motion`.

Use animation for:

- navigation transitions;
- synchronization state;
- opening/closing contextual surfaces;
- meaningful status transitions.

Do not animate note content unnecessarily.

## 22. Accessibility targets

V1 should target WCAG 2.2 AA principles for contrast, keyboard navigation, focus, semantics and interaction.

## 23. Logo and brand asset area

The official Nymbus logo must be stored under:

```text
assets/brand/
├── nymbus-logo.svg
├── nymbus-logo-dark.svg
├── nymbus-mark.svg
├── nymbus-mark-dark.svg
├── nymbus-icon-512.png
└── nymbus-icon-192.png
```

These filenames define the intended asset contract; the actual artwork is intentionally not generated in this document.

### Logo placement

The logo should appear in the application shell/sidebar and onboarding/authentication surfaces where appropriate. It should not be repeated excessively inside the editor.

### Logo clear space

The logo must have sufficient breathing room and must not be visually crowded by navigation controls.

### Logo usage

Do not:

- stretch the logo;
- alter its proportions;
- add arbitrary effects;
- recolor it outside approved theme variants;
- place it on backgrounds that compromise contrast.

## 24. Icon/app mark direction

The Nymbus mark should be simple enough to remain recognizable at small PWA icon sizes. The concept should visually combine the ideas of:

- cloud/knowledge;
- protected/private content;
- lightweight information storage.

The mark should avoid literal padlock-only symbolism because Nymbus is primarily a knowledge application, not a password manager.

## 25. Visual design acceptance rules

A new component should be rejected or redesigned when it:

1. introduces a new color without semantic justification;
2. introduces arbitrary spacing values without need;
3. requires heavy decoration to communicate its purpose;
4. reduces editor/content area without meaningful benefit;
5. hides a security state;
6. violates responsive behavior;
7. creates a different interaction pattern for the same semantic action.
