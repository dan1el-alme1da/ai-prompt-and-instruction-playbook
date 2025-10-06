
---

## WHAT ARE ADAPTERS? 5 REAL-WORLD JAVA USAGE EXAMPLES

---

### Port-Adapter Pattern (Hexagonal Architecture)

I use **adapters** to separate business logic from infrastructure.

Example: `UserPersistenceAdapter` implements the `UserRepositoryPort` port, and encapsulates the JPA calls.

---

### DTO to Entity Conversion (and vice-versa)

To avoid mixing layers, I create **adapters** (or **mappers**) that convert `UserDTO` to `UserEntity` and the other way around.

This keeps the controller clean and the domain separate from persistence.

---

### Usage with Legacy Libraries or Old APIs

When the system needs to communicate with legacy code or old SDKs, I use an **adapter** to translate the call.

This prevents old code from spreading throughout the project.

---

### Integration with Third-Party Services

When I need to call email APIs (like AWS SES or Mailgun), I create an **EmailServiceAdapter** that transforms my internal model into the request expected by the API.

This way, the rest of the system doesn't need to know the integration details.

---

### Easy Implementation Swapping

I have a **StorageAdapter** with two implementations: one for AWS S3 and another for saving locally to disk.

This allows me to swap the storage backend without changing the core logic.

---
