# 06 — Elicited Security Requirements

This document contains the initial list of security requirements
elicited for the SecurePay Mobile case study.

The requirements are derived from:
- Business context and security goals
- Assets and risk assessment
- Misuse cases and supporting artifacts
- Reusable baselines (e.g., OWASP ASVS-like controls)

Each requirement includes:
- ID
- Description
- Level (System / Software / Infrastructure)
- Category
- Linked Assets
- Linked Risks

---

## 1. Authentication and Session Management

| ID    | Description                                                                                                                     | Level       | Category        | Linked Assets    | Linked Risks       |
|-------|---------------------------------------------------------------------------------------------------------------------------------|-------------|-----------------|------------------|--------------------|
| SR-01 | The system **shall enforce multi-factor authentication (MFA)** for all customers accessing SecurePay Mobile.                    | System      | Authentication  | A01, A02, A03    | R-01               |
| SR-02 | The system **shall lock or slow down authentication attempts** after a configurable number of failed login attempts per account or device. | Software    | Authentication  | A01, A02, A03    | R-01               |
| SR-03 | All authentication and session tokens **shall be generated using cryptographically secure random functions** and have a limited lifetime. | Software    | Authentication  | A03               | R-01, R-08         |
| SR-04 | The mobile app and API **shall support secure session termination**, including logout and automatic timeout after inactivity.  | Software    | Session         | A02, A03         | R-01               |
| SR-05 | Authentication secrets (passwords, PINs) **shall be stored using strong, salted, adaptive hashing algorithms** (e.g., bcrypt/Argon2). | Infrastructure / Software | Crypto / Authentication | A03 | R-08 |

---

## 2. Authorization and Access Control

| ID    | Description                                                                                                              | Level       | Category          | Linked Assets | Linked Risks |
|-------|--------------------------------------------------------------------------------------------------------------------------|-------------|-------------------|---------------|-------------|
| SR-06 | The system **shall enforce server-side authorization checks** for all operations on accounts and transactions based on the authenticated customer identity. | Software    | Authorization     | A02           | R-02        |
| SR-07 | The Back-Office UI **shall implement role-based access control (RBAC)**, granting users only the minimum privileges required for their job. | System      | Authorization     | A01, A02      | R-03        |
| SR-08 | Sensitive actions in Back-Office (e.g., bulk export, manual transaction adjustment) **shall require elevated privileges** and explicit confirmation. | System      | Authorization     | A01, A02      | R-02, R-03  |
| SR-09 | The system **shall ensure that object identifiers used in APIs (e.g., account IDs, transaction IDs)** cannot be used to access resources belonging to other customers. | Software    | Access Control    | A02           | R-02        |

---

## 3. Data Protection

| ID    | Description                                                                                                              | Level        | Category         | Linked Assets   | Linked Risks          |
|-------|--------------------------------------------------------------------------------------------------------------------------|--------------|------------------|-----------------|-----------------------|
| SR-10 | All communication channels between the mobile app, API Gateway, and Back-Office **shall use TLS 1.2+ with strong ciphers**. | Infrastructure | Data in Transit | A01, A02, A03   | R-04                  |
| SR-11 | Sensitive data at rest (such as PII and authentication secrets) **shall be protected using appropriate encryption and access controls** at the storage layer. | Infrastructure | Data at Rest    | A01, A03        | R-03, R-08            |
| SR-12 | The mobile application **shall avoid storing long-term secrets in insecure storage locations** (e.g., plain-text files, unprotected preferences). | Software      | Data Protection | A03             | R-08                  |

---

## 4. Logging and Monitoring

| ID    | Description                                                                                                                  | Level       | Category      | Linked Assets      | Linked Risks          |
|-------|------------------------------------------------------------------------------------------------------------------------------|-------------|---------------|--------------------|-----------------------|
| SR-13 | The system **shall log all security-relevant events**, including authentication attempts, failed authorization checks, and sensitive operations in Back-Office. | Software    | Logging       | A01, A02, A03      | R-01, R-02, R-03, R-07 |
| SR-14 | Logs **shall be protected against unauthorized access and modification**, and retained for a period compliant with business and regulatory requirements. | Infrastructure | Logging    | A01, A02, A03      | R-07                  |
| SR-15 | The system **shall generate alerts for suspicious activities**, such as unusual login patterns, high-volume data exports, and repeated failed authentication attempts. | System      | Monitoring    | A01, A02, A03      | R-01, R-03, R-07      |

---

## 5. Availability and Abuse Protection

| ID    | Description                                                                                                                | Level       | Category        | Linked Assets | Linked Risks |
|-------|----------------------------------------------------------------------------------------------------------------------------|-------------|-----------------|---------------|-------------|
| SR-16 | The public APIs **shall enforce rate limiting** per IP, per account, and/or per device to reduce the risk of DoS and brute-force attacks. | Infrastructure | Availability / Abuse | A02, A04      | R-01, R-05, R-06 |
| SR-17 | The system **shall be deployed with capacity and scaling mechanisms** (e.g., load balancing, auto-scaling) to maintain availability under expected and peak loads. | Infrastructure | Availability  | A02, A04      | R-05        |
| SR-18 | The system **shall detect and block automated abusive traffic** (e.g., from known bad IPs, obvious bots) using appropriate security controls (such as WAF or API gateway features). | Infrastructure | Abuse Protection | A02, A04      | R-05, R-06 |

---

## 6. Back-Office Security

| ID    | Description                                                                                                                | Level       | Category      | Linked Assets | Linked Risks |
|-------|----------------------------------------------------------------------------------------------------------------------------|-------------|---------------|---------------|-------------|
| SR-19 | Back-Office access **shall be restricted to internal networks and VPN** and shall not be directly exposed to the public Internet. | Infrastructure | Network Security | A01, A02      | R-03        |
| SR-20 | The system **shall provide detailed audit trails** for Back-Office activities, including which operator performed which action and when. | Software    | Logging       | A01, A02      | R-03, R-07  |
| SR-21 | High-risk Back-Office actions (e.g., bulk exports, direct data changes) **shall require an additional confirmation step** and be clearly flagged in audit logs. | System      | Process / UX  | A01, A02      | R-03, R-07  |

---

## 7. Notes

- This list is not exhaustive; it represents the **initial elicited set** of security requirements.
- Further refinement, categorization, and prioritization are documented in:
  - `07-categorization.md`
  - `08-prioritization.md`
  - `09-inspection.md`
