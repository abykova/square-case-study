# 08 — Prioritization of Security Requirements

This document prioritizes the security requirements (SR-01 … SR-21)
based on the risk assessment from `04-risk-assessment.md`.

The goal is to decide **what must be implemented before go-live**,
what should follow in the first iterations, and what can be treated
as longer-term hardening.

---

## 1. Priority Levels and Criteria

We use three priority levels:

- **Must-have**
  - Required to reduce **Critical** and **High** risks to an acceptable level
  - Blocking for initial production go-live
- **Should-have**
  - Important for overall security posture
  - Should be implemented in early iterations (e.g., first few releases)
  - Typically addresses **Medium** risks or further reduces High risks
- **Nice-to-have**
  - Hardening and optimization
  - Implemented when capacity allows
  - Usually reduces residual risk, improves UX or detection depth

Prioritization is driven primarily by:
- Risk level (Critical / High / Medium) from `04-risk-assessment.md`
- Impact on CIA (confidentiality, integrity, availability)
- Practicality for initial implementation

---

## 2. Prioritized List of Requirements

### 2.1 Must-have Requirements

These requirements are considered **mandatory before initial production launch**.

| ID    | Priority  | Linked Risks                 | Rationale (short)                                                                 |
|-------|-----------|-----------------------------|-----------------------------------------------------------------------------------|
| SR-01 | Must-have | R-01                        | MFA is key control against account takeover (Critical risk).                     |
| SR-02 | Must-have | R-01                        | Brute-force and credential stuffing protection is essential for login security.  |
| SR-03 | Must-have | R-01, R-08                  | Secure tokens and lifetimes reduce session hijacking and token abuse.            |
| SR-04 | Must-have | R-01                        | Proper session termination limits exposure time for compromised sessions.         |
| SR-05 | Must-have | R-08                        | Strong hashing is baseline for protecting authentication secrets.                |
| SR-06 | Must-have | R-02                        | Server-side authorization checks are critical to prevent unauthorized operations.|
| SR-07 | Must-have | R-03                        | RBAC for Back-Office is essential to limit internal access to PII and accounts.  |
| SR-09 | Must-have | R-02                        | Prevents IDOR and cross-tenant access, central for transaction integrity.        |
| SR-10 | Must-have | R-04                        | TLS for all channels is a basic requirement for data in transit protection.      |
| SR-11 | Must-have | R-03, R-08                  | Encryption and access control for data at rest are required for PII and secrets. |
| SR-13 | Must-have | R-01, R-02, R-03, R-07      | Logging of security events is necessary for detection and incident response.     |
| SR-14 | Must-have | R-07                        | Protecting logs is critical for reliable investigation and compliance.           |
| SR-15 | Must-have | R-01, R-03, R-07            | Security alerts enable timely reaction to high-impact incidents.                 |
| SR-16 | Must-have | R-01, R-05, R-06            | Rate limiting protects against DoS and reduces brute-force/abuse feasibility.    |
| SR-19 | Must-have | R-03                        | Network restriction for Back-Office significantly reduces internal exposure.     |
| SR-20 | Must-have | R-03, R-07                  | Detailed audit trails are required for accountability and detecting misuse.      |

---

### 2.2 Should-have Requirements

These requirements are **highly recommended** and should be delivered
in the first iterations after initial go-live (or together with Must-have
if feasible).

| ID    | Priority   | Linked Risks       | Rationale (short)                                                                |
|-------|------------|-------------------|----------------------------------------------------------------------------------|
| SR-08 | Should-have| R-02, R-03        | Additional protection for high-risk Back-Office actions lowers impact of misuse.|
| SR-12 | Should-have| R-08              | Secure on-device storage reduces risk from mobile device compromise.            |
| SR-17 | Should-have| R-05              | Scaling and capacity planning improve resilience to load and partial DoS.       |

---

### 2.3 Nice-to-have Requirements

These are **hardening and optimization measures**. They add valuable
defense-in-depth and UX improvements and can be implemented after
core controls are in place.

| ID    | Priority      | Linked Risks   | Rationale (short)                                                                |
|-------|---------------|---------------|----------------------------------------------------------------------------------|
| SR-18 | Nice-to-have  | R-05, R-06    | More advanced detection/blocking of abusive automated traffic (beyond basic rate limiting). |
| SR-21 | Nice-to-have  | R-03, R-07    | Extra confirmation and highlighting of high-risk Back-Office actions; improves safety and audit clarity. |

> Note: Although SR-18 and SR-21 are classified as Nice-to-have here,
> in a real project they might be raised to **Should-have** depending on
> regulatory requirements, threat landscape, and operational capabilities.

---

## 3. Risk Coverage Overview

This section summarizes how the prioritized requirements address the risks
identified in `04-risk-assessment.md`.

| Risk ID | Risk Level | Short Description                                              | Covered Primarily by (Must/Should)                              |
|--------:|------------|----------------------------------------------------------------|------------------------------------------------------------------|
| R-01    | Critical   | Account takeover via stolen credentials and weak auth          | SR-01 (M), SR-02 (M), SR-03 (M), SR-04 (M), SR-13 (M), SR-15 (M), SR-16 (M) |
| R-02    | High       | Unauthorized transactions due to weak/missing authorization   | SR-06 (M), SR-09 (M), SR-08 (S), SR-13 (M)                       |
| R-03    | High       | PII exposure via Back-Office misuse or excessive access       | SR-07 (M), SR-11 (M), SR-19 (M), SR-20 (M), SR-08 (S), SR-21 (N) |
| R-04    | Medium     | Interception/tampering of data in transit                     | SR-10 (M)                                                        |
| R-05    | Medium     | Unavailable service due to DoS on public API                  | SR-16 (M), SR-17 (S), SR-18 (N)                                  |
| R-06    | Medium     | Abuse of mobile API by automated scripts/bots                 | SR-16 (M), SR-18 (N)                                             |
| R-07    | High       | Insufficient logging/monitoring → undetected incidents        | SR-13 (M), SR-14 (M), SR-15 (M), SR-20 (M), SR-21 (N)            |
| R-08    | High       | Weak storage protection of auth data and tokens               | SR-03 (M), SR-05 (M), SR-11 (M), SR-12 (S)                       |

Legend:
- **M** — Must-have
- **S** — Should-have
- **N** — Nice-to-have

---

## 4. Summary

- The **Must-have** set is designed to reduce **Critical and High** risks (R-01, R-02, R-03, R-07, R-08)
  to an acceptable level before initial production launch.
- **Should-have** requirements mainly enhance protection against **Medium** risks (R-05, R-06)
  and further strengthen controls related to High risks.
- **Nice-to-have** requirements provide **defense-in-depth and usability improvements**
  and can be planned as hardening tasks once the core controls are in place.

This prioritization can be directly translated into a product backlog:
- Must-have → **blocking epics/stories for first release**
- Should-have → **early follow-up milestones**
- Nice-to-have → **hardening / improvement roadmap**
