# Nymbus — Communication

## 1. External communication

The PWA communicates with the backend through a versioned HTTPS API.

TLS is mandatory for remote access. Authentication/session mechanisms must follow the security documents.

## 2. Internal communication

V1 favors in-process module calls for co-deployed domains. If a domain is extracted into a separate process, communication must use a small authenticated internal API rather than an implicit shared-memory contract.

## 3. Asynchronous events

Events are appropriate for notifications, synchronization triggers and non-critical background work. They must not become a mandatory distributed message-bus dependency in V1.

## 4. Idempotency

Mutating network operations that may be retried must carry stable client operation identifiers where appropriate. The server must treat duplicate delivery as safe.

## 5. Payload rules

Payloads must be bounded and versioned. Private note bodies sent through the API are ciphertext only.

## 6. Error contract

Errors must be machine-readable, user-safe and free of secrets. Internal stack traces and cryptographic material must never be returned to clients.

## 7. Timeouts

Network operations must use bounded timeouts and retry policies. Offline clients must not block the UI waiting for network completion.
