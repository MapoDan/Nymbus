# Nymbus — Realtime Protocol Contract

**Document type:** AFU — Realtime collaboration and event transport  
**Status:** V1 baseline / implementation ADR required  
**Last updated:** 2026-08-09

## 1. Purpose

Realtime transport accelerates collaboration and event delivery. It is not the authoritative persistence mechanism.

## 2. Use cases

Realtime may deliver:

- document changes;
- collaboration presence;
- sharing events;
- synchronization hints;
- notification hints;
- security/permission changes.

## 3. Connection lifecycle

The client connects only when needed and reconnects with bounded backoff.

The server must not require one dedicated heavyweight process per connected channel/user.

## 4. Authentication

Realtime connections require authenticated session context and must be associated with the authenticated user.

## 5. Authorization

Subscription authorization must be checked when a subscription is created and revalidated when security-sensitive changes occur.

## 6. Private content

For private notes, realtime messages carry encrypted operations/payloads as defined by the E2E synchronization model. The realtime service must not decrypt note content.

## 7. Ordering

The transport does not define document causality. Causal ordering comes from the synchronization/CRDT protocol.

## 8. Reconnect

A disconnected client must not assume it received every event. After reconnect it performs normal synchronization from its last durable checkpoint.

## 9. Presence

Presence is ephemeral and should expire automatically when the connection disappears.

## 10. Resource efficiency

The implementation should:

- multiplex connections where practical;
- avoid busy loops;
- use heartbeat intervals appropriate to the environment;
- remove stale subscriptions;
- avoid broadcasting events to users who cannot access the resource.

## 11. Fallback

If realtime transport is unavailable, the application remains functional through normal synchronization/polling mechanisms within the defined limits.

## 12. Security events

Permission revocation and device/session security events should be propagated with sufficient priority to prevent the client from continuing unauthorized operations.
