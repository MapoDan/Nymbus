# Nymbus — User Personas

**Document type:** AFU — User/persona analysis  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

Personas describe representative needs and constraints. They are not intended to encode personal data or demographic assumptions that do not affect product behavior.

## Persona P-01 — Private knowledge user

### Profile

A user who wants a lightweight personal knowledge base but occasionally stores sensitive information such as credentials-related notes, financial information, private project material, personal documents, or other confidential text.

### Primary goals

- Capture information quickly.
- Find information later through search, folders and tags.
- Avoid maintaining a complex workspace system.
- Protect selected notes without encrypting the entire application indiscriminately.
- Unlock protected information quickly on trusted devices.

### Pain points

- Complex note applications can become cumbersome.
- Traditional cloud note systems require trusting the provider with plaintext.
- Passwords for every protected note can become impractical.
- Offline editing often behaves unpredictably.

### Nymbus needs

- Minimal editor.
- Strong search over allowed metadata.
- E2E private notes.
- First password initialization followed by convenient platform-authenticator unlock.
- Bulk unlock.
- Offline-first behavior.
- Clear lock/sync states.

### Success signal

The user can capture and retrieve information without thinking about the infrastructure, while sensitive notes remain protected.

---

## Persona P-02 — Multi-device user

### Profile

A user who moves between phone, tablet, laptop and desktop browser sessions.

### Primary goals

- Continue writing on any device.
- Authenticate quickly.
- See current synchronization state.
- Avoid manual file transfers.

### Pain points

- Repeated passwords.
- Conflicts between devices.
- Interfaces that work well only on desktop.

### Nymbus needs

- Google authentication as the account identity provider.
- Passkeys for subsequent authentication.
- Responsive PWA.
- Offline local persistence.
- Reliable synchronization.
- Clear device/passkey management.

### Success signal

Switching devices feels natural and the user understands whether their work is synchronized.

---

## Persona P-03 — Collaborative user

### Profile

A user who shares selected notes or folders with other Nymbus users.

### Primary goals

- Share only the information required.
- Choose read or edit access.
- Collaborate without losing changes.
- Revoke access when collaboration ends.
- Preserve E2E protection for sensitive shared notes.

### Pain points

- Coarse permissions.
- Collaboration systems that expose all content to the server.
- Ambiguous revocation behavior.
- Conflicts during simultaneous editing.

### Nymbus needs

- Individual-user sharing.
- Folder permission inheritance.
- Read/edit permissions.
- Strong private-note revocation.
- CRDT-based synchronization.
- Real-time collaboration only when active.

### Success signal

Users can collaborate with predictable permissions and no silent loss of concurrent edits.

---

## Persona P-04 — NAS operator / owner

### Profile

A technically capable person running Nymbus on self-hosted NAS infrastructure with limited CPU/RAM compared with cloud infrastructure.

### Primary goals

- Deploy Nymbus predictably.
- Keep resource consumption low.
- Monitor service health.
- Back up persistent data using NAS tooling.
- Upgrade without unnecessary operational complexity.

### Pain points

- Applications with many containers.
- Services that consume RAM while idle.
- Heavy databases and search engines.
- Hidden background jobs.
- Difficult recovery procedures.

### Nymbus needs

- Small number of meaningful services.
- Low idle consumption.
- Bounded background work.
- Explicit persistent-volume requirements.
- Health/operational observability.
- Clear deployment and recovery documentation.

### Success signal

Nymbus runs continuously on the NAS without becoming a significant resource burden or operational project.

---

## Persona P-05 — Administrator

### Profile

A user with explicit administrative permissions responsible for account, device, security and operational management.

### Primary goals

- Manage user/device lifecycle where authorized.
- Investigate operational problems.
- Enforce permissions.
- Maintain security without accessing private-note plaintext.

### Pain points

- Admin interfaces that expose too much data.
- Binary all-or-nothing permissions.
- Poor auditability.

### Nymbus needs

- Granular permissions.
- Server-side authorization.
- Safe operational logs.
- Device/passkey management.
- Security event visibility.
- Explicit separation between administrative authority and private-note decryption.

### Success signal

The administrator can manage the system without receiving cryptographic powers that the product does not require.

---

## Persona implications

These personas create several product-level constraints:

1. The editor must remain simple even though the underlying security model is sophisticated.
2. Private-note UX must distinguish account authentication from content unlock.
3. Search must remain useful while private notes are locked.
4. Offline behavior cannot be treated as an edge case.
5. Collaboration must not force a permanently active real-time infrastructure layer.
6. Administrative functions must not undermine E2E privacy.
7. NAS efficiency is a product requirement, not merely an infrastructure optimization.
