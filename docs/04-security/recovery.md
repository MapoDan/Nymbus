# Nymbus — Recovery Security

**Document type:** AFU — Account and cryptographic recovery  
**Status:** V1 baseline / security gate  
**Last updated:** 2026-08-09

## 1. Purpose

Recovery exists to provide a controlled way to regain access when the normal private-note protection path is unavailable, without turning email access into a permanent copy of the user's private decryption keys.

## 2. Recovery channel

The recovery request is delivered to the Google account email configured for the Nymbus account.

The recovery mechanism must be bound to the account and the recovery transaction that initiated it.

## 3. Temporary recovery key

The system generates a high-entropy temporary recovery key.

The key must:

- be single-use;
- be account-bound;
- expire after exactly 10 minutes from issuance;
- be invalidated after successful consumption;
- be invalidated when the recovery transaction is cancelled or superseded;
- never be stored as plaintext in application logs.

## 4. Recovery flow

The functional sequence is:

1. user initiates recovery;
2. Nymbus creates a short-lived recovery transaction;
3. the temporary recovery key is delivered to the configured Google account email;
4. the user provides the key to Nymbus through the approved recovery flow;
5. the key is validated for account, transaction, expiry and single-use state;
6. the user completes the previously defined recovery step;
7. protected cryptographic material is restored/re-wrapped according to the approved key hierarchy;
8. the temporary recovery transaction is consumed.

## 5. No permanent master-key email

The email must never contain:

- the master password;
- a permanent account private key;
- a plaintext note key;
- an unprotected universal recovery secret.

## 6. Recovery and private notes

Recovery must restore an authorized path to the user's protected cryptographic hierarchy. It must not silently convert all private notes into server-decryptable content.

## 7. Brute-force protection

Recovery attempts must be rate-limited. Invalid or expired recovery keys must not reveal unnecessary information about the account or transaction.

## 8. Expiry

The 10-minute validity period is measured from server-side issuance time. Client clocks must not be trusted to determine validity.

## 9. Concurrent recovery attempts

A new recovery transaction may invalidate or supersede an earlier one according to the final recovery protocol. The implementation must avoid having multiple simultaneously valid recovery paths when the approved protocol does not require them.

## 10. Recovery audit

The system should record security-relevant recovery events such as request, successful validation, consumption and failure, while never recording the recovery key itself.

## 11. Compromised email account

Control of the configured email account is a security dependency of recovery. Users must be informed that recovery cannot provide stronger protection than the recovery channel itself.

## 12. Security gate

Before implementation, an ADR must define exactly how successful recovery obtains or reconstructs the protected key hierarchy, how existing device trust is affected, and how dedicated note passwords interact with recovery.
