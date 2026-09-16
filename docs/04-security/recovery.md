# Nymbus — Recovery Security

**Document type:** AFU — Account and cryptographic recovery  
**Status:** V1 baseline / security gate  
**Last updated:** 2026-09-16

## 1. Purpose

The initial private-note cryptographic activation is initiated by the Nymbus administrator for the relevant user. The purpose is to allow the user to establish their own master password without exposing that password to the administrator or backend.

## 2. Administrator-initiated activation

For a newly activated user:

1. administrator initiates the activation/recovery process for the specific Nymbus user;
2. Nymbus creates a one-time activation transaction;
3. the user completes the approved authentication/activation flow;
4. the user chooses and establishes their master password locally;
5. the master password is processed locally and is never sent to the administrator or Nymbus backend;
6. the user's root/key-encryption material is initialized/protected according to the cryptographic key-management design;
7. the activation transaction is consumed.

The administrator must never be given the master password or plaintext private-note keys.

## 3. Temporary activation/recovery mechanism

If the activation protocol uses a short-lived activation code or token, it must be:

- high entropy;
- single-use;
- account-bound;
- bound to the activation transaction;
- server-expiring;
- rate-limited;
- absent from application logs in plaintext.

The exact transport and lifetime are security-ADR decisions. It must not be a permanent universal decryption secret.

## 4. Later recovery

V1 does not define a separate per-note recovery password or temporary note password.

If later account recovery is implemented, it must restore/re-establish the user's master-password-protected key path without sending the master password or a permanent universal decryption key by email.

Any later email recovery mechanism must be explicitly specified and security-reviewed before implementation.

## 5. Master password

The master password is the sole password-based secret for private-note protection in V1.

It must:

- be entered only through the approved client flow;
- never be transmitted to Nymbus backend;
- never be stored in plaintext;
- never be logged;
- be processed using the approved memory-hard password KDF.

## 6. Recovery and private notes

Recovery must restore an authorized path to the user's protected cryptographic hierarchy. It must not silently convert all private notes into server-decryptable content.

Shared notes remain governed by the user's authorization state. Recovering one user's master-password key hierarchy does not automatically grant access to another user's shared notes.

## 7. Brute-force protection

Activation and any later recovery attempts must be rate-limited. Invalid or expired activation/recovery credentials must not reveal unnecessary information about the account or transaction.

## 8. Recovery audit

The system should record security-relevant activation/recovery events such as request, successful validation, consumption and failure, while never recording passwords, activation secrets or private keys.

## 9. Compromised recovery channel

Control of any recovery/activation channel is a security dependency. The system must not claim that recovery provides stronger protection than the channel used to authorize it.

## 10. Security gate

Before implementation, an ADR must define exactly how administrator-initiated activation establishes the user's master-password-protected key hierarchy, how subsequent device enrollment works, and whether/how later recovery is supported.
