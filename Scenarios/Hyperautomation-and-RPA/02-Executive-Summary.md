# Scenario 02: Executive Summary — Asynchronous RPA API Bridge

## Problem

The business requires API access to corporate registration data from a government system that does not provide APIs.

The only available interface is a web portal, which creates:

- manual effort for data retrieval  
- inconsistent turnaround times  
- lack of integration capability  
- operational inefficiency at scale  

---

## Objective

Provide a stable and scalable API layer that:

- exposes the required data to clients  
- maintains acceptable response times  
- protects backend systems from overload  
- ensures data quality and auditability  

---

## Solution

Introduce an **Asynchronous API Bridge powered by RPA (Robotic Process Automation)**.

Key components:

- API Gateway to accept and manage client requests  
- SQL State Management to track request lifecycle  
- Blue Prism digital workers to retrieve data from the portal  
- validation layer to ensure data quality  
- Human-in-the-Loop fallback for failed automation  

---

## Key Decisions

- Use asynchronous request-reply instead of synchronous API calls  
- Decouple API layer from RPA execution using a state database  
- Validate scraped data before storing or returning results  
- Introduce fallback mechanisms for handling automation failures  
- Monetize access through controlled subscription tiers  

---

## Trade-Offs

- API responsiveness is achieved, but backend processing remains slow  
- Automation introduces operational complexity  
- Manual intervention may still be required for certain cases  
- RPA is a temporary bridge, not a long-term ideal solution  

---

## Risks

- Government portal changes may break automation  
- Bot capacity may be insufficient under high demand  
- Manual workload may increase if automation fails frequently  
- Dependency on third-party licensing (RPA platform)  

---

## Outcome

This design:

- enables API-based access where no API exists  
- improves operational efficiency  
- provides controlled scalability  
- ensures data quality and traceability  
- creates a monetizable service model  

---

## Strategic View

This solution is not the final architecture.

It is a **controlled bridge between legacy systems and modern integration needs**.

The design prioritizes:

- stability over speed  
- control over complexity  
- and gradual modernization over risky transformation  
