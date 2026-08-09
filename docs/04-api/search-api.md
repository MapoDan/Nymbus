# Nymbus — Search API Contract

**Document type:** AFU — Search API  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Scope

The server search API handles only metadata that is intentionally server-readable.

## 2. Searchable fields

At minimum:

- title;
- tags;
- folder;
- favorite state where useful;
- explicitly approved metadata.

## 3. Private body exclusion

The API must never accept a request meaning "search all private note bodies on the server" because plaintext bodies are not available to the backend under the E2E model.

## 4. Locked-note behavior

Private notes can appear in results based on searchable metadata even when their content is locked.

## 5. Client-side private search

Full-text private search is a client operation over locally available decrypted content/indexes.

## 6. Query privacy

Search terms must not be logged or sent to analytics unnecessarily.

## 7. Pagination

Search responses are bounded and use deterministic ordering/cursor pagination where appropriate.

## 8. Filtering

The API supports explicit filters such as tag, folder and favorite according to the approved UI specification.

## 9. Authorization

Search results must be filtered by effective authorization before being returned.

## 10. Performance

Search must rely on appropriate relational indexes in V1 rather than a dedicated search cluster.
