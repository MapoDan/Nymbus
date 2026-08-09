# Nymbus — Deployment Architecture

## 1. Deployment target

The primary V1 target is a self-hosted low-capacity NAS using Docker containers.

## 2. Deployment philosophy

The default deployment must be small, deterministic and easy to restore. The number of containers and persistent services should be minimized.

## 3. Logical stack

The deployment consists conceptually of:

- reverse proxy/TLS boundary;
- Nymbus application backend;
- Nymbus frontend assets delivered by the application or a lightweight web layer;
- relational database;
- persistent encrypted object/file storage.

Optional infrastructure must not be required for normal operation.

## 4. Stateless application

Application containers should remain as stateless as practical. Durable state belongs in database/storage volumes.

## 5. Scaling

V1 targets vertical optimization rather than horizontal scaling. Multiple application replicas are not a requirement.

## 6. Updates

Application updates must preserve persistent data formats and provide a migration/rollback strategy. Encrypted-data migrations require particular care because ciphertext must remain interpretable by the approved crypto version.

## 7. Backup and restore

Deployment documentation must define backup of all persistent volumes and restoration order. A deployment is not considered restorable if encrypted content can be restored without its protected key metadata.

## 8. Resource limits

Container CPU and memory limits should be explicit and configurable. Background synchronization, indexing and maintenance work must use bounded concurrency.

## 9. Security

Only the reverse-proxy/TLS entry point should be exposed externally unless an explicit deployment requirement states otherwise. Database and storage services remain internal.
