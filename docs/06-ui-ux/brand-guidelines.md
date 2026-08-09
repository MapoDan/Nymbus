# Nymbus — Brand Guidelines

**Document type:** AFU — Brand and visual identity  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Brand concept

Nymbus combines the ideas of cloud-like access, personal knowledge and privacy. The visual identity should suggest a protected personal space without relying on literal padlock imagery everywhere.

The identity should be recognizable even when reduced to an app icon.

## 2. Logo placeholder

The definitive Nymbus logo is intentionally left as an asset slot until the final logo artwork is approved.

Recommended repository location:

```text
assets/
└── branding/
    ├── logo-primary.svg
    ├── logo-mark.svg
    ├── logo-monochrome.svg
    ├── logo-white.svg
    ├── favicon.svg
    └── app-icon.png
```

Do not create fake logo assets in code. Until the artwork exists, UI documentation must use a clearly marked placeholder.

## 3. Logo concept direction

The mark should combine a lightweight cloud/nimbus reference with an abstract protected-document or knowledge-space concept.

Avoid:

- literal padlocks as the sole concept;
- generic cloud-storage icons;
- overly complex shields;
- gradients that disappear at small sizes;
- excessive detail.

## 4. Primary color

The principal brand color is Nymbus Indigo `#6366F1`.

Its darker variant `#4F46E5` is used for stronger emphasis and interaction states.

The soft variant `#EEF2FF` is used for subtle backgrounds and selected states.

## 5. Logo color usage

Preferred:

- Indigo mark on light backgrounds;
- white mark on dark/indigo backgrounds;
- monochrome mark where color reproduction is unavailable.

Do not recolor the mark arbitrarily with semantic success/warning/danger colors.

## 6. Clear space

Maintain a clear space around the logo approximately equal to the height of the mark's primary internal unit. Exact minimum clear-space rules will be defined when final artwork is approved.

## 7. Minimum size

The mark must remain recognizable at favicon/app-icon scale. Fine details that cannot survive small rendering should not be part of the primary mark.

## 8. Iconography

Application icons should use the same visual language as the brand mark: simple geometry, restrained stroke weight and high legibility.

## 9. Illustration style

Illustrations, if introduced later, should be minimal and abstract rather than cartoon-heavy. V1 does not require decorative illustrations for ordinary empty states.

## 10. Typography

Brand typography should remain compatible with the UI system. Avoid adding a heavyweight custom font solely for branding if it meaningfully increases application payload.

## 11. Tone of voice

Product copy should be:

- concise;
- clear;
- calm;
- technically honest;
- non-alarmist;
- user-oriented.

Security copy should explain what happened and what the user can do next.

## 12. Example product language

Prefer:

> "Note bloccata. Sblocca per visualizzare il contenuto."

over:

> "ACCESSO NEGATO — CONTENUTO PROTETTO"

The application should feel secure without making ordinary security behavior intimidating.

## 13. Brand misuse

Do not use the Nymbus mark to imply third-party certification, regulatory approval or security guarantees that the product does not actually possess.
