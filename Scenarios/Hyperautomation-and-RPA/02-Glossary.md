# Scenario 02: Architecture Glossary — Hyperautomation & API Integration

This document explains the key concepts used in the Asynchronous RPA API Bridge.

It is designed to bridge the gap between:
- technical implementation  
- business understanding  
- architectural decision-making  

---

## 1. Core Architecture Concepts

### Asynchronous Polling Pattern

**Executive View:**  
A way to request data without waiting. The system accepts the request and processes it in the background.

**Technical View:**  
An API pattern where a client submits a request and receives a `RequestID`. The client then polls a separate endpoint to check the status until the result is ready.

---

### State Management

**Executive View:**  
A tracking system that shows where a request is in its lifecycle.

**Technical View:**  
A centralized mechanism (typically a database) that stores and updates the status of a request (e.g., `PENDING`, `PROCESSING`, `COMPLETED`) across distributed components.

---

### Idempotency

**Executive View:**  
A safety mechanism that prevents duplicate work.

**Technical View:**  
An API design principle ensuring that repeated identical requests produce the same result without triggering duplicate processing (e.g., preventing multiple bot executions for the same request).

---

## 2. RPA & Automation Concepts

### Robotic Process Automation (RPA)

**Executive View:**  
Software robots that perform repetitive tasks by mimicking human actions.

**Technical View:**  
Automation technology that interacts with existing systems through their user interfaces, executing tasks such as data entry, navigation, and extraction without requiring backend APIs.

---

### Digital Worker

**Executive View:**  
A virtual employee that performs tasks automatically.

**Technical View:**  
A software bot running on a virtual machine, executing predefined workflows using RPA tools like Blue Prism.

---

### Human-in-the-Loop (HITL)

**Executive View:**  
A fallback process where humans handle tasks that automation cannot complete.

**Technical View:**  
A design pattern that routes failed or complex automated tasks to human operators for manual review and completion.

---

## 3. Resilience & Control Mechanisms

### Dead Letter Queue (DLQ)

**Executive View:**  
A holding area for failed tasks that need attention.

**Technical View:**  
A secondary queue where messages or tasks are routed after repeated processing failures, allowing for inspection and manual handling without blocking the main workflow.

---

### Exponential Backoff

**Executive View:**  
A retry strategy that waits longer between each attempt.

**Technical View:**  
A fault-handling mechanism where retry attempts are delayed progressively (e.g., 5s → 15s → 30s) to reduce system load and avoid repeated immediate failures.

---

### Time-To-Live (TTL)

**Executive View:**  
A timer that prevents tasks from running forever.

**Technical View:**  
A mechanism that limits the lifespan of a request or process. If the time is exceeded, the task is automatically terminated or marked as failed.

---

### Backpressure

**Executive View:**  
A system’s ability to slow down incoming work when it is overloaded.

**Technical View:**  
A design approach that controls the rate of incoming requests to prevent system overload, often implemented through rate limiting or queue management.

---

## 4. API & Infrastructure Concepts

### Rate Limiting

**Executive View:**  
A control mechanism that limits how much a client can use the system.

**Technical View:**  
An API protection strategy that restricts the number of requests a client can make within a defined timeframe (e.g., HTTP 429 responses when limits are exceeded).

---

### Payload

**Executive View:**  
The actual data being sent or received.

**Technical View:**  
The body of an API request or response, excluding headers and metadata.

---

### Noisy Neighbor Problem

**Executive View:**  
One user consuming too many resources, affecting others.

**Technical View:**  
A condition in shared infrastructure where one client’s excessive usage degrades performance for other clients.

---

## 5. Why This Matters

This glossary ensures that:

- Technical decisions are clearly understood by non-technical stakeholders  
- Business teams can follow architectural discussions  
- Engineering teams share a consistent vocabulary  

> Clear definitions reduce confusion, improve alignment, and accelerate delivery.
