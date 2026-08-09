# Nymbus — Microservices Architecture

## 1. Strategy

Nymbus uses **logical service boundaries** with a lightweight deployment model. V1 must not create a container for every small domain.

The preferred physical architecture is a small number of containers/processes, with modules separated by clear interfaces internally. A domain becomes a separately deployed service only when isolation, lifecycle, scaling, security or operational ownership justifies it.

## 2. Candidate domains

- API/application;
- authentication/session;
- notes;
- sharing/authorization;
- synchronization;
- metadata search;
- attachments;
- notifications;
- administration/observability.

## 3. Resource policy

No service may introduce a continuously running heavyweight dependency merely for convenience.

Avoid by default:

- Kubernetes;
- message brokers;
- distributed caches;
- distributed search engines;
- service meshes.

## 4. Database

A single relational database is the default V1 persistence engine. Logical schemas/tables provide domain separation without requiring separate database servers.

## 5. Internal boundaries

Even when modules run in the same process, they must communicate through explicit domain contracts. Direct access to another domain's persistence structures is prohibited at the architectural level.

## 6. Extraction rule

A module may later be extracted into a service without changing its external contract. This is a future optimization, not a V1 requirement.
