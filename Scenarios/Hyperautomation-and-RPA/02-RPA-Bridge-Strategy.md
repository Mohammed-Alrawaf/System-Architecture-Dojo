# Scenario 02: Strategic Architecture Decision Record — Legacy Portal RPA Bridge

This document outlines the strategic decision-making process behind the Asynchronous RPA API Bridge. It explains why the architecture uses API Gateway, SQL state management, Blue Prism queues, and Human-in-the-Loop fallback to expose a legacy government portal safely through a modern API.

---

## 1. Executive Summary

The business requires API-driven access to corporate registration data from a legacy government portal that does not provide API capabilities.

Because the source system is only available through a web interface, the solution uses **Robotic Process Automation (RPA)** to bridge the gap.

Blue Prism digital workers will:

- Navigate the government portal UI
- Authenticate using secure credentials
- Scrape required registration data
- Validate extracted data
- Write the final JSON response into an on-premise SQL state table

The API layer remains responsive by using an asynchronous request-reply pattern.

---

## 2. Strategic Context

The challenge is not simply “automating a portal.”

The real architecture problem is:

> How do we expose a slow, fragile, UI-based legacy process as a stable API without allowing client traffic to overwhelm the RPA infrastructure?

This requires separation between:

- The **client-facing API experience**
- The **slow backend RPA execution**
- The **state tracking layer**
- The **exception handling process**

---

## 3. Core Architecture Decision

### Decision

Use an **Asynchronous RPA API Bridge** with SQL-based state management.

### Why This Decision Was Made

This approach was selected because it:

- Allows clients to submit requests quickly
- Prevents API polling from hitting the RPA bot layer directly
- Gives every request a trackable lifecycle state
- Protects Blue Prism workers from uncontrolled traffic
- Supports manual fallback when automation fails

### Options Considered

| Option | Description | Outcome |
|------|-------------|---------|
| Direct Portal Access | Clients manually access the government portal | Rejected due to poor user experience and no API governance |
| Synchronous API + RPA | API waits until bot completes the scrape | Rejected due to timeout risk and poor scalability |
| Asynchronous API + RPA | API accepts request and clients poll for status | Selected for Phase 1 |
| Full System Integration | Direct integration with government backend | Not available because no API exists |
| Manual Operations Only | Human team processes all requests | Rejected due to scale and turnaround time |

### Trade-Off

The selected approach creates a modern API experience, but the backend process is still limited by the speed and fragility of UI automation.

---

## 4. Architecture Decision Log

| Decision Area | Decision | Reason | Trade-Off |
|--------------|----------|--------|-----------|
| API Pattern | Asynchronous request-reply | Prevents API timeout while bot works in background | Client must poll for completion |
| State Layer | SQL state table | Decouples API traffic from RPA infrastructure | Requires state lifecycle governance |
| Bot Trigger | Blue Prism queue | Supports controlled task execution | Queue monitoring required |
| Data Validation | JSON Schema validation inside bot process | Prevents corrupted scraped data from entering database | Adds validation logic to RPA workflow |
| Credential Handling | Use CyberArk / Key Vault | Prevents hardcoded credentials | Requires vault integration and support |
| Exception Handling | DLQ + HITL fallback | Allows safe recovery from unknown failures | Requires operations team process |
| Commercial Model | Subscription tiers by request volume | Controls bot resource consumption | Requires quota enforcement and reporting |

---

## 5. State Management & API Integration Strategy

To protect the RPA layer from read-heavy client traffic, the API does not query the bot infrastructure directly.

Instead, lifecycle state is stored in a centralized SQL table.

### State Flow

| State | Meaning |
|------|---------|
| `PENDING` | Request accepted and queued |
| `PROCESSING` | Bot is executing the task |
| `COMPLETED` | Data was scraped, validated, and stored |
| `MANUAL_REVIEW` | Automation failed and HITL is required |
| `FAILED` | Request exceeded timeout or unrecoverable failure occurred |

### Integration Behavior

- Service A creates the request and writes `PENDING`.
- The request is pushed into the Blue Prism queue.
- Service B reads status only from SQL.
- The bot updates the SQL state after completion.
- Failed tasks are routed to DLQ or HITL.

### Why This Matters

This design prevents client polling from overwhelming bot workers and keeps the API layer scalable even when the RPA layer is slow.

---

## 6. Commercials & Total Cost of Ownership

### Estimated Cost Profile

| Cost Type | Estimate | Description |
|----------|----------|-------------|
| OpEx | ~$90,000/year | Blue Prism licenses, Windows VM hosting, API Gateway infrastructure |
| CapEx | ~$50,000–$80,000 | Vendor implementation, UI mapping, UAT delivery |
| Timeline | ~15 weeks | Procurement, InfoSec audit, development, testing, UAT |

### Delivery Timeline

| Phase | Duration | Focus |
|------|----------|-------|
| Procurement / SOW | 4 weeks | Vendor approval and scope finalization |
| InfoSec Audit | 3 weeks | Credential vault, portal access, network/security review |
| Agile Development | 6 weeks | API, queue, bot workflow, validation, state management |
| UAT | 2 weeks | Partner testing, exception handling, production readiness |

---

## 7. Product Strategy & Monetization

