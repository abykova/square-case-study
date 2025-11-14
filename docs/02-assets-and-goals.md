# 02 — Assets and Security Goals

## 1. Assets

| ID  | Asset                          | Description                                        | Owner         |
|-----|--------------------------------|----------------------------------------------------|---------------|
| A01 | Customer PII                   | Name, phone, email, national ID, address          | Business / CISO |
| A02 | Account and transaction data   | Balances, transaction history                      | Business / CISO |
| A03 | Authentication credentials     | Passwords, PINs, device binding data, tokens       | CISO / Security |
| A04 | Mobile API Gateway             | Public API surface exposed to the Internet         | Architect       |
| A05 | Back-Office Web UI             | Internal UI for support and operations             | Operations     |
| A06 | Integration with Core Banking  | Connectivity and data flows to core system         | Architect      |

## 2. High-Level Security Goals

- **G1 — Protect confidentiality of customer PII (A01)**  
  PII must not be disclosed to unauthorized parties, both externally and internally.

- **G2 — Ensure integrity of account and transaction data (A02)**  
  Attackers must not be able to create, modify, or delete transactions improperly.

- **G3 — Ensure strong authentication and session management (A03)**  
  Only legitimate customers can access their accounts; sessions cannot be hijacked easily.

- **G4 — Limit exposure of the Mobile API Gateway (A04)**  
  The API attack surface must be minimized and protected using strong access control,
  rate limiting, and input validation.

- **G5 — Protect internal Back-Office access (A05)**  
  Only authorized staff can use back-office functions; actions are traceable.

- **G6 — Maintain availability of critical services (A02, A04)**  
  Mobile banking must be available to customers with acceptable SLAs.
