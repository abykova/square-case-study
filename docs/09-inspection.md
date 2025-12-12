# 09 — Requirements Inspection

This document summarizes the inspection (review) of the security requirements
for the SecurePay Mobile case study.

The goal of the inspection is to ensure that the elicited and prioritized
requirements are:

- Clear and unambiguous
- Consistent and non-contradictory
- Testable and verifiable
- Aligned with security goals and risk assessment

The inspection is performed as a **lightweight, structured review**
rather than a fully formal Fagan inspection.

---

## 1. Inspection Setup

### 1.1 Participants (Roles)

- **Security Engineer / AppSec** — owner of the security requirements,
  responsible for alignment with risks and best practices.
- **System Architect / Tech Lead** — validates feasibility and impact
  on architecture and design.
- **Backend / API Developer** — validates implementability and clarity
  for development teams.
- **Product Owner / Business Representative** — checks alignment
  with business goals and constraints.
- **(Optional) Compliance / Risk Representative** — checks alignment
  with relevant regulations and internal policies.

### 1.2 Scope of Inspection

Documents in scope:

- `02-assets-and-goals.md`
- `04-risk-assessment.md`
- `06-elicited-requirements.md`
- `07-categorization.md`
- `08-prioritization.md`
- `10-traceability-matrix.md`

---

## 2. Inspection Criteria

During the review, each requirement is checked at least against
the following questions:

1. **Clarity**
   - Is the requirement stated in a clear and unambiguous way?
   - Would two different developers interpret it the same way?

2. **Testability**
   - Can we define how to verify this requirement (test, review, tool)?
   - Is there a measurable or observable outcome?

3. **Consistency**
   - Does this requirement contradict any other requirement or assumption?
   - Is it consistent with the overall architecture and constraints?

4. **Relevance**
   - Is the requirement clearly linked to at least one risk, asset, or goal?
   - Does it provide tangible value for reducing risk or meeting compliance?

5. **Feasibility**
   - Is it realistic to implement this requirement in the given context?
   - Are there obvious blockers or dependencies that need to be captured?

---

## 3. Summary of Inspection Results

The following table summarizes the main findings
for groups of requirements rather than every requirement individually.

| Area / Group                            | Status        | Notes                                                                                   |
|----------------------------------------|--------------|-----------------------------------------------------------------------------------------|
| SR-01 … SR-05 (Authentication & Session) | Accepted with minor clarifications | Clarified wording around session timeout (SR-04) and hashing algorithm examples (SR-05). |
| SR-06 … SR-09 (Authorization & Access) | Accepted     | No major issues; requirements considered critical for transaction integrity.            |
| SR-10 … SR-12 (Data Protection)        | Accepted     | Added note to align encryption choices with organization-wide crypto policies.          |
| SR-13 … SR-15 (Logging & Monitoring)   | Accepted with follow-up | Identified need to define log retention period and alert thresholds later in design.    |
| SR-16 … SR-18 (Availability & Abuse)   | Accepted     | Marked SR-18 as “Nice-to-have” for first release; rest are required for go-live.        |
| SR-19 … SR-21 (Back-Office Security)   | Accepted     | Agreed that some details (e.g., network zones, specific roles) will be refined at design stage. |

Overall result:  
- **No requirements were rejected.**  
- Several requirements received **clarifications** and **implementation notes**,
  but their intent and priority did not change.

---

## 4. Examples of Clarifications

### 4.1 SR-04 — Secure Session Termination

- **Original:**  
  “The mobile app and API shall support secure session termination, including logout and automatic timeout after inactivity.”

- **Clarification:**  
  - Add a **configurable inactivity timeout value**, with recommended defaults.
  - Specify that session tokens must be invalidated on the server side
    when logout or timeout occurs.

> Implementation note: final timeout values and UX behavior (e.g., warnings)
> will be defined during detailed design.

---

### 4.2 SR-05 — Strong Hashing for Passwords/PINs

- **Original:**  
  “Authentication secrets (passwords, PINs) shall be stored using strong, salted, adaptive hashing algorithms (e.g., bcrypt/Argon2).”

- **Clarification:**  
  - Align with the organization’s crypto baseline (e.g., “use Argon2id
    with parameters at least as strong as the current baseline”).
  - Explicitly mention that secrets must **never** be stored in plaintext
    or with legacy unsalted hashes.

---

### 4.3 SR-13 / SR-14 — Logs and Retention

- **Original:**  
  “The system shall log all security-relevant events…”  
  “Logs shall be protected and retained…”

- **Clarification:**  
  - Add a note that **log retention periods** will depend on regulatory
    and internal requirements (e.g., 1–3 years).
  - Mention that logs should be centralized (e.g., SIEM or log platform),
    not only local.

---

## 5. Sample Verification Ideas (High-Level)

Although detailed test cases are outside the scope of this document,
the inspection included a quick check that requirements are **verifiable**.
Below are a few examples.

### SR-01 — Enforce MFA for Customers

- **Verification method:** Functional and security testing.
- **Example check:**
  - Attempt to log in from a new device with valid credentials.
  - Verify that a second factor (e.g., OTP, push) is required.
  - Attempt to disable MFA; verify that flow is protected and audited.

### SR-06 — Server-Side Authorization Checks

- **Verification method:** API security testing / code review.
- **Example check:**
  - Log in as Customer A, attempt to access accounts or transactions
    of Customer B by changing IDs in requests.
  - Verify that access is denied and the event is logged.

### SR-16 — Rate Limiting for Public APIs

- **Verification method:** Non-functional / stress testing.
- **Example check:**
  - Send a high volume of requests from one IP/account/device.
  - Verify that the API starts returning appropriate error codes
    (e.g., 429 Too Many Requests) and that service remains available
    for normal users.

---

## 6. Next Steps

Based on the inspection:

1. Clarifications documented above will be reflected in:
   - `06-elicited-requirements.md` (if wording changes are needed)
   - Design and architecture documents (for parameters and patterns)

2. The set of accepted requirements will be:
   - Translated into **epics and user stories** in the product backlog
   - Linked to **security test plans** and/or automation where applicable

3. Future changes to requirements (as the system evolves) should:
   - Go through at least a lightweight review
   - Update the traceability matrix and this inspection summary if impact is significant
