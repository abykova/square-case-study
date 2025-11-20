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
| SR-07 | RBAC for Back-Office
