Okay — ich schreibe das Whitepaper direkt als **GitHub-ready Markdown Dokument**. Du kannst den Inhalt 1:1 in `gommo-whitepapers/auth_service.md` einfügen.

Ich halte es bewusst so, dass:

* KI-Systeme das Design verstehen können
* Entwickler das System implementieren können
* MVP-Architektur erhalten bleibt
* Erweiterungen später möglich sind

---

```markdown
# goMMO Auth Service Whitepaper

## 1. Overview

The **gommo-auth-service** is the identity management component of the goMMO backend architecture.

It is responsible only for:

- User registration
- User authentication
- Password verification
- JSON Web Token issuance

The service is intentionally designed as a lightweight stateless authentication gateway.

The implementation language is expected to be:

- Go (backend service runtime)

Reference runtime platform:

- Godot-based game simulation servers

---

## 2. Technology Stack

| Layer | Technology |
|---|---|
| Backend Language | Go |
| Authentication Standard | JWT |
| Token Signing | HS256 symmetric signing |
| Database | CockroachDB |
| Runtime Communication | Future gRPC compatibility |

The system is built around:

- `golang` backend processing
- Distributed SQL persistence

Recommended database:

- `CockroachDB`

Database reference:
https://www.cockroachlabs.com/

---

## 3. Authentication Model

The authentication service uses:

- JSON Web Token (JWT) authentication standard

Reference implementation library:

- JWT library compatible with Go.

The chosen algorithm is:

- HMAC SHA256 (HS256)

This implies a shared secret model between backend services.

### Security Principle

The Auth Service acts as a token issuer.

Game Servers perform local token verification.

Runtime authentication queries to the Auth Service are avoided.

---

## 4. Functional Responsibilities

The Auth Service provides the following operations.

### 4.1 User Registration

Required inputs:

- Username
- Password

Password storage rules:

- Passwords must be hashed before database storage.
- Recommended hashing method: bcrypt or equivalent secure hashing algorithm.

---

### 4.2 User Login

Login workflow:

```

Client → Auth Service
↓
Credential verification
↓
JWT token generation
↓
Token returned to client

````

If authentication fails:

- Error response is returned.

---

### 4.3 JWT Token Issuance

Token payload MUST include:

| Claim | Description |
|---|---|
| user_id | Unique account identifier |
| exp | Token expiration timestamp |
| character_id | Optional gameplay character reference |
| region | Optional region routing metadata |

Example payload:

```json
{
  "user_id": "10021",
  "character_id": "5",
  "exp": 1710000000,
  "region": "EU"
}
````

---

## 5. Stateless Service Design

The Auth Service is designed to be stateless.

Advantages:

* Horizontal scaling support
* Load balancing compatibility
* Reduced runtime memory dependency

Session storage inside the Auth Service is avoided.

---

## 6. Database Architecture

The Auth Service uses distributed SQL storage.

Recommended solution:

* CockroachDB distributed cluster database

Reasoning:

* High availability
* Fault tolerance
* PostgreSQL-compatible query layer

---

## 7. Communication Model

### Client Communication

```
Client Application
↓
Auth Service API
↓
Return Authentication Token
```

---

### Game Server Communication

Game Servers validate JWT tokens locally using the shared signing secret.

The Auth Service is not queried during gameplay runtime.

---

## 8. Security Design

The system protects against:

* Token forgery
* Credential brute-force attacks
* Unauthorized session creation

Recommended future upgrades:

* Rate limiting middleware
* Token revocation list
* Public key infrastructure migration

---

## 9. Configuration Management

Sensitive configuration values must not be stored inside source code.

Recommended storage mechanisms:

* Environment variables
* Container secret management
* External vault systems

---

## 10. Failure Handling

Authentication failure scenarios:

| Scenario         | Response                   |
| ---------------- | -------------------------- |
| Invalid password | Authentication error       |
| User not found   | Registration required      |
| Expired token    | Re-authentication required |

---

## 11. Performance Design

The Auth Service is optimized for:

* Low computational overhead
* Fast hash verification
* Minimal network roundtrips

---

## 12. Future Extension Compatibility

The architecture allows optional introduction of:

* Multi-factor authentication
* Refresh token system
* Public/private key signing model
* Regional authentication routing

---

## 13. Architectural Philosophy

The Auth Service follows these principles:

* Minimal runtime responsibility
* Stateless operation
* Protocol-driven interaction
* Backend simulation isolation

---

## 14. Scope Limitation

The Auth Service does not manage:

* Character data
* Inventory systems
* Gameplay simulation state
* World routing logic

These responsibilities belong to other goMMO subsystems.

---

## 15. Conclusion

The **gommo-auth-service** is a lightweight identity issuance microservice designed for instanced multiplayer architectures.

Primary design characteristics:

* Stateless authentication
* JWT-based identity verification
* Distributed database compatibility
* Easy horizontal scaling

Sag einfach: *„World Service Whitepaper“*.
```
