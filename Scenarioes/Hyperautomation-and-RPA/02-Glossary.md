# Domain Glossary: Hyperautomation & API Integration

* **Asynchronous Polling Pattern:** An architectural pattern where a server immediately acknowledges a request but processes it in the background, requiring the client to periodically "poll" a separate endpoint to retrieve the final result.
* **Backpressure:** A system's ability to resist incoming data when it is overwhelmed, typically implemented via rate limiting or shedding load to protect backend resources from crashing.
* **Dead Letter Queue (DLQ):** A secondary storage queue used to isolate messages or tasks that cannot be processed successfully after multiple retries, allowing for manual inspection without blocking the main workflow.
* **Digital Worker:** A software robot (bot) designed to automate specific tasks and execute business processes, operating securely alongside human workers on virtual machines.
* **Exponential Backoff:** A standard error-handling strategy for network applications in which the client periodically retries a failed request with increasing delays between attempts (e.g., waiting 5s, then 15s, then 30s).
* **Human-in-the-Loop (HITL):** A system design that routes exceptions or high-complexity tasks to a human operator when automated processes (like RPA bots) fail or encounter unknown variables.
* **Idempotency:** A property of API design ensuring that making the same request multiple times produces the same result without causing unintended duplication (e.g., caching a bot request so spamming "submit" doesn't launch 50 identical bots).
* **Noisy Neighbor Problem:** A cloud computing and shared-infrastructure issue where one client consumes an excessive amount of shared resources (like bot CPU time), drastically degrading performance for other clients.
* **Payload:** The actual data or message content being transmitted via an API, excluding headers and metadata.
* **Rate Limiting:** A defensive mechanism used by APIs to restrict the number of requests a client can make within a specific timeframe (e.g., returning an HTTP 429 status code) to ensure system stability.
* **Robotic Process Automation (RPA):** Software technology used to build, deploy, and manage robots that emulate human actions interacting with legacy digital systems and software.
* **State Management:** The mechanism by which an application stores, updates, and tracks the lifecycle status of a specific transaction or request across distributed systems (e.g., using a SQL Database).
* **Time-To-Live (TTL):** A mechanism that limits the lifespan of a task or data packet in a system. Used here as a "watchdog timer" to automatically kill frozen RPA bot sessions.
