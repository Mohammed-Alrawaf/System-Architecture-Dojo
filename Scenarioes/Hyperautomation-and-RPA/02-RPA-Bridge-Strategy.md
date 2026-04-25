# Strategic Architecture Decision Record: Legacy Portal RPA Bridge

## 1. Executive Summary
The business requires real-time, API-driven access to corporate registration data from a legacy government portal that lacks API capabilities. This initiative bridges modern RESTful API architecture with back-office Robotic Process Automation (RPA) using Blue Prism digital workers, enabling automated, validated JSON payloads without human intervention.

## 2. State Management & API Integration Strategy
To prevent overwhelming the RPA infrastructure with read-heavy polling requests, a decoupled State Management architecture is utilized:
* **Storage Layer:** All `RequestIDs` and lifecycle states are stored in a centralized SQL table (`tbl_Request_State`).
* **Interaction:** Service A writes the `PENDING` state to the DB and pushes the trigger to the Blue Prism Queue. Service B reads strictly from the DB, shielding the RPA environment from client traffic. 
* **Bot Write-Back:** Upon completion, the Blue Prism bot updates the SQL record to `COMPLETED` and writes the final JSON payload using strict SQL transactions (`BEGIN TRAN`, `COMMIT`, `ROLLBACK`) to prevent partial data corruption if a network drop occurs.

## 3. Commercials & Total Cost of Ownership (TCO)
* **Operational Expenditure (OpEx):** ~$90,000/year (5 Blue Prism concurrent worker licenses + Windows VM hosting + API Gateway infrastructure).
* **Capital Expenditure (CapEx):** ~$50,000 - $80,000 for external vendor implementation, UI mapping, and UAT delivery.
* **Delivery Timeline:** Target Go-Live is 15 weeks (4 weeks procurement, 3 weeks InfoSec audit, 6 weeks agile development, 2 weeks UAT).

## 4. Enterprise Risk Matrix & Security Mitigations
| Risk Category | Threat / Failure State | Architectural Mitigation |
| :--- | :--- | :--- |
| **Data Integrity** | Gov site layout changes, resulting in garbage data scraped | Strict **JSON Schema Validation** inside the RPA script. Failures abort the DB write and route to the DLQ. |
| **Data Security** | Hardcoded credentials in RPA script | Implementation of **CyberArk / Key Vault**. Bots request Government portal credentials dynamically at runtime. |
| **Infrastructure** | Bot VMs freeze or Orchestrator crashes | Splunk/Dynatrace monitoring tracks active bot count. PagerDuty alerts triggered to AppOps if consumption drops to zero. |
| **State Corruption** | Connection drops between RPA and SQL DB | Automated SQL Agent Job runs an "Orphan Sweep" every 10 minutes, moving 2-hour-old `PROCESSING` tasks to `FAILED`. |
| **Process Failure** | Captcha added / Unknown Exception | System routes task to a Dead Letter Queue (DLQ). Triggers the **Human-in-the-Loop (HITL)** fallback for manual processing. |

## 5. Operational Knowledge & Support (Runbooks)
To ensure platform reliability and data governance, Level-2 AppOps Runbooks will govern system failures:
* **Internal Admin Portal:** A secure internal web UI is provided for the HITL operations team. Staff can view DLQ exceptions, manually execute the portal search, and securely submit the validated data into the `tbl_Request_State` database without requiring direct SQL access.
* **Queue Management:** Procedures for safely resetting "Stuck" or "Locked" queue items in Blue Prism.
* **Upstream Outages:** Escalation paths for when the upstream Government portal undergoes unannounced maintenance.
