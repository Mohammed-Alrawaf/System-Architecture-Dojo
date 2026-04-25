# Scenario 01: Architecture Glossary

This document serves as a translation matrix for the specific technologies and architectural concepts utilized in the Real-Time Alert Gateway scenario. It bridges deep technical execution with executive business strategy.

## Security & Governance

### HMAC (Hash-based Message Authentication Code)
* **Executive Summary:** A digital "tamper-evident seal" for data in transit. It ensures that the payload received is exactly what was sent, and that it was sent by an authorized party.
* **Technical Definition:** A cryptographic authentication technique that uses a shared secret key and a cryptographic hash function (e.g., SHA-256) to sign a JSON payload. The receiving server validates the signature to prevent Man-in-the-Middle (MitM) alterations or DNS hijacking.

### Immutable Audit Logs
* **Executive Summary:** A digital ledger written in permanent ink, secured behind unbreakable glass. Used for strict legal compliance and SLA dispute resolution.
* **Technical Definition:** A "Write-Once, Read-Many" (WORM) data storage architecture. Once a system event (e.g., an API request) is recorded, it is cryptographically locked. It cannot be altered or deleted, even by database administrators with root access, ensuring absolute non-repudiation.

## Telemetry & Monitoring

### Deep Telemetry
* **Executive Summary:** Moving beyond "is the server on or off?" to measuring the exact heartbeat, blood pressure, and oxygen levels of an IT system in real-time. 
* **Technical Definition:** The granular, automated collection of system metrics, distributed traces, and log data. It captures exact execution times in milliseconds, payload sizes, and network traversal paths, enabling advanced FinOps billing (cost-to-serve) and precise fault isolation.

## Integration & Infrastructure

### Message Broker (e.g., RabbitMQ, Apache Kafka)
* **Executive Summary:** A highly efficient post office sorting facility. It absorbs massive, sudden spikes in traffic and hands the workload to the servers at a safe, manageable speed so they don't crash.
* **Technical Definition:** An asynchronous message-queuing middleware that decouples data producers from data consumers. It holds system events in memory queues, allowing for traffic smoothing, guaranteed delivery, and fault-tolerant retry mechanisms.

### API Gateway (e.g., Kong, AWS API Gateway)
* **Executive Summary:** The heavily armed bouncer at the front door of your IT estate. It checks IDs, enforces VIP rules, kicks out abusers, and counts exactly who enters.
* **Technical Definition:** A reverse proxy that sits between external clients and internal backend services. It centralizes cross-cutting concerns such as OAuth 2.0 authentication, strict rate limiting, payload inspection, and request routing, eliminating the need to code these security features into every individual microservice.

### Enterprise Service Bus / iPaaS (e.g., IBM webMethods)
* **Executive Summary:** A massive, fully-staffed digital fulfillment center. It handles receiving, translating, sorting, and shipping all under one proprietary roof, bought off-the-shelf.
* **Technical Definition:** A Commercial Off-The-Shelf (COTS) integration platform. It acts as a combined API Gateway, Message Broker, and Visual Data Transformation engine. While highly secure and robust for legacy modernization, it carries high licensing costs compared to composable open-source cloud architectures.
