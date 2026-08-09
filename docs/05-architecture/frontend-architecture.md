# Nymbus — Frontend Architecture

## 1. Application model

Nymbus is a PWA. The frontend is responsible for UI rendering, local application state, offline persistence, synchronization orchestration and client-side private-note cryptography.

## 2. Logical layers

1. presentation/UI;
2. interaction/application state;
3. domain models;
4. synchronization/offline engine;
5. cryptography/key-access layer;
6. persistence adapter;
7. network/API adapter.

Dependencies must flow toward stable domain contracts rather than from domain logic directly into UI or transport details.

## 3. Local state

The frontend maintains a local database for offline work, synchronization queues and encrypted content. Plaintext private content should be transient and retained only as long as necessary.

## 4. Crypto isolation

Cryptographic operations must be centralized behind a dedicated client-side abstraction. UI components must never implement encryption directly.

## 5. Reactivity

UI state should update from local state first. Network synchronization updates local state asynchronously and the UI observes the resulting state transition.

## 6. Performance

The frontend must minimize memory copies, large reactive trees and unnecessary re-rendering. Large attachments and documents should be processed incrementally where practical.

## 7. PWA responsibilities

Service Worker functionality is limited to application shell/network strategy and offline support. It is not treated as a key vault.

## 8. Browser support

Features depending on WebAuthn, IndexedDB, Web Crypto or other browser capabilities must have explicit capability detection and graceful fallback behavior.
