# Bias Audit Framework for Architecture & Product Decisions

**Source:** Inspired by *The Art of Thinking Clearly* by Rolf Dobelli  
**Purpose:** Detect and mitigate cognitive biases in architecture, product, and delivery decisions.

---

## Core Principle

> Most bad architecture decisions are not technical failures — they are thinking failures.

This framework is used to audit decisions before they become expensive mistakes.

---

## Zone 1 — Creation & Solution Design

Biases that affect how solutions are designed.

### IKEA Effect  
**Risk:** Overvaluing solutions we build ourselves  
**Architecture Impact:** Building custom systems when existing solutions are better  
**Mitigation:** Always benchmark against external/vendor options  

---

### Not-Invented-Here  
**Risk:** Rejecting external solutions to preserve ownership  
**Architecture Impact:** Reinventing existing tools  
**Mitigation:** Require evaluation of at least 2 external alternatives  

---

### Complexity Bias  
**Risk:** Assuming complex systems are superior  
**Architecture Impact:** Over-engineering  
**Mitigation:** Prefer the simplest solution that meets requirements  

---

## Zone 2 — Data & Evidence

Biases that affect how we interpret data.

### Survivorship Bias  
**Risk:** Ignoring failures and only seeing successes  
**Architecture Impact:** Copying patterns that worked elsewhere without context  
**Mitigation:** Analyze failure cases, not just success stories  

---

### Confirmation Bias  
**Risk:** Looking for data that supports our existing belief  
**Architecture Impact:** Choosing preferred architecture without real comparison  
**Mitigation:** Assign a “devil’s advocate” during design reviews  

---

### Option Blindness  
**Risk:** Not exploring real alternatives  
**Architecture Impact:** Settling on first solution  
**Mitigation:** Require at least 3 architecture options  

---

### Clustering Illusion  
**Risk:** Seeing patterns in random data  
**Architecture Impact:** Misinterpreting system behavior or trends  
**Mitigation:** Validate using real metrics and historical data  

---

## Zone 3 — Team & Stakeholders

Biases that come from people and group dynamics.

### Authority Bias  
**Risk:** Overvaluing senior opinions  
**Architecture Impact:** Accepting decisions without challenge  
**Mitigation:** Separate ideas from hierarchy  

---

### Groupthink  
**Risk:** Avoiding disagreement  
**Architecture Impact:** Weak decisions due to lack of debate  
**Mitigation:** Encourage structured disagreement  

---

### Halo Effect  
**Risk:** Trusting based on reputation  
**Architecture Impact:** Choosing vendors or tools without proper evaluation  
**Mitigation:** Use objective evaluation criteria  

---

### Framing Effect  
**Risk:** Decisions change based on wording  
**Architecture Impact:** Misaligned understanding of problems  
**Mitigation:** Reframe the problem in multiple ways  

---

## Zone 4 — Strategy & Delivery

Biases that affect long-term decisions and execution.

### Sunk Cost Fallacy  
**Risk:** Continuing due to past investment  
**Architecture Impact:** Keeping failing legacy systems  
**Mitigation:** Define exit criteria early  

---

### Loss Aversion  
**Risk:** Avoiding change due to fear of loss  
**Architecture Impact:** Delaying modernization  
**Mitigation:** Quantify both risk and opportunity  

---

### Outcome Bias  
**Risk:** Judging decisions by results only  
**Architecture Impact:** Reinforcing bad decision processes  
**Mitigation:** Evaluate decision quality, not just outcomes  

---

### Overconfidence / Planning Fallacy  
**Risk:** Underestimating effort and risk  
**Architecture Impact:** Unrealistic timelines and designs  
**Mitigation:** Use probabilistic estimation and buffers  

---

## How to Use This

Apply this checklist during:

- Architecture design reviews  
- Sprint planning  
- Roadmap definition  
- Stakeholder discussions  

---

## Example (Applied to Scenario)

```md
### Bias Audit — API Gateway Scenario

- ⚠️ Confirmation Bias: Initial preference for event-driven design  
- Mitigation: Compared with REST polling and batch processing  

- ⚠️ Overconfidence Bias: Timeline underestimated  
- Mitigation: Added phased rollout and buffer  

- ⚠️ Complexity Bias: Early design too complex  
- Mitigation: Simplified to webhook-based MVP  
```

---

## Final Thought

> Good architecture is not just about choosing the right solution.  
> It is about avoiding the wrong thinking.
