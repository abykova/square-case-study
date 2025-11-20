# 07 — Categorization of Security Requirements

This document categorizes the elicited security requirements (SR-01 … SR-21)
by **implementation level** (System / Software / Infrastructure) and by their
primary **security objectives (CIA)** and **functional category**.

The goal is to make it easier to:
- Understand where each requirement will be implemented
- See which parts of the CIA triad each requirement primarily supports
- Group requirements for further design, implementation, and validation

---

## 1. Categorization by Level and Functional Category

The following table summarizes each requirement by its implementation level
and functional category.

| ID    | Short Name                                      | Level          | Functional Category        |
|-------|-------------------------------------------------|----------------|----------------------------|
| SR-01 | Enforce MFA for customers                       | System         | Authentication             |
| SR-02 | Lock / slow down failed logins                  | Software       | Authentication / Abuse     |
| SR-03 | Secure token generation & lifetime              | Software       | Authentication / Session   |
| SR-04 | Secure session termination                      | Software       | Session Management         |
| SR-05 | Strong hashing for passwords/PINs               | Infrastructure / Software | Crypto / Authentication |
| SR-06 | Server-side authorization checks                | Software       | Authorization / Access Ctrl|
| SR-07 | RBAC for Back-Office                            | System         | Authorization              |
| SR-08 | Elevated privileges for risky Back-Office ops   | System         | Authorization / Process    |
| SR-09 | Prevent IDOR / cross-tenant access              | Software       | Access Control             |
| SR-10 | TLS 1.2+ for all channels                       | Infrastructure | Data in Transit            |
| SR-11 | Encryption & access control for data at rest    | Infrastructure | Data at Rest               |
| SR-12 | Secure storage on mobile                        | Software       | Data Protection            |
| SR-13 | Log security-relevant events                    | Software       | Logging                    |
| SR-14 | Protect and retain logs                         | Infrastructure | Logging / Data Protection  |
| SR-15 | Security alerts for suspicious activity         | System         | Monitoring / Detection     |
| SR-16 | Rate limiting for public APIs                   | Infrastructure | Availability / Abuse       |
| SR-17 | Capacity and scaling mechanisms                 | Infrastructure | Availability               |
| SR-18 | Detect/block abusive automated traffic          | Infrastructure | Abuse Protection / WAF     |
| SR-19 | Network restriction for Back-Office access      | Infrastructure | Network Security           |
| SR-20 | Detailed audit trails for Back-Office           | Software       | Logging / Audit            |
| SR-21 | Extra confirmation for high-risk Back-Office ops| System         | Process / UX / Audit       |

---

## 2. Mapping to CIA Security Objectives

Each requirement also supports one or more of the CIA security objectives:

- **C — Confidentiality**
- **I — Integrity**
- **A — Availability**

In many cases, a requirement contributes to more than one objective.  
Below is the primary mapping.

| ID    | CIA Focus | Rationale (short)                                                                 |
|-------|-----------|------------------------------------------------------------------------------------|
| SR-01 | C, I      | Reduces account takeover → protects confidentiality and integrity of accounts     |
| SR-02 | C, I      | Limits brute-force abuse → protects accounts and data from unauthorized access    |
| SR-03 | C, I      | Secure tokens reduce risk of session hijacking and token abuse                    |
| SR-04 | C, I      | Proper session termination limits exposure window for stolen sessions             |
| SR-05 | C         | Strong hashing protects confidentiality of authentication secrets                  |
| SR-06 | I, C      | Ensures only authorized operations on accounts/transactions                       |
| SR-07 | C, I      | Limits internal access to PII and financial data                                  |
| SR-08 | C, I      | Controls high-risk internal actions that may impact data confidentiality/integrity|
| SR-09 | I, C      | Prevents cross-tenant access and IDOR-style manipulation                          |
| SR-10 | C, I      | Protects data in transit from eavesdropping and tampering                         |
| SR-11 | C, I      | Protects stored PII and secrets from unauthorized access or modification          |
| SR-12 | C         | Prevents leakage of secrets from insecure storage on the device                   |
| SR-13 | I (A\*)   | Supports detection and investigation of security incidents (indirectly CIA)       |
| SR-14 | C, I      | Protects log integrity/confidentiality → reliable forensic data                   |
| SR-15 | C, I, A   | Early detection of attacks impacting confidentiality, integrity, and availability |
| SR-16 | A, C\*    | Protects availability and makes brute force harder (indirectly protects C)        |
| SR-17 | A         | Directly targets availability under load and failures                             |
| SR-18 | A, C\*    | Reduces automated abuse affecting availability and potential data misuse          |
| SR-19 | C, I      | Isolates Back-Office from Internet → reduces exposure of internal data/systems    |
| SR-20 | I (C\*)   | Ensures reliable audit trail; indirectly supports accountability/confidentiality  |
| SR-21 | I, C      | Reduces risk of unauthorized or incorrect high-impact actions                     |

\*Пометка `C\*` или `A\*` означает, что влияние на эту цель **вторичное**, но присутствует.

---

## 3. Summary

- **System-level requirements** (SR-01, SR-07, SR-08, SR-15, SR-21) — задают общие правила и поведение, которые обычно оформляются как архитектурные решения и политики.
- **Software-level requirements** (SR-02, SR-03, SR-04, SR-06, SR-09, SR-12, SR-13, SR-20) — реализуются в коде и логике приложений (API, Back-Office, mobile app).
- **Infrastructure-level requirements** (SR-05, SR-10, SR-11, SR-14, SR-16, SR-17, SR-18, SR-19) — покрывают хранилища, сеть, конфигурацию платформ и сервисов.
