# 10 — Traceability Matrix

This document provides a traceability view that links:

- **Security Goals (G)**  
- **Assets (A)**  
- **Risks (R)**  
- **Security Requirements (SR)**  

The goal is to show how high-level business/security goals are supported
by concrete security requirements, and how those requirements mitigate
specific risks for specific assets.

---

## 1. Security Goals → Assets → Risks → Requirements

This table is **goal-centric**: for each security goal, we show which
assets it covers, which risks are relevant, and which requirements
primarily support that goal.

| Goal ID | Security Goal                                               | Assets            | Related Risks                 | Supporting Requirements (SR)                                                                                         |
|---------|-------------------------------------------------------------|-------------------|-------------------------------|----------------------------------------------------------------------------------------------------------------------|
| G1      | Protect confidentiality of customer PII                     | A01               | R-01, R-03, R-04, R-07, R-08  | SR-01, SR-02, SR-03, SR-05, SR-07, SR-08, SR-10, SR-11, SR-12, SR-13, SR-14, SR-15, SR-19, SR-20, SR-21             |
| G2      | Ensure integrity of account and transaction data           | A02               | R-02, R-07                    | SR-06, SR-08, SR-09, SR-10, SR-11, SR-13, SR-14, SR-15, SR-20, SR-21                                                |
| G3      | Ensure strong authentication and session management         | A03               | R-01, R-08                    | SR-01, SR-02, SR-03, SR-04, SR-05, SR-11, SR-12, SR-13, SR-15                                                       |
| G4      | Limit exposure of the Mobile API Gateway                    | A04               | R-01, R-02, R-05, R-06        | SR-01, SR-02, SR-03, SR-06, SR-09, SR-10, SR-13, SR-15, SR-16, SR-17, SR-18                                        |
| G5      | Protect internal Back-Office access                         | A05               | R-03, R-07                    | SR-07, SR-08, SR-11, SR-13, SR-14, SR-15, SR-19, SR-20, SR-21                                                       |
| G6      | Maintain availability of critical services                  | A02, A04          | R-05, R-06                    | SR-16, SR-17, SR-18, SR-13, SR-15                                                                                   |

Notes:

- Assets are defined in `02-assets-and-goals.md`.
- Risks are defined in `04-risk-assessment.md`.
- Requirements are defined in `06-elicited-requirements.md` and prioritized in `08-prioritization.md`.

---

## 2. Risk-centric Traceability: Risks → Assets → Goals → Requirements

This section is **risk-centric**: for each risk, we show which assets
and goals it affects, and which requirements mitigate it.

### R-01 — Account Takeover via Stolen Credentials and Weak Authentication

- **Assets:** A01 (Customer PII), A02 (Account and transaction data), A03 (Authentication credentials), A04 (Mobile API Gateway)  
- **Goals:** G1, G2, G3, G4  
- **Mitigating Requirements:**
  - SR-01 — Enforce MFA for customers
  - SR-02 — Lock / slow down failed logins
  - SR-03 — Secure token generation & lifetime
  - SR-04 — Secure session termination
  - SR-05 — Strong hashing for passwords/PINs
  - SR-10 — TLS for data in transit
  - SR-13 — Log security-relevant events
  - SR-15 — Security alerts for suspicious activity
  - SR-16 — Rate limiting for public APIs

---

### R-02 — Unauthorized Transactions Due to Weak/Missing Authorization

- **Assets:** A02 (Account and transaction data), A04 (Mobile API Gateway)  
- **Goals:** G2, G4  
- **Mitigating Requirements:**
  - SR-06 — Server-side authorization checks
  - SR-08 — Elevated privileges for high-risk Back-Office operations
  - SR-09 — Prevent IDOR / cross-tenant access
  - SR-13 — Log security-relevant events
  - SR-15 — Security alerts for suspicious activity
  - SR-20 — Detailed audit trails for Back-Office

---

### R-03 — PII Exposure via Back-Office Misuse or Excessive Access

- **Assets:** A01 (Customer PII), A02 (Account and transaction data), A05 (Back-Office Web UI)  
- **Goals:** G1, G2, G5  
- **Mitigating Requirements:**
  - SR-07 — RBAC for Back-Office
  - SR-08 — Elevated privileges for high-risk Back-Office operations
  - SR-11 — Encryption & access control for data at rest
  - SR-13 — Log security-relevant events
  - SR-14 — Protect and retain logs
  - SR-15 — Security alerts for suspicious activity
  - SR-19 — Network restriction for Back-Office access
  - SR-20 — Detailed audit trails for Back-Office
  - SR-21 — Extra confirmation for high-risk Back-Office operations

---

### R-04 — Interception or Tampering with Data in Transit

- **Assets:** A01 (Customer PII), A02 (Account and transaction data), A03 (Authentication credentials)  
- **Goals:** G1, G2, G3  
- **Mitigating Requirements:**
  - SR-10 — TLS 1.2+ for all channels
  - SR-13 — Log security-relevant events (for detection)
  - SR-15 — Security alerts for suspicious activity (for detection)

---

### R-05 — Unavailable Service Due to DoS on Public API

- **Assets:** A02 (Account and transaction data), A04 (Mobile API Gateway)  
- **Goals:** G4, G6  
- **Mitigating Requirements:**
  - SR-16 — Rate limiting for public APIs
  - SR-17 — Capacity and scaling mechanisms
  - SR-18 — Detect/block abusive automated traffic
  - SR-13 — Log security-relevant events
  - SR-15 — Security alerts for suspicious activity

---

### R-06 — Abuse of Mobile API by Automated Scripts/Bots

- **Assets:** A02 (Account and transaction data), A04 (Mobile API Gateway)  
- **Goals:** G4, G6  
- **Mitigating Requirements:**
  - SR-16 — Rate limiting for public APIs
  - SR-18 — Detect/block abusive automated traffic
  - SR-13 — Log security-relevant events
  - SR-15 — Security alerts for suspicious activity

---

### R-07 — Insufficient Logging and Monitoring (Undetected Incidents)

- **Assets:** A01 (Customer PII), A02 (Account and transaction data), A03 (Authentication credentials), A05 (Back-Office Web UI)  
- **Goals:** G1, G2, G5  
- **Mitigating Requirements:**
  - SR-13 — Log security-relevant events
  - SR-14 — Protect and retain logs
  - SR-15 — Security alerts for suspicious activity
  - SR-20 — Detailed audit trails for Back-Office
  - SR-21 — Extra confirmation and flagging of high-risk Back-Office actions

---

### R-08 — Weak Storage Protection of Authentication Data and Tokens

- **Assets:** A03 (Authentication credentials), A01 (Customer PII)  
- **Goals:** G1, G3  
- **Mitigating Requirements:**
  - SR-03 — Secure token generation & lifetime
  - SR-05 — Strong hashing for passwords/PINs
  - SR-11 — Encryption & access control for data at rest
  - SR-12 — Secure storage on mobile

---

## 3. Summary and Usage

This traceability matrix can be used to:

- Demonstrate to stakeholders **how business/security goals are covered** by concrete controls.
- Validate that each identified **risk has one or more mitigating requirements**.
- Support **impact analysis**:  
  - If a requirement changes or is not implemented, we can see which risks and goals are affected.
  - If a new risk appears, we can check whether existing requirements are sufficient.

In a real project, this matrix would typically be:

- Maintained in a requirements management tool or spreadsheet
- Linked with **test cases** and **verification activities**
- Updated as architecture, risks, or requirements evolve
