# Nymbus — Caching

## 1. Principle

Caching is an optimization and must never change authorization or cryptographic guarantees.

## 2. Client cache

The client may cache metadata and encrypted content needed for offline operation. Private plaintext should not be intentionally cached beyond active use.

## 3. Server cache

V1 should avoid a mandatory distributed cache. Application-level in-memory caching is permitted only for small, non-sensitive, bounded data such as static configuration or short-lived metadata.

## 4. Authorization

Cached authorization decisions must have a short, explicit validity model and must never survive a revocation event in a way that permits unauthorized server operations.

## 5. Private content

Server-side plaintext caching of private notes is prohibited.

## 6. Invalidation

Changes affecting permissions, notes or synchronization must invalidate relevant cache state. The correctness path must not depend on cache invalidation being perfect.

## 7. Resource limits

Caches must have bounded size and eviction behavior to protect a low-capacity NAS.
