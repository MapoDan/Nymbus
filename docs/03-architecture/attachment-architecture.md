# Nymbus — Attachment Architecture

**Document type:** AFU — Images and inline attachments  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Scope

V1 supports inline images inside Markdown notes. The user chooses the processing/quality level at upload time because some images require high detail while others do not.

## 2. User flow

```text
Insert image
   ↓
Select image
   ↓
Choose quality/processing level
   ↓
Client validates size/type
   ↓
Optional client processing
   ↓
Upload/store
   ↓
Insert image reference into note
```

## 3. Quality options

The UX must provide understandable quality choices rather than technical compression terminology alone.

The final product specification should define at least a high-detail option and one or more optimized options, including expected resolution/size behavior.

The original image must not be silently destroyed unless the user explicitly chooses a destructive optimization.

## 4. Image validation

The application must validate:

- supported MIME/type;
- file size;
- image dimensions;
- decoding safety;
- upload limits.

The backend must repeat security-sensitive validation and must not trust browser-provided MIME information.

## 5. Normal note images

For non-private notes, images can be stored as ordinary application assets subject to authorization.

The note stores a stable asset reference rather than embedding large binary data directly into the document representation.

## 6. Private note images

Private-note images are part of the note's confidential content.

They must be encrypted before persistent server storage and must remain inaccessible without the appropriate note cryptographic authorization.

## 7. Thumbnail/preview generation

Any thumbnail or preview generated for a private image must not accidentally create an unencrypted plaintext copy on the NAS.

If server-side image processing would require plaintext, the preferred V1 approach is to process privately on the client before encryption where feasible.

If server-side processing is later required, it needs a dedicated security ADR because it changes the E2E trust boundary.

## 8. Image references

Image references inside a note must remain stable across synchronization and version history.

The reference must not expose storage filesystem paths.

## 9. Deletion

Removing an image from a note does not necessarily mean immediate physical deletion because older versions may still reference it.

Asset garbage collection must consider:

- current note;
- retained 15 versions;
- pending offline operations;
- shared access;
- synchronization state.

## 10. Access control

Asset access must be authorized through the associated note/resource permissions. A user must not be able to obtain an asset by guessing its identifier.

## 11. Download behavior

The client should request only assets required for the current user context. Large images should not be eagerly downloaded for every note in a list.

## 12. Lazy loading

Inline images outside the immediate viewport should be lazily loaded where practical.

This is especially important on mobile devices and low-bandwidth connections.

## 13. Offline images

Images already cached locally may be displayed offline if the user is authorized and, for private notes, has the required local cryptographic access.

## 14. Export

Markdown export must define how image references are represented and whether the export is self-contained or references external assets.

PDF export must render images locally or through a process that respects the private-note trust boundary.

## 15. EXIF and metadata

Image metadata may contain sensitive information such as location or device information. The product must define whether EXIF metadata is preserved, stripped or optionally controlled by the user.

A secure default should avoid unnecessarily propagating sensitive EXIF data.

## 16. Resource constraints

Image processing must be bounded because high-resolution images can create CPU and memory spikes on the NAS.

Client-side processing is preferred for transformations that do not require server trust.

## 17. Upload failure

Interrupted uploads must not leave the note referencing a non-existent asset.

The client must be able to retry safely without creating duplicate logical attachments.
