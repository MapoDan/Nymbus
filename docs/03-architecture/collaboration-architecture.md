# Nymbus — Collaboration Architecture

**Document type:** AFU — Collaboration, channels and threads  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Scope

Nymbus supports collaborative content according to the previously selected product model. Channels and threads are configurable at account level and can be enabled or disabled by the user.

## 2. Collaboration modes

The architecture must support:

- private individual notes;
- shared notes;
- collaborative editing where permissions allow;
- channels/threads when enabled;
- discussion/context associated with collaborative content where applicable.

## 3. Channels

Channels provide an organizational/conversational context for collaborative activity.

A disabled channel/thread feature must not consume unnecessary backend resources or clutter the primary navigation.

## 4. Threads

Threads are subordinate conversations associated with supported collaborative content.

Thread data must have explicit ownership, authorization and lifecycle rules.

## 5. Real-time collaboration

Realtime communication is an optimization for active collaboration, not the authoritative data store.

The persistent synchronized document state remains authoritative.

## 6. Editing model

Collaborative editing must use the selected mergeable document model defined by the synchronization architecture. The final CRDT/operation model requires an ADR.

## 7. Presence

Optional presence indicators may show active collaborators. Presence is ephemeral and should not be persisted as ordinary note content unless a future requirement explicitly needs history.

## 8. Permission requirements

Only users with edit permission can generate content-edit operations.

Read-only users may receive relevant updates but must not be able to submit unauthorized modifications.

## 9. Private collaborative notes

Private shared notes remain E2E encrypted.

Collaboration transport must not require the server to decrypt note plaintext.

The cryptographic design must support multiple authorized key holders.

## 10. User removal

Removing a collaborator must:

- revoke server authorization;
- terminate active collaboration membership where possible;
- prevent future synchronization operations;
- trigger cryptographic re-keying when required by the private-note security policy.

## 11. Concurrent edits

Concurrent edits should merge deterministically according to the selected document CRDT/operation model.

The user should not be shown low-level operation conflicts unless automatic reconciliation genuinely cannot produce a safe result.

## 12. Threads and private content

If threads contain private-note-derived information, they must follow the same confidentiality classification as the associated private resource.

A thread must not accidentally become a plaintext side channel around E2E encryption.

## 13. Notifications

Collaboration notifications should be represented as lightweight persistent events. Realtime delivery may accelerate presentation but is not the only delivery mechanism.

## 14. Resource efficiency

Idle channels should not require permanently active server processes per channel.

Realtime subscriptions should be multiplexed where practical.

## 15. Feature toggles

The account setting enabling/disabling channels/threads affects UI and relevant subscription behavior. It must not corrupt or delete existing channel/thread data merely because the feature is disabled.

## 16. Auditability

Permission changes, invitations and removals may produce audit events. Message/note content must not be copied into audit logs.
