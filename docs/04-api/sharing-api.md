# Nymbus — Sharing API Contract

**Document type:** AFU — Sharing and permissions API  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Scope

Sharing changes who may access a note/folder. It is therefore both an authorization operation and, for private notes, a cryptographic key-distribution operation.

## 2. Grant access

A permitted owner/editor can grant a resource to another authenticated Nymbus user.

The operation must specify:

- target user;
- resource;
- role;
- optional capabilities;
- effective time if supported.

## 3. Private-note sharing

For private notes, the API coordinates protected note-key access for the recipient. It must not receive or store a plaintext note key.

## 4. Change role

Changing a collaborator's role must update authorization and, where necessary, cryptographic key-access state.

## 5. Revoke access

Revocation immediately prevents future authorized server operations and removes active collaboration membership where possible.

The cryptographic architecture determines whether re-keying is required for future content and/or historical versions.

## 6. Existing copies

The API must not claim that revocation can erase content already copied by the recipient.

## 7. Permission listing

Authorized users may list current sharing state. Private cryptographic material must not be exposed in this response.

## 8. Invitations

If V1 uses direct user-to-user sharing, target resolution must be based on authenticated Nymbus identity. Unauthenticated email invitations are outside the primary V1 trust model unless separately approved.

## 9. Authorization checks

The server must verify the actor's ability to share the resource before any cryptographic key distribution record is created.

## 10. Audit

Grant, role-change and revoke operations should create audit records containing actor, target, resource, action and timestamp, but never note plaintext or key material.
