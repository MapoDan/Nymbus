# Nymbus — Search Architecture

**Document type:** AFU — Search and indexing architecture  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Objective

Search must remain fast, simple and lightweight while respecting the E2E boundary of private notes.

## 2. Two search domains

Nymbus has two fundamentally different search domains:

1. server-side metadata search;
2. client-side private-content search.

They must not be conflated.

## 3. Server-side metadata search

The server may index/search approved metadata such as:

- note title;
- user-defined tags;
- folder;
- favorite state;
- other explicitly classified non-secret metadata.

This search works while private notes are locked.

## 4. Private-content search

The backend must not receive a plaintext index of private note bodies.

After authorized local unlock, the client may search:

- decrypted note body;
- private inline content where applicable;
- local private metadata that is not server-readable.

## 5. Combined results

The UI may combine server metadata results with local private-content results.

Results must clearly indicate when a private result requires unlocking to open.

## 6. Locked private notes

A locked private note can appear in search because its title/tags/other approved metadata remain searchable.

The search result must not leak body content, snippets or content-derived information that would defeat the private-note boundary.

## 7. Search index storage

The server-side metadata index should use the primary relational database's capabilities unless an ADR proves that a dedicated search engine is necessary.

A dedicated search engine is explicitly disfavored for V1 because of NAS resource constraints.

## 8. Local private index

The local private search index is sensitive data.

It must be protected by the local cryptographic lifecycle and must become inaccessible when the private-note unlock session expires.

## 9. Index invalidation

When a private note changes, the local index must eventually reflect the latest decrypted state.

When a note is locked or its key access is revoked, the client must invalidate or render inaccessible the relevant private index entries.

## 10. Search performance

Search should provide responsive local results for normal note collections without requiring continuous backend calls.

For server metadata search, queries should be bounded and indexed appropriately.

## 11. Search privacy

Search terms entered by the user may themselves reveal sensitive information. The application should avoid unnecessary telemetry or logging of search queries.

Private-content queries must not be transmitted to the backend merely to improve analytics.

## 12. Sorting and filtering

Search results should support the product-defined metadata filters such as tags, folders and favorite state.

Private-content search may additionally filter locally available unlocked notes.

## 13. Index rebuild

The client must be able to rebuild its local private index from locally available encrypted content after corruption or version changes.

The rebuild must not require sending plaintext to the server.

## 14. Empty/error states

The UI should distinguish:

- no matching notes;
- private notes exist but are locked;
- local private index unavailable;
- server search unavailable;
- synchronization pending.

## 15. Resource constraints

No separate Elasticsearch/OpenSearch service is required for V1.

If future scale makes the relational metadata search insufficient, the migration must be justified with real usage metrics rather than anticipated scale alone.
