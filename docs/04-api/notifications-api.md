# Nymbus — Notifications API Contract

**Document type:** AFU — Notifications API  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Purpose

Notifications provide a persistent, lightweight representation of events requiring user attention.

## 2. Notification examples

The notification center may include:

- synchronization failures;
- sharing invitations/events;
- permission changes;
- security events;
- recovery/security warnings;
- other actionable application events.

## 3. Persistence

Notifications that represent actionable state may be persisted until resolved/read according to the product policy.

Transient realtime presence events should not become persistent notifications by default.

## 4. API operations

The API supports conceptually:

- list notifications;
- retrieve notification details;
- mark read;
- mark resolved where applicable;
- clear eligible notifications.

## 5. User isolation

A user can retrieve only their own notifications.

## 6. Content security

Notifications must not contain private note plaintext or sensitive cryptographic material.

A notification may identify a note by safe metadata such as title/ID only where the user is authorized to see that metadata.

## 7. Synchronization

Notification state may synchronize across a user's devices where useful, but ephemeral UI state should remain local when possible to reduce backend load.

## 8. Resource constraints

Notifications must use ordinary relational persistence and should not require a separate messaging infrastructure for V1.
