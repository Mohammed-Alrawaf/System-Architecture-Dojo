# Strategic Architecture Decision Record: Legacy Portal RPA Bridge

## 1. Executive Summary
The business requires real-time, API-driven access to corporate registration data from a legacy government portal that lacks API capabilities. This initiative bridges modern RESTful API architecture with back-office Robotic Process Automation (RPA) using Blue Prism digital workers, enabling automated JSON payloads without human intervention.

## 2. State Management & API Integration Strategy
To prevent overwhelming the RPA infrastructure with read-heavy polling requests, a decoupled State Management architecture is utilized:
* **Storage:** All `RequestIDs` are stored in a centralized SQL table (`tbl_Request_State`).
* **Interaction:** Service A writes the `PENDING` state to the DB and pushes the payload to the Blue Prism Queue. Service B reads strictly from the DB, shielding the RPA environment from client traffic. 
* **Bot Write-Back:** Upon completion, the Blue Prism bot updates the SQL record to `COMPLETED` and writes the final scraped data payload.

## 3. Commercials & Total Cost of Ownership (TCO)
* **Operational Expenditure (OpEx):** ~$90,000/year (5 Blue Prism concurrent worker licenses + Windows VM hosting + API Gateway infrastructure).
* **Capital Expenditure (CapEx):** ~$50,000 - $80,000 for external vendor implementation, UI mapping, and UAT delivery.
* **Delivery Timeline:** Target Go-Live is 15 weeks (4 weeks procurement, 3 weeks InfoSec audit, 6 weeks agile development, 2 weeks UAT).

## 4. Enterprise Risk Matrix & Security Mitigations
| Risk Category | Threat / Failure State | Architectural Mitigation |
| :--- | :--- | :--- |
| **Data Security** | Hardcoded credentials in RPA script | Implementation of **CyberArk / Key Vault**. Bots request Government portal credentials dynamically at runtime in memory. |
| **Infrastructure** | Bot VMs freeze or Orchestrator crashes | Splunk/Dynatrace monitoring tracks active bot count. PagerDuty alerts triggered to AppOps if consumption drops to zero. |
| **Queue Misrouting** | Payload pushed to wrong Dev/UAT queue | Service A configurations use strict Environment Variables mapping to `Target_Queue: "Gov_Portal_Prod"`. Validation checks for BP `Item_ID` response. |
| **Process Failure** | Captcha added / UI drastically changes | System routes task to a Dead Letter Queue (DLQ). Triggers the **Human-in-the-Loop (HITL)** fallback for manual processing. |

## 5. Operational Knowledge & Support (Runbooks)
To ensure platform reliability, a Level-2 AppOps Runbook will be established prior to Go-Live, detailing:
* Procedures for safely resetting "Stuck" or "Locked" queue items in Blue Prism.
* Escalation paths for when the upstream Government portal undergoes unannounced maintenance.
* SLA guidelines for processing Human-in-the-Loop (HITL) manual overrides within 24 hours.
