# Nymbus — API Conventions

**Document type:** AFU — API standards  
**Status:** V1 baseline  
**Last updated:** 2026-08-09

## 1. Naming

Resource names must be stable, predictable and technology-neutral. Public API names must not expose internal database table or microservice names.

## 2. Authentication

Protected requests require a valid authenticated application session. Authentication credentials must be handled according to the authentication architecture.

## 3. Authorization

Authentication alone is insufficient. Every resource mutation/read that is protected must evaluate effective authorization for the requesting user.

## 4. Content types

JSON is the default structured API representation. Binary asset transfer uses the explicitly documented upload/download contract.

## 5. Timestamps

API timestamps must use an unambiguous standard representation and UTC semantics. Clients must not be expected to interpret server-local time.

## 6. IDs

Resource IDs are opaque strings/UUID-like identifiers. Clients must treat them as opaque.

## 7. Validation

The server validates every externally supplied field even if the client already validates it.

Validation errors identify the relevant field without exposing internal implementation details.

## 8. Error envelope

Errors use a consistent structure conceptually containing:

- machine-readable error code;
- human-readable safe message;
- optional field errors;
- correlation/request identifier.

Internal stack traces, SQL errors, encryption keys and infrastructure details must never be returned.

## 9. Authorization failure

The API must avoid unnecessarily revealing whether a resource exists when the caller is not authorized to access it.

## 10. Idempotency

Retryable mutation endpoints must support an idempotency mechanism where duplicate execution could otherwise create duplicate resources or side effects.

## 11. Concurrency

Mutable resources should use an explicit optimistic-concurrency mechanism such as a version/ETag equivalent where appropriate.

## 12. Request limits

The API must define limits for:

- JSON body size;
- note payload size;
- attachment size;
- batch operation size;
- pagination limits;
- synchronization batch size.

Limits must protect the low-capacity NAS from accidental or malicious resource exhaustion.

## 13. Pagination

Every potentially large collection endpoint must have bounded page size and deterministic ordering.

## 14. Logging

API logs may contain request identifiers, route names, timing and safe status information.

They must never contain:

- note plaintext;
- private search queries where sensitive;
- passwords;
- recovery keys;
- raw tokens;
- encryption keys;
- biometric data.

## 15. Correlation IDs

Requests should carry or receive a correlation identifier so failures can be diagnosed without logging sensitive payloads.

## 16. Caching

Private decrypted content must not be placed into shared HTTP caches.

Cache-control behavior for protected responses must be explicitly defined.

## 17. Retry

Clients may retry transient failures only when the endpoint is documented as retry-safe. Authentication failures and validation failures must not be blindly retried.

## 18. Backward compatibility

Non-breaking fields may be added with documented semantics. Existing required fields must not change meaning silently.
