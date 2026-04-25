# Strategic Architecture Decision Record: Legacy Portal RPA Bridge

## 1. Executive Summary
The business requires real-time, API-driven access to corporate registration data from a legacy government portal that lacks API capabilities. This initiative bridges modern RESTful API architecture with back-office Robotic Process Automation (RPA) using Blue Prism digital workers. 

## 2. Commercials & Total Cost of Ownership (TCO)
To justify the integration, a strict Return on Investment (ROI) and cost-analysis framework is applied:
* **Operational Expenditure (OpEx):** ~$90,000/year (5 Blue Prism concurrent worker licenses + Windows VM hosting + API Gateway throughput).
* **Capital Expenditure (CapEx):** ~$50,000 - $80,000 for external vendor (e.g., Darme) implementation, UI mapping, and UAT delivery.
* **Delivery Timeline:** Target Go-Live is 15 weeks (includes 4 weeks procurement/vendor SOW, 3 weeks InfoSec audit, 6 weeks agile development, 2 weeks UAT).

## 3. Product Strategy & Monetization (Subscription Tiers)
To manage bot utilization and prevent "Noisy Neighbor" resource exhaustion, API access is strictly monetized via the API Gateway:
* **Tier 3 (Basic):** $1,000/month. 100 requests/day limit. SLA: Best effort via shared bot pool.
* **Tier 2 (Pro):** $5,000/month. 1,000 requests/day limit. SLA: Priority routing in Blue Prism queue.
* **Tier 1 (Enterprise):** Custom Pricing. 5,000+ requests/day. SLA: Dedicated, isolated Blue Prism VM to guarantee throughput.

## 4. Enterprise Risk Matrix & Mitigations
| Risk Category | Threat / Failure State | Architectural Mitigation |
| :--- | :--- | :--- |
| **Security** | Hardcoded credentials in RPA script | Implementation of **CyberArk / Key Vault**. Bots request credentials dynamically at runtime. |
| **Infrastructure** | Bot VMs freeze or Orchestrator crashes | Splunk/Dynatrace monitoring tracking active bot count. PagerDuty alerts triggered if consumption drops to zero. |
| **Upstream Uptime** | Government portal is offline or times out | Blue Prism logs a "Business Exception." API returns `502 Bad Gateway` to client. |
| **Process Failure** | Captcha added / UI drastically changes | System routes task to a Dead Letter Queue (DLQ). Triggers the **Human-in-the-Loop (HITL)** fallback. |

## 5. Decision Log
* **Decision:** Utilize Blue Prism native Work Queues instead of external message brokers (RabbitMQ).
* **Rationale:** Reduces infrastructure complexity and licensing costs while utilizing the RPA tool's intended orchestration mechanics.
