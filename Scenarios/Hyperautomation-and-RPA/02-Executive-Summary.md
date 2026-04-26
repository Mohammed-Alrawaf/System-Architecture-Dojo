# Scenario 02: Executive Summary — Asynchronous RPA API Bridge

## Problem

The business requires API access to a government system that has no APIs.

The current process:
- is manual  
- slow  
- not scalable  

---

## Objective

Provide API access while:
- protecting backend systems  
- maintaining performance  
- ensuring data reliability  

---

## Solution

Introduce an **asynchronous API bridge powered by RPA**.

Core elements:
- API Gateway for request intake  
- SQL state layer for lifecycle tracking  
- RPA bots for data retrieval  
- Human-in-the-Loop fallback  

---

## Key Decisions

- use asynchronous request-reply pattern  
- decouple API from RPA execution  
- introduce controlled fallback for failures  

---

## Trade-Offs

- API responsiveness improves, but backend remains slow  
- operational complexity increases  
- manual intervention still required  

---

## Outcome

- enables API-based access  
- improves scalability  
- maintains controlled execution  

---

## Strategic View

A controlled bridge between legacy UI systems and modern API expectations.