To prevent noisy-neighbor resource exhaustion, API access is monetized and controlled through subscription tiers.

| Tier | Price | Limit | SLA Model |
|-----|-------|-------|-----------|
| Basic | $1,000/month | 100 requests/day | Best effort via shared bot pool |
| Pro | $5,000/month | 1,000 requests/day | Priority routing in Blue Prism queue |
| Enterprise | Custom pricing | 5,000+ requests/day | Dedicated isolated Blue Prism VM |

### Strategic Reasoning

The pricing model is not only commercial.

It is also an architecture control mechanism.

Higher-volume customers require stronger isolation to prevent one client from consuming all bot capacity.

---

## 8. Enterprise Risk Matrix

| Risk Category | Threat / Failure State | Architectural Mitigation |
|--------------|------------------------|---------------------------|
| Data Integrity | Government site layout changes, causing corrupted scraped data | JSON Schema validation before database write |
| Data Security | Hardcoded credentials inside RPA script | CyberArk / Key Vault credential retrieval |
| Infrastructure | Bot VMs freeze or Blue Prism Orchestrator fails | Monitoring via Splunk / Dynatrace and PagerDuty alerts |
| State Corruption | Connection drops between RPA bot and SQL DB | SQL transactions, exponential backoff, orphan sweeps |
| Process Failure | Captcha, unexpected portal errors, or unknown exceptions | DLQ and Human-in-the-Loop fallback |
| Capacity Risk | Too many client requests for available bots | Subscription tiers, rate limits, queue prioritization |

---

## 9. Operational Knowledge & Support

The architecture requires operational support, not only technical deployment.

### Internal Admin Portal

A secure internal admin portal should allow HITL operators to:

- View DLQ exceptions
- Review failed bot attempts
- Manually execute portal searches
- Submit validated data back into `tbl_Request_State`
- Avoid direct SQL access

### Runbooks Required

- Resetting stuck or locked Blue Prism queue items
- Handling upstream government portal outages
- Reviewing failed requests in DLQ
- Validating manually processed records
- Escalating bot infrastructure failures
- Auditing unusual request spikes

---

## 10. Decision Quality Layer

This section connects the scenario to the Executive Knowledge Base.

The purpose is to make the architecture decision more transparent by checking for bias, uncertainty, and false confidence.

---

### Bias Audit

| Bias | How It Could Affect This Scenario | Mitigation |
|------|----------------------------------|------------|
| Automation Bias | Assuming RPA is reliable because it is automated | Add validation, DLQ, HITL, and monitoring |
| Complexity Bias | Overbuilding the API platform before proving demand | Start with MVP and controlled request tiers |
| Sunk Cost Fallacy | Continuing RPA forever because licensing was purchased | Define exit criteria for replacing RPA with direct API integration if available |
| Overconfidence / Planning Fallacy | Underestimating portal instability, captcha, or layout changes | Add UAT, monitoring, and manual fallback |
| Authority Bias | Accepting vendor timeline or tool recommendation without challenge | Validate vendor assumptions with proof-of-concept |
| Loss Aversion | Avoiding automation due to fear of failure | Use controlled rollout and manual fallback instead of rejecting modernization |

---

## 11. Forecasting & Uncertainty Check

Inspired by *Superforecasting*, this section avoids presenting the RPA bridge as a perfect solution. Instead, it records assumptions, confidence levels, and conditions that may require the architecture to change.

### Forecasts

| Forecast | Confidence | Why |
|---------|------------|-----|
| Asynchronous polling will reduce API timeout failures | 85% | API no longer waits for bot completion |
| SQL state management will protect bot infrastructure from read-heavy traffic | 80% | Clients poll SQL-backed status, not Blue Prism directly |
| Government portal layout changes will cause failures within the first year | 70% | UI automation is fragile by nature |
| HITL fallback will be required in production | 75% | Captcha, portal downtime, and unexpected UI changes are likely |
| Enterprise-tier customers will need isolated bot capacity | 65% | High-volume clients may exhaust shared worker pools |
| A direct API integration would eventually be superior to RPA | 90% | RPA is a bridge pattern, not the ideal long-term target |

### What Would Change My Mind

The architecture should be revisited if:

- The government portal releases an official API.
- Captcha or anti-bot controls make automation unreliable.
- RPA failure rate exceeds acceptable SLA thresholds.
- Subscription demand does not justify Blue Prism licensing costs.
- Operational teams cannot support HITL workload.
- Regulatory rules prevent automated scraping.

---

## 12. Strategic Summary

This scenario is not simply about wrapping RPA with an API.

The real architectural move is creating a controlled bridge between:

- modern API expectations,
- legacy UI-only systems,
- operational risk,
- data quality controls,
- and commercial capacity management.

The chosen design is intentionally pragmatic.

It accepts that RPA is not the perfect long-term architecture, but uses it as a controlled modernization bridge until a better upstream integration option becomes available.

---

## 13. Personal Reflection

This scenario highlights the importance of separating the user-facing experience from backend execution constraints.

A client may experience a clean API, but the architecture must still respect the limitations of slow UI automation, fragile portals, and limited bot capacity.

The key lesson is:

> Do not expose backend fragility directly to clients.

Instead, use state management, queues, validation, and fallback processes to create a stable service boundary around an unstable backend.
