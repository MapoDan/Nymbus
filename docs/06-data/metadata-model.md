# Nymbus — Metadata Model

**Status:** V1 baseline

## 1. Purpose

Metadata enables navigation, filtering and discovery without decrypting private note content.

## 2. Private-note metadata

The following are explicitly approved as server-readable unless a later privacy decision changes the model:

- title;
- tags;
- folder association;
- favorite state;
- creation/update timestamps;
- deletion/trash state;
- synchronization state;
- ownership and sharing metadata;
- non-secret cryptographic/version references.

## 3. Searchability

The server may index approved metadata. Therefore a locked private note remains discoverable by title and tags.

## 4. Prohibited metadata

The server must not persist or index the following for private notes:

- body text;
- plaintext excerpts;
- plaintext Markdown;
- OCR text extracted from private images;
- plaintext attachment names when classified as private content;
- semantic embeddings derived from private plaintext.

## 5. Metadata privacy trade-off

Users must understand that private-note encryption does not hide all metadata. A storage/backend administrator may observe approved metadata and operational information.

## 6. Derived metadata

Any new derived field must be explicitly classified before implementation. A derived value is not automatically safe because it is not the original content.

## 7. Synchronization metadata

Synchronization metadata may include operation IDs, versions, cursors and status. It must not contain private plaintext payloads.

## 8. Consistency

Metadata changes must follow documented conflict policies and must not accidentally overwrite unrelated encrypted content state.
