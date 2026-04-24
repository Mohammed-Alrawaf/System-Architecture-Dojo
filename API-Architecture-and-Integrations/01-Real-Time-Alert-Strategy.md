# Scenario 01: Architecture Strategy & Decision Matrix

This document outlines the strategic decision-making process behind the Real-Time Alert Gateway. It acts as an Architecture Decision Record (ADR), detailing how the technical blueprint must pivot when faced with strict organizational, regulatory, or commercial constraints.

## 1. The Core Strategy: Why Push Over Pull?
The legacy architecture relied on a "Pull" model, where external partners constantly queried our databases. This created unacceptable compute costs and data synchronization delays. The strategic shift to a "Push" model (Webhooks) was chosen to align IT infrastructure costs strictly with actual business events, rather than wasted polling cycles.

## 2. Architecture Decision Matrix (Edge Cases & Pivots)

An enterprise architecture must survive the realities of the business. Below are the pre-approved strategic pivots if core assumptions are invalidated by stakeholders.

### Pivot A: The Regulatory Wall (Cloud Ban)
* **The Threat:** Data sovereignty laws or the Chief Information Security Officer (CISO) prohibit core logistics data from being processed in a public multi-tenant cloud (AWS/Azure).
* **The Strategic Pivot:** Degrade from a Cloud-Native architecture to an On-Premises COTS (Commercial Off-The-Shelf) architecture. 
* **Execution:** Swap AWS API Gateway for a self-hosted solution like IBM webMethods or Kong Open-Source within the internal DMZ. The Webhook logic remains intact, but the hosting boundary shifts to satisfy legal compliance.

### Pivot B: The "Zero API" Baseline (Low IT Maturity)
* **The Threat:** The organization has never deployed a public-facing API and lacks foundational security infrastructure (WAF, OAuth 2.0 servers).
* **The Strategic Pivot:** Halt immediate external B2B integration to prevent a security breach. Adopt a "Crawl, Walk, Run" rollout strategy.
* **Execution:** First, execute an internal pilot using the Gateway to push internal alerts (e.g., to MS Teams/Slack). Only after the InfoSec team audits and signs off on the internal plumbing will external B2B partner onboarding commence.

### Pivot C: Fixed Subscription Pricing
* **The Threat:** The business rejects a "Tiered Volume" or "Pay-Per-Call" monetization model, insisting on a flat monthly subscription. This exposes IT to uncapped cloud infrastructure costs if a partner generates excessive traffic.
* **The Strategic Pivot:** Shift the API Gateway policy from "Telemetry Only" to "Active Throttling."
* **Execution:** Implement hard rate-limiting quotas (e.g., max 10,000 alerts per day, per partner). If a partner exceeds the quota, the Gateway drops the payload, ensuring cloud compute costs never exceed the fixed subscription revenue.

### Pivot D: The Massive Payload (Black Swan Event)
* **The Threat:** A market anomaly causes the daily ETL batch to generate 500,000 alerts at once. Attempting to push a 500MB JSON payload will crash both our outbound Gateway and the receiving partner's server.
* **The Strategic Pivot:** Implement the **Claim Check Pattern**.
* **Execution:** If a payload exceeds a safety threshold (e.g., 10MB), the Gateway aborts the direct push. Instead, it pushes a lightweight notification containing a secure, temporary URL where the partner can download the massive batch file asynchronously.
