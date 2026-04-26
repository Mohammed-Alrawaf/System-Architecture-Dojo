# Bias Audit Framework for Architecture & Product Decisions

**Source:** Inspired by *The Art of Thinking Clearly* by Rolf Dobelli  
**Purpose:** Detect and mitigate cognitive biases in architecture, product, and delivery decisions.

---

## Core Principle

> Most bad architecture decisions are not technical failures — they are thinking failures.

---

## Quick Bias Audit (Use This in Real Work)

### Zone 1 — Creation & Solution Design

| Bias | Risk in Architecture | Mitigation |
|------|---------------------|-----------|
| IKEA Effect | Overvaluing internally built solutions | Benchmark against external/vendor solutions |
| Not-Invented-Here | Rejecting proven external tools | Force evaluation of at least 2 external options |
| Complexity Bias | Choosing complex systems assuming higher quality | Prefer simplest solution that meets requirements |

---

### Zone 2 — Data & Evidence

| Bias | Risk in Architecture | Mitigation |
|------|---------------------|-----------|
| Survivorship Bias | Ignoring failed implementations | Review failure case studies |
| Confirmation Bias | Favoring data that supports initial idea | Assign a "devil’s advocate" in design reviews |
| Option Blindness | Not comparing real alternatives | Require minimum 3 architecture options |
| Clustering Illusion | Seeing patterns in noisy data | Validate with statistical or historical data |

---

### Zone 3 — Team & Stakeholders

| Bias | Risk in Architecture | Mitigation |
|------|---------------------|-----------|
| Authority Bias | Following senior opinion blindly | Separate idea from seniority |
| Groupthink | Avoiding conflict for consensus | Encourage structured disagreement |
| Halo Effect | Overtrusting known vendors or teams | Evaluate based on objective criteria |
| Framing Effect | Decisions influenced by wording | Reframe problem in multiple ways |

---

### Zone 4 — Strategy & Delivery

| Bias | Risk in Architecture | Mitigation |
|------|---------------------|-----------|
| Sunk Cost Fallacy | Continuing failing systems | Define exit criteria early |
| Loss Aversion | Avoiding change due to fear of loss | Quantify upside vs downside |
| Outcome Bias | Judging decision by result only | Evaluate decision process, not outcome |
| Overconfidence / Planning Fallacy | Underestimating effort and risk | Use probabilistic estimates |

---

## Expanded Understanding (Deep Thinking Layer)

### Creation & Solution Design

- **IKEA Effect:** We overvalue what we build → leads to unnecessary custom systems  
- **Not-Invented-Here:** Rejecting external tools → slows delivery  
- **Complexity Bias:** Complex ≠ better → often worse  

---

### Data & Evidence

- **Survivorship Bias:** Only seeing success stories → misleading patterns  
- **Confirmation Bias:** Looking for proof, not truth  
- **Option Blindness:** Not exploring alternatives  
- **Clustering Illusion:** Seeing patterns in noise  

---

### Team & Stakeholders

- **Authority Bias:** Senior ≠ correct  
- **Groupthink:** Agreement ≠ good decision  
- **Halo Effect:** Reputation ≠ capability  
- **Framing Effect:** Wording changes decisions  

---

### Strategy & Delivery

- **Sunk Cost Fallacy:** Past investment ≠ future value  
- **Loss Aversion:** Fear of loss blocks progress  
- **Outcome Bias:** Good result ≠ good decision  
- **Overconfidence:** We underestimate risk and effort  

---

## How to Use This

Use this checklist during:

- Architecture design reviews  
- Sprint planning  
- Roadmap discussions  
- Stakeholder alignment  

---

## Example (Applied to Scenario)

```md
### Bias Audit — API Gateway Scenario

- ⚠️ Confirmation Bias: Preferred event-driven approach early  
- Mitigation: Compared with REST and batch alternatives  

- ⚠️ Complexity Bias: Initial design too complex  
- Mitigation: Simplified to webhook-based MVP  

- ⚠️ Overconfidence: Timeline underestimated  
- Mitigation: Added phased rollout and buffer  
```

---

## Final Thought

> Good architecture is not just about choosing the right solution.  
> It is about avoiding the wrong thinking.
