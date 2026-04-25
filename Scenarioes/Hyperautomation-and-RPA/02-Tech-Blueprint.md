# Technical Blueprint: Asynchronous RPA API Bridge

## 1. Architectural Pattern
This solution utilizes the **Asynchronous Request-Reply (Polling) Pattern** combined with **Idempotency** to bridge a fast API Gateway (30-second timeout) with a slow backend UI automation process (~3-minute execution).

## 2. Component Design & Traffic Flow
1.  **API Gateway (Service A):** Validates API Key, enforces Rate Limits, and pushes the `Company_ID` directly into the Blue Prism REST API Work Queue. 
2.  **Blue Prism (Digital Workers):** A pool of 5 Windows VMs constantly polls the Work Queue, accesses the target web portal, scrapes the data, and updates the queue status.
3.  **Telemetry DB (SQL Server):** Records every transaction payload, timestamp, and HTTP status code to the `Usage_DumpUsage` table for monthly client billing and Power BI analytics.

## 3. API Contract Specifications

### Service A: The Initiator (`POST /api/v1/registry/request`)
Receives the request and immediately terminates the HTTP connection to prevent 504 Timeouts.
* **Success Response:** `202 Accepted`
* **Payload:** Returns a unique `RequestID` and instructions to poll Service B.

### Service B: The Poller (`GET /api/v1/registry/status/{RequestID}`)
Queried by the client every 30 seconds to check bot progress.
* **State 1 (Running):** Returns `200 OK` | `{"status": "PROCESSING"}`
* **State 2 (Finished):** Returns `200 OK` | `{"status": "COMPLETED", "data": {...}}`
* **State 3 (HITL Fallback):** Returns `200 OK` | `{"status": "MANUAL_REVIEW", "message": "Routed to operations.", "estimated_completion": "24_HOURS"}`

### Service C: The Audit Trail (`GET /api/v1/registry/history?company_id={ID}`)
Returns historical API calls utilizing pagination (limit 50 per page) to prevent payload bloat.

## 4. Resilience & Backpressure Engineering
To protect the RPA infrastructure from traffic spikes, strict API Rate Limiting is enforced at the Gateway level. If a client exceeds their tier quota, the gateway rejects the payload before it reaches the bot queue.

**HTTP 429 Payload (Rate Limit Exceeded):**
```json
HTTP/1.1 429 Too Many Requests
Retry-After: 3600
Content-Type: application/json

{
  "error_code": "RATE_LIMIT_EXCEEDED",
  "message": "Quota exceeded. Please wait 3600 seconds."
}
