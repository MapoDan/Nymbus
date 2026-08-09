# Nymbus — Device Management Security

**Document type:** AFU — Trusted-device lifecycle  
**Status:** V1 baseline / security gate  
**Last updated:** 2026-08-09

## 1. Device model

A device is a client environment capable of holding an authenticated Nymbus session and, when explicitly authorized, protected local cryptographic access material.

A device must not be considered trusted merely because it successfully logged in with Google.

## 2. Device registration

A newly authenticated device starts with account/session access only. Access to the private-note key hierarchy must be explicitly provisioned according to the approved multi-device protocol.

## 3. Device identity

Device records must use a server-recognizable identifier that does not contain private cryptographic material.

The server must be able to distinguish active, revoked and retired devices.

## 4. Trusted-device state

The UI and backend must distinguish:

- authenticated device;
- cryptographically provisioned device;
- revoked device;
- retired/removed device.

These states must not be conflated.

## 5. Provisioning a second device

Provisioning must require explicit authorization from an already trusted context or the approved recovery protocol.

The backend must not automatically release the account cryptographic root to every newly authenticated device.

## 6. Device revocation

A user must be able to revoke a device/passkey from account security settings.

Revocation must immediately prevent the revoked credential/device from obtaining new authorized server operations.

## 7. Cryptographic effect of revocation

Revoking a device does not guarantee destruction of plaintext or keys already present in that device's memory/storage. Where the device participated in note-key access, the approved key lifecycle must determine whether affected notes require re-keying.

## 8. Lost device

A lost device must be revocable remotely. The user must not need the lost device to complete revocation.

## 9. Device replacement

Replacing a device must follow the same explicit provisioning rules as adding a new device. Device replacement must not silently weaken private-note protection.

## 10. Multiple devices

A user may have multiple trusted devices. Each device must have independently manageable authorization state.

One compromised/revoked device must not require deleting the entire account unless the approved cryptographic recovery protocol determines that this is necessary.

## 11. Offline device

A device that becomes offline may continue using locally authorized encrypted data according to the offline policy, but it cannot use offline state to create new server authorization after revocation.

## 12. Device cleanup

When a user explicitly removes a device, the client should clear locally held application data and protected key-access material where the platform permits. This is best-effort and must not be presented as guaranteed forensic erasure.

## 13. Security gate

The exact multi-device key provisioning and device-to-key binding protocol requires an explicit cryptographic ADR before implementation.
