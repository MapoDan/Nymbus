# Nymbus — Editor Rendering and Content Security

**Document type:** AFU — Rendering/XSS security specification  
**Status:** V1 mandatory  
**Last updated:** 2026-08-09

## 1. Principle

Note content is untrusted input regardless of whether it originated from the current user, another collaborator, a paste operation or an imported/rendered document.

## 2. Raw HTML

Raw HTML inside Markdown must not be treated as trusted executable markup by default.

## 3. Script execution

JavaScript, event-handler attributes and executable browser payloads must never be allowed through note content.

## 4. Links

Allowed URL schemes must be explicitly allow-listed. Dangerous schemes such as script-like executable schemes must be rejected or rendered as inert text.

## 5. Images

Image sources must resolve through controlled attachment references or explicitly approved safe external sources if external images are supported. External image loading should be treated as a privacy consideration because remote servers can observe requests.

## 6. Iframes/embed

Arbitrary iframe/embed/object content is outside the V1 note model unless separately approved with a security design.

## 7. SVG

SVG content must be treated carefully because SVG can contain active content. Uploaded SVG should be rejected or sanitized into a safe representation before use.

## 8. Clipboard HTML

Clipboard HTML is untrusted and must be sanitized before conversion to the document model.

## 9. Markdown rendering

Markdown parsing and HTML rendering must use a maintained, well-reviewed parser/sanitizer rather than a custom ad-hoc Markdown-to-HTML implementation.

## 10. CSP

The deployed PWA should use an appropriate restrictive Content Security Policy. The exact policy belongs to deployment/security documentation and must be compatible with the chosen authentication and asset strategy.

## 11. Private notes

Decrypting private content creates plaintext in browser memory. The application should minimize its lifetime and avoid unnecessary copies.

## 12. Search snippets

Private plaintext must not be placed into server-side search indexes, analytics events, HTTP caches or ordinary server logs.

## 13. Export

Exported HTML/Markdown/PDF is an output boundary and must not be assumed to inherit Nymbus's application security controls.

## 14. Threat model

Security review must explicitly consider:

- malicious shared note content;
- malicious Markdown;
- malicious images;
- malicious clipboard data;
- compromised external image hosts;
- browser XSS;
- DOM clobbering;
- unsafe URL schemes;
- oversized parser inputs;
- decompression/image bombs;
- resource exhaustion.
