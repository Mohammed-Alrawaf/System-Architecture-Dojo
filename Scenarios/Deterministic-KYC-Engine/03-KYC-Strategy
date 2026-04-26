# Scenario 03: Strategic Architecture Decision Record — Deterministic KYC Orchestration

---

## Source Scenario

This scenario is inspired by the AWS Architecture Blog:

**Modernizing KYC with AWS serverless solutions and agentic AI for financial services**  
https://aws.amazon.com/blogs/architecture/modernizing-kyc-with-aws-serverless-solutions-and-agentic-ai-for-financial-services/

> **Key Insight**
> This scenario intentionally explores an alternative architecture lens under stricter regulatory and data-sovereignty constraints.

---

## 1. Executive Summary

The business requires faster digital onboarding while maintaining strong compliance control.

Current KYC is slow due to:
- fragmented systems  
- manual review processes  
- batch-based risk scoring  

> **Impact**
> Onboarding takes days while customers expect near-real-time approval.

---

> **Decision**
> Introduce a **deterministic KYC orchestration layer** that improves speed while preserving auditability.

---

## 2. Strategic Context

KYC modernization is a multi-dimensional problem:

- customer experience vs compliance  
- speed vs accuracy  
- innovation vs regulation  
- automation vs accountability  

---

> **Important**
> The AWS reference promotes event-driven + agentic AI architecture.  
> This scenario adapts that idea under stricter enterprise constraints.

---

## 3. Core Architecture Decision

> **Decision**
> Use a **deterministic, on-premise orchestration layer for Phase 1**

---

### Why This Decision Was Made

- Ensures repeatable outcomes  
- Supports auditability  
- Keeps PII within controlled boundaries  
- Enables phased modernization  

---

### AI Usage Boundaries

| AI Usage Area | Phase 1 Position | Reason |
|--------------|------------------|--------|
| Document extraction | Allowed | Improves speed |
| Analyst assistance | Controlled | Supports human decisions |
| Final decisioning | Restricted | Must remain auditable |

---

> **Key Insight**
> This is not an anti-AI design.  
> It is a **controlled AI adoption strategy**.

---

## 4. Options Considered

| Option | Outcome |
|------|---------|
| Agentic AI (cloud-native) | Deferred (regulatory constraints) |
| Deterministic orchestration | Selected |
| Manual process | Rejected |
| Full system replacement | Rejected |
| Hybrid AI-assisted review | Future phase |

---

## 5. Architecture Decision Log

| Decision Area | Decision | Trade-Off |
|--------------|----------|-----------|
| Decisioning | Deterministic rules | Less adaptive |
| Document Processing | IDP | Requires validation |
| Integration | API Facade | Adds complexity |
| Risk Scoring | Real-time API | Requires testing |
| Deployment | Shadow Mode | Slower rollout |
| Rollout | Canary | Requires monitoring |
| Compliance UX | Single Pane | Requires change adoption |

---

## 6. Enterprise Constraints Matrix

| Constraint | Response |
|-----------|----------|
| Regulatory | Deterministic decisions |
| Data Sovereignty | On-prem control |
| CX Expectations | STP for low-risk |
| Legacy Systems | API Facades |
| Operations | Unified dashboard |
| Cost | Controlled IDP usage |

---

## 7. Product & Operating Model

> **Target**
> Achieve **up to 85% STP** after validation.

---

### Compliance Shift

```text
Before → manual processing  
After  → exception handling
```

---

### Customer Flow

| Case | Outcome |
|------|--------|
| Low risk | Auto-approved |
| Medium risk | Manual review |
| High risk | Escalated |

---

## 8. Deployment Strategy

> **Key Principle**
> Avoid Big Bang deployment

---

### Phase 1: Shadow Mode

- Runs alongside legacy system  
- No business impact  
- Validates output parity  

---

### Phase 2: Canary Release

| Stage | Traffic |
|------|--------|
| 1 | 5% |
| 2 | 20% |
| 3 | 50% |
| 4 | 100% |

---

## 9. Enterprise Risk Matrix

| Risk | Mitigation |
|------|-----------|
| Data Quality | Validation + thresholds |
| Compliance | Reason codes |
| Data Leakage | On-prem control |
| Legacy Failures | Defensive parsing |
| Resistance | Training |
| AI Risk | Restricted usage |
| Performance | Load balancing |
| Cutover | Shadow Mode |

---

## 10. Decision Quality Layer

### Bias Audit

| Bias | Mitigation |
|------|-----------|
| Confirmation | Compare options |
| Complexity | Avoid overengineering |
| Loss Aversion | Controlled AI use |
| Overconfidence | Shadow Mode |
| Authority Bias | Adapt AWS approach |
| Sunk Cost | Plan future exit |

---

## 11. Forecasting & Uncertainty

| Forecast | Confidence |
|---------|------------|
| Deterministic easier for regulators | 80% |
| IDP reduces effort | 75% |
| Shadow Mode reveals gaps | 70% |
| Manual review remains | 85% |
| AI viable later | 60% |

---

## 12. Iteration Thinking

### Phase 2

- AI-assisted review  
- Fraud analytics  
- Rules engine  

---

### Risks

- Fraud evolves faster than rules  
- IDP accuracy issues  
- Adoption resistance  

---

### Simplifications

- Start small  
- Limit STP scope  
- Delay AI decisioning  

---

## 13. Personal Reflection

> **Insight**
> The best architecture is not always the most advanced.

This design prioritizes:

- explainability  
- control  
- realistic execution  

---

## 14. Knowledge Integration

### Superforecasting
- Uncertainty is explicit  
- Decisions are probabilistic  

---

### Thinking Clearly
- Bias awareness applied  
- Avoids hype vs fear extremes  

---

### Atomic Habits
- Incremental change  
- Controlled feedback loops  

---

> **Final Thought**
> Innovation is not rejected.  
> It is applied carefully under constraints.
