# Scenario 03: Strategic Architecture Decision Record — Deterministic KYC Orchestration

## Source Scenario

This scenario is inspired by the AWS Architecture Blog:

**Modernizing KYC with AWS serverless solutions and agentic AI for financial services**  
https://aws.amazon.com/blogs/architecture/modernizing-kyc-with-aws-serverless-solutions-and-agentic-ai-for-financial-services/

The original AWS reference architecture explores cloud-native KYC modernization using serverless services, event-driven architecture, and agentic AI.

This scenario intentionally explores an alternative architecture lens:

> What if the financial institution operates under strict regulatory, explainability, and data-sovereignty constraints that make fully cloud-native agentic decisioning unsuitable for Phase 1?

---

## 1. Executive Summary

The business requires faster digital onboarding while maintaining strong compliance control.

The current KYC process is slow because it depends on manual intervention across multiple siloed systems:

- document ingestion
- legacy identity verification
- batch risk scoring
- compliance review dashboards

This results in onboarding delays of several days, while the mobile channel expects a near-real-time onboarding experience.

The proposed solution is a **deterministic KYC orchestration layer** that modernizes the customer journey without giving full decision authority to non-deterministic AI models.

The architecture uses:

- API Facades to wrap legacy systems
- Intelligent Document Processing (IDP) for document extraction
- a deterministic real-time Risk API
- a Single Pane of Glass for compliance review
- Shadow Mode and Canary Release for controlled rollout

The goal is to achieve high Straight-Through Processing (STP) for low-risk applicants while preserving regulatory explainability and auditability.

---

## 2. Strategic Context

KYC modernization is not only a technology problem.

It is a balance between:

- customer onboarding speed
- regulatory compliance
- explainability
- data sovereignty
- operational cost
- fraud prevention
- legacy system constraints

The AWS reference architecture highlights how modern KYC can use event-driven workflows, Amazon MSK, AWS Lambda, Amazon Bedrock, and agentic AI to process KYC cases faster and at scale.

However, in this scenario, the institution operates in a regulatory environment where:

- PII must remain on-premise or within strict regional boundaries
- automated rejection decisions must be explainable
- compliance teams require deterministic reason codes
- risk models must produce repeatable outcomes
- regulators may challenge black-box or probabilistic decisions

Therefore, the architecture prioritizes deterministic orchestration over fully autonomous AI decisioning.

---

## 3. Core Architecture Decision

### Decision

Use a **deterministic, on-premise KYC orchestration layer** for Phase 1.

AI-enabled tools may be used for extraction and assistance, but final KYC decisioning must remain rule-based, explainable, and auditable.

---

### Why This Decision Was Made

The deterministic approach was selected because it:

- keeps sensitive data within controlled infrastructure
- provides repeatable decision outcomes
- supports regulatory explainability
- reduces dependency on black-box decisioning
- allows legacy systems to be modernized without full replacement
- enables phased modernization instead of high-risk transformation

---

### Important Distinction

This design does **not** reject AI completely.

It separates AI usage into three categories:

| AI Usage Area | Phase 1 Position | Reason |
|--------------|------------------|--------|
| Document extraction | Allowed | IDP improves speed while humans/rules validate output |
| Analyst assistance | Allowed with control | Can summarize or highlight issues without making final decisions |
| Automated KYC decisioning | Restricted | Final approval/rejection must be deterministic and auditable |

---

## 4. Options Considered

| Option | Description | Outcome |
|------|-------------|---------|
| Cloud-Native Agentic AI | Uses LLMs and autonomous agents for document analysis, fraud detection, and compliance routing | Deferred for Phase 1 due to data sovereignty, explainability, and regulatory concerns |
| Deterministic On-Premise Orchestration | Uses API Facades, IDP, deterministic risk scoring, and controlled human review | Selected for Phase 1 |
| Keep Existing Manual Process | Continue manual reviews across siloed systems | Rejected due to poor customer experience and operational inefficiency |
| Full Core System Replacement | Replace legacy KYC and risk systems entirely | Rejected due to cost, complexity, and delivery risk |
| Hybrid AI-Assisted Review | Use AI to assist compliance officers but not make final decisions | Candidate for Phase 2 |

---

## 5. Architecture Decision Log

