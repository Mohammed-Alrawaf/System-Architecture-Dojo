# Scenario 01: Architecture Strategy & Decision Matrix

This document outlines the strategic decision-making process behind the Real-Time Alert Gateway. It acts as an Architecture Decision Record (ADR), showing how the technical blueprint must adapt when faced with regulatory constraints, commercial pressure, partner adoption challenges, and execution realities.

---

## 1. Strategic Context

The legacy architecture relied on a **pull-based model**, where external partners constantly queried internal systems for shipment updates.

This created three business problems:

1. **High compute cost** from repeated polling.
2. **Data synchronization delays** between internal freight systems and external partner platforms.
3. **SLA and customer experience risk** during peak logistics periods.

The strategic move is to shift from a pull-based model to a **push-based webhook model**, where alerts are sent only when relevant freight events occur.

---

## 2. Core Architecture Decision

### Decision

Use a **push-based webhook gateway** as the primary integration model, while maintaining REST polling as a fallback for partners who are not ready to consume webhooks.

### Why This Decision Was Made

The webhook model was selected because it:

- Reduces unnecessary API and database traffic.
- Improves partner visibility into freight status changes.
- Supports near-real-time alert delivery after batch completion.
- Creates stronger auditability through API telemetry.
- Provides a foundation for future API monetization.

### Options Considered

| Option | Description | Outcome |
|------|-------------|---------|
| REST Polling | Partners repeatedly call an API to check for updates | Rejected as primary model due to cost and inefficiency |
| Webhooks | System pushes alerts to partners when updates are available | Selected for Phase 1 |
| True Event Streaming | CDC + Kafka/Event Hubs for near-real-time streaming | Deferred to future phase due to cost and legacy risk |
| Manual Portal Access | Partners manually check shipment updates | Rejected due to poor scalability and customer experience |

### Trade-Off

The selected approach improves delivery automation, but it does not make the underlying data truly real-time if the legacy backend still depends on nightly ETL.

---

## 3. Architecture Decision Log

| Decision Area | Decision | Reason | Trade-Off |
|--------------|----------|--------|-----------|
| Integration Model | Use webhook push model | Reduces wasted polling and improves partner notification speed | Requires partners to expose webhook endpoints |
| Legacy Constraint | Keep nightly ETL for Phase 1 | Avoids risky changes to fragile backend systems | Data freshness is still limited by batch timing |
| Partner Readiness | Maintain REST fallback | Prevents partner churn | Increases support complexity |
| Payload Size | Use Claim Check Pattern for large payloads | Prevents partner systems from crashing on massive batches | Requires secure temporary file access |
| Cloud Strategy | Support cloud-native and on-prem fallback | Handles data sovereignty constraints | Requires two deployment models |
| Telemetry | Log all delivery attempts | Supports SLA defense and operational reporting | Adds storage and monitoring overhead |

---

## 4. Architecture Decision Matrix: Edge Cases

An enterprise architecture must survive business reality. The following pivots are pre-approved if original assumptions are invalidated.

### Pivot A — Regulatory Wall: Cloud Ban

If data sovereignty laws block public cloud usage, the architecture degrades from AWS API Gateway to an on-premises COTS platform such as IBM webMethods hosted inside the internal DMZ.

**Strategic Reasoning:**  
The business capability remains the same, but execution shifts from cloud-native to internally controlled infrastructure.

---

### Pivot B — Fixed Subscription Pricing

If the business rejects pay-per-call billing, strict hard-throttling must be enforced at the gateway to cap maximum daily compute cost per partner.

**Strategic Reasoning:**  
This protects internal infrastructure cost even when commercial pricing is not usage-based.

---

### Pivot C — Massive Payload Event

If a market anomaly generates a very large alert batch, the gateway applies the **Claim Check Pattern**.

Instead of pushing the full payload, it sends a lightweight notification containing a secure temporary download URL.

**Strategic Reasoning:**  
This protects partner systems from crashing while still delivering the required data.

---

## 5. Product Adoption & Go-To-Market Strategy

A technical deployment fails if external B2B partners refuse to adopt it.

The adoption strategy must balance modernization with partner readiness.

### Incentivizing Migration

The business should avoid punishing partners immediately for using legacy channels.

Instead:

- New tracking features should only be available through the new JSON API.
- Early adopters may receive temporary commercial incentives.
- Legacy access should remain available during the migration window.

### The Stubborn Partner: Adapter Pattern

If a Tier-1 partner refuses to consume JSON webhooks, internal modernization should not stop.

**Solution:**  
Build an internal Adapter Microservice.

The gateway sends the JSON webhook to the adapter, and the adapter converts it into the partner’s preferred format, such as CSV/XML over SFTP.

**Strategic Benefit:**  
The company modernizes internally while the partner experiences minimal disruption.

---

## 6. Execution & Organizational Change Management

The rollout must account for operational readiness, not only technical correctness.

### Phased Rollout

