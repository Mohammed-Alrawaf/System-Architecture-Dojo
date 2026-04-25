# Scenario 01: Architecture Strategy & Decision Matrix

This document outlines the strategic decision-making process behind the Real-Time Alert Gateway. It acts as an Architecture Decision Record (ADR), detailing how the technical blueprint must pivot when faced with strict organizational constraints, commercial disputes, and execution realities.

## 1. The Core Strategy: Why Push Over Pull?
The legacy architecture relied on a "Pull" model, where external partners constantly queried our databases. This created unacceptable compute costs and data synchronization delays. The strategic shift to a "Push" model (Webhooks) was chosen to align IT infrastructure costs strictly with actual business events, rather than wasted polling cycles.

## 2. Architecture Decision Matrix (Edge Cases)

An enterprise architecture must survive the realities of the business. Below are the pre-approved strategic pivots if core assumptions are invalidated:

* **Pivot A - The Regulatory Wall (Cloud Ban):** If data sovereignty laws block public cloud usage, we degrade from AWS API Gateway to an On-Premises COTS (e.g., IBM webMethods) hosted within the internal DMZ.
* **Pivot B - Fixed Subscription Pricing:** If the business rejects a pay-per-call billing model, we must enforce strict hard-throttling (API Rate Limits) at the Gateway to cap maximum daily compute costs per partner.
* **Pivot C - The Massive Payload:** If a market anomaly generates a 500MB JSON payload, we execute the **Claim Check Pattern**, dropping the payload and pushing a secure, temporary download URL instead to prevent server crashes.

## 3. Product Adoption & Go-To-Market (GTM) Strategy

A technical deployment is a failure if external B2B partners refuse to adopt it. We enforce adoption through strategic incentives and architectural buffers, avoiding SLA penalties.

* **Incentivizing Migration:** We will not penalize legacy usage. Instead, we enforce "Planned Obsolescence"—100% of new tracking features will exclusively be available via the new JSON API. Additionally, we will offer commercial incentives (e.g., waived API overage fees for 6 months) for partners who migrate before Q4.
* **The Stubborn Partner (Adapter Pattern):** If a Tier-1 partner flat-out refuses to dedicate developer resources to consume our new JSON webhook, we will not halt our internal modernization. 
  * *Solution:* We will build an internal Adapter Microservice. The Gateway fires the JSON webhook to the Adapter, which converts it back into a legacy CSV/XML and drops it on an SFTP server for the partner. We successfully retire our heavy legacy database while the partner experiences zero disruption.

## 4. Execution & Organizational Change Management (OCM)

The deployment must account for the operational readiness of internal IT teams. 

* **Phased Rollout Timeline:**
  * **Phase 1 (Internal MVP):** Deploy Gateway and push internal alerts to IT support (MS Teams/Slack) to validate infrastructure and security (WAF) configurations.
  * **Phase 2 (Pilot):** Onboard one highly technical Tier-1 partner. 
  * **Phase 3 (General Availability):** Open the developer portal for self-service onboarding.
* **Training & Enablement:**
  * **The Gap:** The current AppOps support team is accustomed to monitoring SQL batch jobs. They do not know how to troubleshoot asynchronous JSON webhooks.
  * **The Fix:** Before Phase 2, mandate training using Postman to visualize payload delivery. Establish Runbooks detailing how to check the Dead Letter Queue (DLQ) and read HTTP 503 error logs.
* **Risk Mitigation:** To prevent the "Zero API" cold-start problem, InfoSec must audit the OAuth 2.0 token server and Web Application Firewall (WAF) *before* any external B2B endpoints are exposed to the public internet.
