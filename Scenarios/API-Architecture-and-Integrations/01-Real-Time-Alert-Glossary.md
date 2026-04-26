# Scenario 01: Architecture Glossary

This document translates the technical components used in the Real-Time Alert Gateway into clear business and technical language.

The purpose is to bridge the gap between:
- technical implementation  
- executive understanding  
- architectural intent  

---

## 1. Security & Governance

### HMAC (Hash-based Message Authentication Code)

**Executive View:**  
A digital tamper-proof seal that ensures the data received is exactly what was sent and from a trusted source.

**Technical View:**  
A cryptographic mechanism that uses a shared secret key and hashing algorithm (e.g., SHA-256) to sign a payload.  
The receiver validates the signature to detect tampering or impersonation attempts.

---

### Immutable Audit Logs

**Executive View:**  
A permanent, unchangeable record of system activity used for compliance and dispute resolution.

**Technical View:**  
A Write-Once-Read-Many (WORM) storage model where records cannot be modified or deleted after being written.  
Ensures non-repudiation and regulatory compliance.

---

## 2. Telemetry & Monitoring

### Deep Telemetry

**Executive View:**  
A detailed health monitoring system that shows exactly how the system behaves in real time.

**Technical View:**  
The continuous collection of metrics, logs, and traces that provide visibility into system performance, execution time, payload size, and request paths.  
Used for troubleshooting, optimization, and cost analysis (FinOps).

---

## 3. Integration & Infrastructure

### Message Broker (e.g., RabbitMQ, Kafka)

**Executive View:**  
A traffic control system that manages large volumes of data safely and prevents system overload.

**Technical View:**  
An asynchronous messaging system that decouples producers from consumers.  
It queues messages and processes them at a controlled rate, enabling scalability, fault tolerance, and retry mechanisms.

---

### API Gateway (e.g., Kong, AWS API Gateway)

**Executive View:**  
The secure front door of the system that controls access, enforces rules, and monitors usage.

**Technical View:**  
A reverse proxy layer that handles authentication, rate limiting, routing, and request validation between external clients and backend services.  
Centralizes cross-cutting concerns such as security and traffic management.

---

### Enterprise Service Bus / iPaaS (e.g., IBM webMethods)

**Executive View:**  
A fully integrated platform that handles data transformation, routing, and system communication in one place.

**Technical View:**  
A commercial integration platform that combines API management, message brokering, and data transformation capabilities.  
Often used in enterprise environments for legacy system integration, though typically at a higher licensing cost.

---

## 4. Design Patterns Used in This Scenario

### Claim Check Pattern

**Executive View:**  
A way to handle large data safely by sending a link instead of the full data.

**Technical View:**  
A pattern where large payloads are stored externally, and only a reference (URL) is sent through the system.  
Prevents performance issues and reduces load on receiving systems.

---

## 5. Why This Matters

This glossary ensures that:

- Technical decisions are understandable at the executive level  
- Business stakeholders can follow architectural discussions  
- Teams share a consistent understanding of key concepts  

> Clear language reduces risk, improves alignment, and accelerates delivery.
