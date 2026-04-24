## 1. The Problem Statement

**Current State:** A B2B logistics and freight forwarding company currently relies on a legacy, portal-based system for shipment tracking. Retail partners must manually log in or run heavy, scheduled batch jobs to retrieve tracking updates from a legacy SOAP XML backend. 

**Business Pain Point:** The "pull-based" architecture causes severe data desync for retail partners. During peak seasons, delayed tracking updates lead to increased customer support tickets, lost revenue, and missed Service Level Agreement (SLA) targets. The legacy portal is becoming a massive bottleneck.

**Target State:** Design a high-throughput, "push-based" API Gateway. The architecture must transition from manual batch processing to real-time, event-driven webhooks. It needs to translate the legacy XML backend data into modern JSON payloads, pushing status changes directly into external B2B partner CRMs the moment a freight event occurs.
