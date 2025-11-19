# 03 — Supporting Artifacts for Security Requirements

This document describes the supporting artifacts used to apply the SQUARE methodology
to the SecurePay Mobile case study.

The goal is to provide enough structure and context so that security requirements
are not created in isolation, but are clearly linked to business context,
data flows, and misuse scenarios.

---

## 1. Context Diagram

**Purpose:**  
To show the system in its environment, including external actors and major integrations.

**Description (high-level):**

- **External Actors:**
  - Mobile Customer (iOS / Android)
  - Back-Office Operator (internal staff)
  - Payment Provider
  - Core Banking System
  - Push Notification Service

- **System:**
  - SecurePay Mobile App
  - Mobile API Gateway
  - Back-Office Web UI
  - Integration Layer (connectors to Core Banking and payment providers)

The context diagram illustrates:
- Which interfaces are exposed to the Internet
- Which components are internal only
- Trust boundaries between mobile clients, DMZ, and internal network

> See `diagrams/context-diagram.png` (placeholder for now).

---

## 2. Data Flow Diagram (DFD) — Level 1

**Purpose:**  
To understand how data (especially sensitive data) flows through the system
and where trust boundaries and processing steps are.

**Key data flows:**

- Customer authentication and session establishment
- Viewing balances and transactions
- Initiating a P2P transfer
- Card management operations (block/unblock, limits)

**Main elements:**

- Processes:
  - P1: Authentication Service
  - P2: Accounts and Transactions Service
  - P3: Payments / Transfers Service
  - P4: Card Management Service
  - P5: Back-Office Service

- Data Stores:
  - D1: Customer Profile and PII
  - D2: Account and Transaction Data
  - D3: Audit Logs

- External Entities:
  - E1: Customer Device
  - E2: Core Banking
  - E3: Payment Provider

> See `diagrams/dfd-level1.png` (placeholder for now).

---

## 3. Misuse Cases

**Purpose:**  
To explicitly model how an attacker might abuse the system, and use that to derive
security requirements and controls.

### 3.1 Misuse Case: Unauthorized Access to Account

- **Actor:** External attacker
- **Goal:** Gain access to another customer’s account
- **Main scenario:**
  1. Attacker obtains stolen credentials (phishing, credential stuffing).
  2. Attacker tries to log in to SecurePay Mobile.
  3. If MFA is weak or missing, attacker gains access to the victim’s data and actions.

- **Related threats:**
  - Credential stuffing
  - Brute force
  - Weak MFA strategy

- **Candidate mitigations:**
  - Strong MFA for all customers
  - Rate limiting and anomaly detection
  - Device binding / risk-based authentication

---

### 3.2 Misuse Case: Unauthorized Transaction Manipulation

- **Actor:** External attacker or malicious insider
- **Goal:** Create or modify a financial transaction without proper authorization
- **Main scenario:**
  1. Attacker gains access to a valid session or internal interface.
  2. Attacker crafts a request to initiate or modify a transaction.
  3. System does not verify authorization or integrity sufficiently.
  4. Money is transferred to an account controlled by the attacker.

- **Related threats:**
  - Broken access control (IDOR, missing checks)
  - Parameter tampering
  - API abuse

- **Candidate mitigations:**
  - Strong server-side authorization checks
  - Transaction signing / step-up verification
  - Detailed audit logging and alerting

---

### 3.3 Misuse Case: Data Exfiltration via Compromised Back-Office Account

- **Actor:** Malicious insider or attacker with compromised employee account
- **Goal:** Export large amounts of customer PII and transaction data
- **Main scenario:**
  1. Attacker logs in to Back-Office UI.
  2. Attacker uses bulk export or reporting features.
  3. System allows large, unmonitored exports of sensitive data.
  4. Data is exfiltrated and used for fraud or sold.

- **Related threats:**
  - Excessive privileges
  - Lack of monitoring and segmentation
  - Weak data loss prevention

- **Candidate mitigations:**
  - Least privilege for back-office roles
  - Monitoring and rate limiting for exports
  - Alerts on unusual access patterns

---

## 4. Security Requirements Template

To keep security requirements consistent and traceable, the following template is used:

- **ID:** Unique identifier (e.g., SR-01)
- **Title:** Short name describing the requirement
- **Description:** Detailed requirement statement
- **Level:** System / Software / Infrastructure
- **Category:** (Optional) e.g., Authentication, Authorization, Logging, Crypto
- **Linked Assets:** References to assets this requirement protects
- **Linked Risks:** References to risk IDs from `04-risk-assessment.md`
- **Rationale:** Short explanation why this requirement exists
- **Verification:** How the requirement will be verified (test, review, tool, etc.)

Example:

> **ID:** SR-01  
> **Title:** Strong Customer Authentication  
> **Description:** The system shall enforce multi-factor authentication for all customers accessing SecurePay Mobile.  
> **Level:** System  
> **Category:** Authentication  
> **Linked Assets:** A01, A02, A03  
> **Linked Risks:** R-01  
> **Rationale:** Reduce the likelihood of account takeover via stolen credentials.  
> **Verification:** Security testing, login flow review, configuration review.

---

## 5. Checklists and Reusable Requirements

**Sources used as reusable requirements/checklists:**

- OWASP ASVS (Application Security Verification Standard)
- Internal (hypothetical) security baseline for:
  - Authentication & session management
  - API security
  - Logging and monitoring
  - Data protection in transit and at rest

These sources are used during elicitation to ensure that common security controls
are not missed, even if they were not explicitly mentioned in stakeholder interviews.
