# Nymbus — Navigation Specification

**Document type:** AFU — Navigation and responsive shell  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Desktop and tablet navigation

On wide screens, Nymbus uses a persistent lateral sidebar.

The sidebar provides:

- application identity/logo;
- New Note primary action;
- Search;
- primary note collections;
- folders;
- shared areas;
- settings/account entry.

The sidebar should support a compact/collapsed state where appropriate without hiding essential context from assistive technologies.

## 2. Small-screen navigation

On narrow screens where a persistent sidebar would materially reduce usable editor space, primary navigation moves to a fixed top navigation area.

The top area must remain visually stable while the user scrolls note content.

## 3. Navigation persistence

The application should remember non-security-sensitive navigation preferences such as sidebar expanded/collapsed state where this improves usability.

Security-sensitive unlock state must not be inferred from navigation state.

## 4. Back navigation

Browser back behavior must remain predictable. Opening a note is a navigational state; switching between note list and note detail must not unexpectedly discard edits.

## 5. Search entry

Search must be reachable from the primary navigation on all supported screen sizes.

On desktop, search may occupy a persistent header position if this improves discoverability. On mobile, it may be a dedicated top-level action.

## 6. Note contextual actions

Actions such as share, export, version history, move, favorite and privacy settings should be accessible from the open-note context rather than permanently occupying the main navigation.

## 7. Account navigation

The account/avatar entry should provide access to:

- account identity;
- security/passkeys;
- notifications;
- appearance;
- preferences;
- logout.

## 8. Administrator navigation

Administrative navigation is visible only when the current user has administrative privileges. Hidden UI is not a security mechanism; server-side authorization remains mandatory.

## 9. Navigation states

The active destination must be visually identifiable. Focus state and active state must remain distinct.

## 10. Mobile constraints

On narrow screens:

- do not use a permanent sidebar;
- do not place multiple large persistent toolbars above the editor;
- keep title and primary note context accessible;
- keep formatting controls available without permanently consuming excessive vertical space;
- use bottom sheets/dialogs for secondary workflows where appropriate.
