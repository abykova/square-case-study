# SQUARE Security Requirements Case Study — SecurePay Mobile

This repository is a **case study of applying the SQUARE (Security Quality Requirements Engineering) methodology** to a fictitious mobile banking application called **SecurePay Mobile**.

The goal is to demonstrate how I approach **security requirements engineering** as an Application Security Engineer / Security Architect.

---

## Scenario

**SecurePay Mobile** is a retail mobile banking application that allows customers to:

- View account balances and transactions
- Initiate P2P transfers
- Pay for utilities and mobile services
- Manage cards (limits, blocks, new card requests)

The system exposes:
- A public **Mobile API** (REST/JSON over HTTPS)
- An internal **Back-Office Web UI** for support and operations
- Integrations with **core banking** and **payment providers**

The repository focuses on **security requirements**, not on implementation.

---

## SQUARE Process Coverage

This case study follows the 9 SQUARE steps:

1. **Agree on definitions** — shared security glossary for all stakeholders  
2. **Identify assets and security goals** — what we protect and why  
3. **Develop supporting artifacts** — context diagrams, misuse cases, templates  
4. **Perform risk assessment** — risk scenarios based on threats and business impact  
5. **Select elicitation techniques** — workshops, checklists, reusable requirements  
6. **Elicit security requirements** — initial list of security requirements  
7. **Categorize requirements** — by level (system / software / infrastructure) and type  
8. **Prioritize requirements** — based on risk and business constraints  
9. **Requirements inspection** — review and refinement of the requirements

Each step is documented in `docs/0X-*.md`.

---

## Repository Structure

- `docs/00-context.md` — project context and stakeholders  
- `docs/01-glossary.md` — security glossary used in this case  
- `docs/02-assets-and-goals.md` — identified assets and security goals  
- `docs/03-artifacts.md` — supporting artifacts (diagrams, misuse cases, templates)  
- `docs/04-risk-assessment.md` — risk register and approach  
- `docs/05-elicitation-techniques.md` — chosen elicitation techniques  
- `docs/06-elicited-requirements.md` — initial security requirements list  
- `docs/07-categorization.md` — categorization of requirements  
- `docs/08-prioritization.md` — prioritization based on risk  
- `docs/09-inspection.md` — inspection findings and decisions  
- `docs/10-traceability-matrix.md` — mapping risks ↔ requirements ↔ controls  
- `diagrams/` — context and data flow diagrams

---

## What this demonstrates

This case study demonstrates my ability to:

- Structure a **security requirements process** using an industry methodology (SQUARE)
- Translate **business goals and risks** into concrete, testable security requirements
- Communicate security decisions in a way that is understandable for
  both **technical** and **non-technical** stakeholders
- Integrate security requirements into an **SDLC / DevSecOps** process (backlog-driven)

---

## Notes

- The project and data are **fictitious** and created purely for educational purposes.
- The methodology is based on SQUARE by the Software Engineering Institute (SEI) at Carnegie Mellon University.
