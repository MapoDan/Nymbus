# Nymbus — Markdown Document Model

**Document type:** AFU — Canonical editor/document representation  
**Status:** V1 baseline / implementation ADR required  
**Last updated:** 2026-08-09

## 1. Canonical representation

Nymbus should use a structured document model as the canonical editor representation and provide deterministic Markdown serialization for export/interchange.

## 2. Block nodes

The model must represent at least:

- paragraph;
- heading;
- unordered list;
- ordered list;
- checklist;
- block quote;
- code block;
- table;
- horizontal rule.

## 3. Inline nodes/marks

The model must represent at least:

- plain text;
- bold;
- italic;
- strikethrough;
- inline code;
- link;
- image reference where appropriate.

## 4. Stable identity

Collaboratively editable structural elements should have stable identities where required by the synchronization layer. The model must not depend on character offsets alone for long-lived references.

## 5. Markdown serialization

Serialization must be deterministic so equivalent document states produce predictable Markdown output.

## 6. Markdown parsing

Any Markdown parser must operate under a strict supported subset and produce a safe structured representation. Raw HTML must not become an arbitrary executable surface.

## 7. Tables

Tables are structured nodes rather than opaque Markdown strings. Cells contain supported inline/block content according to the final editor rules.

## 8. Images

Images are represented by attachment IDs/references, not embedded binary data inside the document state.

## 9. Checklists

Checklist items include a boolean checked state independent of their displayed text.

## 10. Links

Links contain normalized target URLs and optional display text. Unsafe schemes must be rejected or neutralized.

## 11. Code blocks

Code blocks preserve whitespace and must not interpret contained markup.

## 12. Unsupported syntax

When imported/pasted content contains unsupported constructs, the parser should preserve safe textual meaning where possible and clearly avoid silent execution or data corruption.

## 13. Compatibility

The model should permit future extension without changing the semantics of existing node types.

## 14. CRDT compatibility

The selected CRDT must operate on this structured representation or an equivalent deterministic representation. The architecture must avoid requiring a full-document destructive replacement for every character edit.

## 15. Private content

The complete private document model remains inside the client trust boundary while unlocked. Its serialized encrypted representation is what crosses the backend boundary.

## 16. Versioning

A user-visible version captures a valid document state. Internal synchronization operations remain distinct from user-visible version records.
