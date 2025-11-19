# 04 — Risk Assessment

This document summarizes the initial security risk assessment
for the SecurePay Mobile system.

The purpose is to identify and prioritize key risks so that
security requirements can be derived and justified.

---

## 1. Risk Assessment Approach

- **Method:** Lightweight, scenario-based risk assessment
- **Inputs:**
  - Business context and assets (see `02-assets-and-goals.md`)
  - Context and data flow diagrams (see `03-artifacts.md`)
  - Misuse cases
- **Scale:**
  - Likelihood (L): Low / Medium / High
  - Impact (I): Low / Medium / High
  - Risk Level: derived qualitatively (Low / Medium / High / Critical)

This is not a full enterprise risk management exercise,
but a focused assessment for the SecurePay Mobile case study.

---

## 2. Risk Register

| Risk ID | Scenario                                                                 | Asset(s)         | Likelihood | Impact   | Risk Level | Mitigation Idea (high-level)                                     |
|--------:|--------------------------------------------------------------------------|------------------|------------|----------|------------|-------------------------------------------------------------------|
| R-01    | Account takeover via stolen credentials and weak authentication          | A01, A02, A03    | High       | High     | Critical   | Enforce MFA, rate limiting, anomaly detection                    |
| R-02    | Unauthorized transactions due to missing or weak authorization checks   | A02              | Medium     | High     | High       | Strong server-side authorization, transaction validation          |
| R-03    | Exposure of PII via compromised Back-Office account or excessive access | A01, A02         | Medium     | High     | High       | Least privilege, RBAC, monitoring of data exports                |
| R-04    | Interception or tampering with sensitive data in transit                | A01, A02, A03    | Low        | High     | Medium     | TLS 1.2+, strong ciphers, HSTS, certificate pinning (optional)   |
| R-05    | Unavailable mobile banking service due to DoS on public API             | A02, A04         | Medium     | Medium   | Medium     | Rate limiting, WAF, autoscaling, monitoring                      |
| R-06    | Abuse of mobile API by automated scripts / bots                         | A02, A04         | Medium     | Medium   | Medium     | API keys/ OAuth, rate limiting, device fingerprinting            |
| R-07    | Insufficient logging and monitoring leading to undetected incidents      | A01, A02, A03    | Medium     | High     | High       | Centralized logging, security alerts, retention policies         |
| R-08    | Weak storage protection of authentication data and tokens               | A03              | Medium     | High     | High       | Secure storage, encryption, proper token lifecycle management    |

---

## 3. Notes on Selected Risks

### R-01 — Account Takeover

- **Why important:** Direct impact on customers, fraud potential, reputational damage.
- **Main drivers:** Credential reuse, phishing, lack of MFA, insufficient rate limiting.
- **Priority:** Critical — must be addressed before go-live.

### R-02 — Unauthorized Transactions

- **Why important:** Financial loss for bank and/or customers; potential regulatory issues.
- **Main drivers:** Missing server-side checks, relying only on client-side logic, IDOR.
- **Priority:** High — strong business and security impact.

### R-03 — PII Exposure via Back-Office

- **Why important:** Legal and regulatory impact (data protection laws), reputational damage.
- **Main drivers:** Over-privileged internal roles, lack of monitoring and segmentation.
- **Priority:** High — controls should be clearly defined and tested.

---

## 4. Mapping to Security Goals

Short mapping between risks and high-level security goals (from `02-assets-and-goals.md`):

- **G1 (Protect confidentiality of customer PII):**
  - Related risks: R-01, R-03, R-04, R-07, R-08

- **G2 (Ensure integrity of account and transaction data):**
  - Related risks: R-02, R-07

- **G3 (Strong authentication and session management):**
  - Related risks: R-01, R-08

- **G4 (Limit exposure of the Mobile API Gateway):**
  - Related risks: R-01, R-02, R-05, R-06

- **G5 (Protect internal Back-Office access):**
  - Related risks: R-03, R-07

- **G6 (Maintain availability of critical services):**
  - Related risks: R-05, R-06
