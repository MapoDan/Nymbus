# Nymbus — Attachments

**Status:** V1 baseline

## 1. Scope

Attachments include images and other files associated with notes. V1 supports user-controlled image quality/detail at upload time.

## 2. Image quality choice

When uploading an image, the user may choose the desired processing/detail level because different images have different requirements. Nymbus must not force one universal compression policy.

## 3. Private attachments

Attachments belonging to private notes are encrypted before server persistence. The server stores ciphertext and non-secret storage metadata only.

## 4. Public/normal note attachments

Attachments for non-private notes may follow the normal application storage model, subject to access control and retention rules.

## 5. Metadata

Storage metadata may include size, MIME type, dimensions where approved, checksum/integrity data, note ID and timestamps. Private filenames must be classified according to the note's privacy boundary before being stored server-side.

## 6. Deduplication

Server-side content deduplication must not require decrypting private attachments. Any deduplication strategy must preserve the E2E security boundary.

## 7. Upload lifecycle

The client validates and prepares the file, encrypts it when required, and uploads the resulting representation. Interrupted uploads must be resumable or safely retryable without creating duplicate logical attachments.

## 8. Download lifecycle

An authorized client downloads ciphertext, verifies integrity, decrypts locally and presents the result.

## 9. Version association

An attachment may be associated with a note version/content revision where required. Historical references must remain resolvable for retained versions.

## 10. Limits

The implementation must define maximum attachment size, accepted MIME types, image dimensions and upload batch sizes before API implementation. Limits must be compatible with low-capacity NAS operation.

## 11. Deletion

Deleting an attachment must update its logical state first and physically remove storage only according to the retention policy.