| Phase | Scope | Purpose |
|------|-------|---------|
| Phase 1: Internal MVP | Push alerts internally to IT support channels | Validate infrastructure, WAF rules, and operational visibility |
| Phase 2: Pilot Partner | Onboard one technically mature Tier-1 partner | Validate external delivery and support model |
| Phase 3: General Availability | Launch developer portal and broader onboarding | Scale adoption across partner ecosystem |

---

### Training & Enablement

**Current Gap:**  
The AppOps support team is used to monitoring SQL batch jobs, not asynchronous JSON webhooks.

**Required Enablement:**

- Train support teams using Postman to visualize webhook delivery.
- Create runbooks for checking failed delivery attempts.
- Document how to inspect Dead Letter Queue items.
- Teach support teams how to interpret HTTP 503 and retry logs.

---

### Risk Mitigation Before External Exposure

Before any B2B endpoint is exposed publicly:

- InfoSec must audit OAuth 2.0 token flows.
- WAF rules must be tested.
- HMAC signing must be validated.
- Partner endpoint allowlisting must be confirmed.
- Monitoring and alerting must be active.

---

## 7. Decision Quality Layer

This section connects the scenario to the Executive Knowledge Base.

The purpose is to make the architecture decision more transparent by checking for bias, uncertainty, and false confidence.

---

### Bias Audit

| Bias | How It Could Affect This Scenario | Mitigation |
|------|----------------------------------|------------|
| Confirmation Bias | Assuming webhooks are automatically better than polling | Compare webhooks, polling, and event streaming before selecting |
| Complexity Bias | Over-engineering the solution with Kafka or CDC too early | Use webhook-based MVP first, defer CDC to future phase |
| Sunk Cost Fallacy | Keeping the legacy portal because partners already use it | Define migration path and planned obsolescence |
| Loss Aversion | Avoiding modernization because partner disruption feels risky | Use adapter pattern and phased migration |
| Overconfidence / Planning Fallacy | Underestimating partner onboarding and support effort | Pilot with one Tier-1 partner before general availability |
| Authority Bias | Accepting the preferred technology of a senior stakeholder without analysis | Use decision matrix and documented trade-offs |

---

## 8. Forecasting & Uncertainty Check

Inspired by *Superforecasting*, this section avoids presenting the design as certain. Instead, it records assumptions, probabilities, and conditions that may change the decision.

### Forecasts

| Forecast | Confidence | Why |
|---------|------------|-----|
| Webhook delivery will reduce wasted partner polling traffic | 80% | Alerts are pushed only when updates exist |
| One Tier-1 partner can be onboarded successfully during pilot | 65% | Depends on partner technical readiness |
| REST fallback will still be required after launch | 75% | Some partners will resist webhook adoption |
| True CDC will be deferred beyond Phase 1 | 85% | Cost and legacy risk are high |
| Claim Check Pattern will be required for large batches | 60% | Depends on shipment volume spikes and payload size |

### What Would Change My Mind

The architecture should be revisited if:

- Partners broadly reject webhook adoption.
- Regulatory constraints fully ban cloud-native components.
- Batch data latency becomes unacceptable to the business.
- Shipment volumes create payload sizes beyond expected thresholds.
- The legacy backend becomes stable enough for CDC/event streaming.

---

## 9. Strategic Summary

This scenario is not simply about replacing SOAP with JSON.

The real architectural move is shifting the business from:

- manual tracking to automated notification,
- blind polling to event-triggered delivery,
- weak observability to auditable telemetry,
- rigid partner integration to flexible adoption paths.

The chosen design is intentionally pragmatic.

It does not attempt to solve every future problem in Phase 1. Instead, it creates a controlled modernization path that balances cost, risk, regulatory pressure, and partner adoption.

## What I Would Do Differently (Iteration Thinking)

### Phase 2 Improvements

- Introduce event streaming (Kafka/Event Hubs) if real-time requirements increase  
- Automate REST fallback onboarding through a developer portal  
- Enhance partner self-service monitoring and alerting  

---

### Assumptions That May Fail

- Assumption: Partners will adopt webhooks quickly  
  → Risk: Some partners may resist change longer than expected  

- Assumption: Nightly ETL latency is acceptable  
  → Risk: Business may require near-real-time updates  

- Assumption: Payload sizes remain manageable  
  → Risk: High shipment volume could increase payload complexity  

---

### What I Would Simplify

- Reduce initial scope of telemetry to essential metrics only  
- Delay advanced monetization features until adoption stabilizes  
- Avoid introducing CDC early unless business demand is proven  


## Personal Reflection

This scenario highlights the importance of balancing:
- technical capability  
- partner readiness  
- cost constraints  
- regulatory limitations  

The solution is intentionally pragmatic.

Instead of aiming for perfect real-time architecture, it focuses on:
- controlled modernization  
- measurable improvement  
- and realistic adoption  

This reflects a shift in thinking:
from “best technical solution”  
to “best decision under constraints”
