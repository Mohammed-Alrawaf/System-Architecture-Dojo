### File 3: `02-Glossary.md`
*(Copy the text below and paste it into this file)*

```markdown
# Domain Glossary: Hyperautomation & API Integration

* **Asynchronous Polling Pattern:** An architectural pattern where a server immediately acknowledges a request but processes it in the background, requiring the client to periodically "poll" a separate endpoint to retrieve the final result.
* **Backpressure:** A system's ability to resist incoming data when it is overwhelmed, typically implemented via rate limiting or shedding load to protect backend resources from crashing.
* **Dead Letter Queue (DLQ):** A secondary storage queue used to isolate messages or tasks that cannot be processed successfully after multiple retries, allowing for manual inspection without blocking the main workflow.
* **Human-in-the-Loop (HITL):** A system design that routes exceptions or high-complexity tasks to a human operator when automated processes (like RPA bots) fail or encounter unknown variables.
* **Idempotency:** A property of API design ensuring that making the same request multiple times produces the same result without causing unintended duplication (e.g., caching a bot request so spamming "submit" doesn't launch 50 identical bots).
* **Noisy Neighbor Problem:** A cloud computing and shared-infrastructure issue where one client consumes an excessive amount of shared resources (like bot CPU time), drastically degrading performance for other clients.
* **Time-To-Live (TTL):** A mechanism that limits the lifespan of a task or data packet in a system. Used here as a "watchdog timer" to automatically kill frozen RPA bot sessions.