| Decision Area | Decision | Reason | Trade-Off |
|--------------|----------|--------|-----------|
| KYC Decisioning | Deterministic rules and reason codes | Required for auditability and regulatory explanation | Less adaptive than AI-led decisioning |
| Document Processing | Use IDP for extraction | Speeds up document handling and reduces manual entry | Requires validation to avoid OCR errors |
| Legacy Integration | Use API Facade pattern | Modernizes access without replacing core systems | Adds middleware complexity |
| Risk Scoring | Extract batch logic into real-time Risk API | Enables faster onboarding decisions | Requires careful parity testing against legacy batch results |
| Deployment | Use Shadow Mode before go-live | Validates output accuracy before customer impact | Extends delivery timeline |
| Rollout | Use Canary Release | Reduces production risk | Requires monitoring and traffic control |
| Compliance Review | Use Single Pane of Glass | Reduces manual review time | Requires UI/process change for compliance team |

---

## 6. Enterprise Constraints Matrix

| Constraint Area | Challenge | Architecture Response |
|----------------|-----------|------------------------|
| Regulatory Compliance | Rejections must be explainable | Deterministic decision rules and reason codes |
| Data Sovereignty | PII cannot freely move to public cloud | On-premise orchestration and controlled data residency |
| Customer Experience | Mobile onboarding expects near-real-time response | STP path for low-risk applicants |
| Legacy Systems | Existing KYC systems are siloed and batch-oriented | API Facades and orchestration layer |
| Operational Readiness | Compliance teams work across multiple screens | Single Pane of Glass dashboard |
| Cost | IDP and infrastructure introduce new cost | Use STP only where measurable value exists |
| Reliability | Legacy protocol failures may occur | Graceful error handling and fallback review path |

---

## 7. Product & Operating Model

### Target Outcome

The target is to reduce onboarding time from several days to near-real-time for low-risk applicants.

A realistic target is:

> Up to 85% Straight-Through Processing after Shadow Mode validation confirms accuracy against the legacy process.

This is treated as a target, not a guaranteed Day-1 outcome.

---

### Compliance Team Role Change

The compliance team shifts from:

```text
manual data entry and system checking
```

to:

```text
exception handling and decision review
```

This requires training, updated runbooks, and operational readiness.

---

### Customer Experience Model

| Applicant Type | System Response |
|---------------|-----------------|
| Low-risk, valid identity, clean documents | Auto-approved through STP |
| Medium-risk or incomplete information | Routed to compliance review |
| High-risk or failed validation | Escalated for enhanced due diligence |

---

## 8. Deployment Strategy

A Big Bang deployment is rejected due to compliance and operational risk.

### Phase 1: Shadow Mode

The new orchestration layer runs silently alongside the existing process.

It receives real application data but does not affect the official business outcome.

The purpose is to compare:

- extracted document data
- identity validation results
- risk score outputs
- approval/rejection outcomes
- reason codes

Shadow Mode continues until the new system proves parity with the legacy process.

---

### Phase 2: Canary Release

Live traffic is gradually routed to the new system.

Example rollout:

| Stage | Traffic Routed |
|------|----------------|
| Canary 1 | 5% |
| Canary 2 | 20% |
| Canary 3 | 50% |
| Full Release | 100% |

Monitoring must include:

- decision accuracy
- latency
- false rejection rate
- manual review volume
- system error rate
- database locks
- CPU and memory thresholds

---

## 9. Enterprise Risk Matrix

| Risk Category | Threat / Failure State | Architectural Mitigation |
|--------------|------------------------|---------------------------|
| Data Quality | IDP extracts incorrect document fields | Validation rules, confidence thresholds, and manual review |
| Compliance | Automated decision cannot be explained | Deterministic rules and reason codes |
| Data Sovereignty | PII leaves approved environment | On-premise or regionally controlled processing |
| Legacy Integration | SOAP endpoint returns unexpected HTML or malformed XML | Defensive parsing and fallback routing |
| Operational Change | Compliance team resists new workflow | Training and phased adoption |
| Model Risk | AI-generated output influences decisions incorrectly | AI limited to extraction/assistance, not final decisioning |
| Performance | Real-time Risk API fails under peak mobile traffic | Active-active deployment and load balancing |
| Cutover Risk | New system produces different decisions from legacy batch | Shadow Mode parity testing |

---

## 10. Decision Quality Layer

This section connects the scenario to the Executive Knowledge Base.

The purpose is to make the architecture decision more transparent by checking for bias, uncertainty, and false confidence.

---

### Bias Audit

