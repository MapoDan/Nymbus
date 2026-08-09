# Nymbus — Service Responsibilities

| Domain | Responsibility | Must not do |
|---|---|---|
| Web/PWA | UI, local state, crypto operations, offline work | trust client authorization as server authority |
| API/Application | request orchestration, validation, business rules | decrypt private content |
| Auth/Session | Google authentication, sessions, passkey registration orchestration | store private plaintext or passwords |
| Notes | note lifecycle and metadata | expose private plaintext to backend |
| Authorization/Sharing | permissions and encrypted-key distribution metadata | decrypt shared note content |
| Sync | operation exchange, ordering, acknowledgements, convergence | grant authorization |
| Search | metadata indexing/search | index private plaintext |
| Attachments | ciphertext/object persistence, metadata, lifecycle | inspect private plaintext |
| Notifications | user-visible events and pending actions | contain secrets/plaintext |
| Admin | system/account administration and health | bypass private-note cryptography |

## Cross-domain rule

A domain may request another domain's operation through its documented contract. It must not directly manipulate another domain's internal storage.

## Security rule

Any proposed service boundary that would require server access to private plaintext is rejected unless the security ADR explicitly changes the product's E2E guarantee.
