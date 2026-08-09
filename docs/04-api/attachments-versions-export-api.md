# Nymbus — Attachments, Versions and Export API Contract

**Document type:** AFU — Content support APIs  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Attachments

The attachment API manages image/blob lifecycle independently from note metadata while maintaining authorization through the owning note/resource.

## 2. Upload

The client may request an upload context and then transfer the binary asset according to the approved storage strategy.

The server validates size/type and authorization even if client-side validation already occurred.

## 3. Private attachment upload

Private images are encrypted before persistent storage according to the E2E architecture.

## 4. Attachment references

A note references attachments using stable opaque IDs. Filesystem paths must never be exposed to the client.

## 5. Attachment deletion

Deletion marks an attachment for removal. Physical cleanup occurs only after checking retained versions, pending synchronization and other references.

## 6. Versions

The versions API supports:

- list retained versions;
- retrieve an authorized version;
- restore a version where permitted;
- export a version where permitted.

V1 retains exactly 15 user-visible versions per note.

## 7. Private versions

Private version payloads remain encrypted. The client decrypts them after local authorization.

## 8. Export

V1 export supports two user-selectable formats:

- Markdown;
- PDF.

The selected format is part of the user action, not a global application constraint.

## 9. Private export

Private export requires local decryption. The backend must not decrypt private content solely to produce an export.

## 10. Export security

The UX should warn users that an exported file may no longer be protected by Nymbus E2E encryption once it leaves the application.

## 11. Markdown export

Markdown export must define how inline images are packaged/referenced so that a self-contained export does not produce broken references.

## 12. PDF export

PDF rendering must preserve the note's supported formatting and inline images. For private notes, rendering must occur on an authorized client or through a separately approved trusted processing model.

## 13. Export authorization

Read permission and export permission must follow the final authorization matrix. The server must enforce any server-side authorization portion, while private export additionally requires local decryption capability.