| Bias | How It Could Affect This Scenario | Mitigation |
|------|----------------------------------|------------|
| Confirmation Bias | Assuming deterministic architecture is safer because it feels more controllable | Compare with cloud-native and hybrid AI options before selecting |
| Complexity Bias | Assuming agentic AI is better because it is more advanced | Evaluate whether complexity improves compliance outcomes |
| Loss Aversion | Rejecting AI entirely because of regulatory fear | Allow controlled AI usage for extraction and assistance |
| Overconfidence / Planning Fallacy | Assuming legacy risk logic can be extracted easily | Use Shadow Mode to validate parity |
| Authority Bias | Accepting AWS or vendor architecture as automatically suitable | Adapt reference architecture to local constraints |
| Sunk Cost Fallacy | Preserving legacy systems longer than necessary | Define future exit path toward API-native or hybrid architecture |

---

## 11. Forecasting & Uncertainty Check

Inspired by *Superforecasting*, this section avoids presenting the architecture as certain. It records assumptions, confidence levels, and conditions that may require the design to change.

### Forecasts

| Forecast | Confidence | Why |
|---------|------------|-----|
| Deterministic decisioning will be easier to defend with regulators in Phase 1 | 80% | Repeatable rules and reason codes are easier to audit |
| IDP will reduce manual document handling time | 75% | Extraction removes repetitive data entry, but accuracy depends on document quality |
| Shadow Mode will reveal mismatches with legacy batch logic | 70% | Legacy systems often contain hidden rules and undocumented exceptions |
| Compliance teams will still require manual review for medium/high-risk cases | 85% | KYC risk decisions usually require human judgment for edge cases |
| Agentic AI may become viable for assisted review in later phases | 60% | Depends on governance maturity and regulator acceptance |
| Full autonomous AI decisioning will remain restricted in the near term | 75% | Explainability and accountability concerns remain significant |

---

### What Would Change My Mind

The architecture should be revisited if:

- regulators explicitly approve AI-based decisioning with clear governance requirements
- the institution receives approval for controlled cloud processing of PII
- IDP accuracy is too low to support STP safely
- Shadow Mode shows high mismatch between real-time Risk API and legacy batch results
- compliance workload does not decrease after dashboard adoption
- customer drop-off remains high despite faster processing

---

## 12. What I Would Do Differently (Iteration Thinking)

### Phase 2 Improvements

- Introduce AI-assisted compliance review, but keep final decisions human-approved
- Add fraud-pattern analytics using historical onboarding behavior
- Build a policy rules engine so compliance teams can update rules without developer deployment
- Introduce real-time monitoring of false positives and false negatives
- Explore hybrid cloud processing if regulators approve specific workloads

---

### Assumptions That May Fail

- Assumption: Deterministic rules are enough for fraud detection  
  → Risk: fraud patterns may evolve faster than rules can be updated

- Assumption: IDP accuracy will be high enough for STP  
  → Risk: poor document quality may increase manual review

- Assumption: Compliance teams will adopt the new dashboard  
  → Risk: operational resistance may reduce expected efficiency gains

- Assumption: Legacy batch logic can be extracted cleanly  
  → Risk: hidden rules may exist in old jobs or manual workarounds

---

### What I Would Simplify

- Start with one customer segment before expanding to all onboarding types
- Limit STP to low-risk cases only
- Keep AI out of final decisioning in Phase 1
- Delay advanced agentic workflows until governance matures

---

## 13. Personal Reflection

This scenario highlights the importance of regulatory thinking in architecture.

The most advanced technology is not always the best first decision.

In a highly regulated environment, the better architecture may be the one that is:

- explainable
- repeatable
- auditable
- operationally realistic

The key lesson is:

> Use AI where it improves speed and insight, but keep accountability clear where decisions affect customers.

---

## 14. Knowledge Integration

This scenario is influenced by principles from the Executive Knowledge Base.

### From Superforecasting

- Decisions are expressed with uncertainty and confidence levels
- Assumptions are documented and revisited
- The architecture avoids presenting one solution as universally correct

---

### From The Art of Thinking Clearly

- Bias audit is applied to reduce flawed decision-making
- The design avoids blindly following either vendor hype or internal fear
- Overconfidence and complexity bias are explicitly checked

---

### From Atomic Habits

- Modernization is treated as incremental improvement
- Shadow Mode and Canary Release create controlled feedback loops
- The operating environment is redesigned so compliance teams can work differently

---

> The goal is not to reject innovation.
> The goal is to apply innovation safely under real-world constraints.
