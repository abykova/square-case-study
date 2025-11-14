# 00 — Project Context

## 1. Business Overview

**Project name:** SecurePay Mobile  
**Domain:** Retail mobile banking  

SecurePay Mobile allows individual customers to:
- View balances and recent transactions
- Send money to other customers and external accounts
- Pay for utilities and mobile services
- Manage their cards (set limits, block card, request new card)

## 2. High-Level Architecture

- **Mobile App (iOS / Android)** — used by customers
- **Mobile API Gateway** — public REST API over HTTPS
- **Core Banking System** — existing internal system
- **Payment Providers** — external partners
- **Back-Office Web UI** — used by support and operations

See `diagrams/context-diagram.png` for the system context.

## 3. Stakeholders

- **Business Owner** — responsible for product vision and revenue
- **CISO / Security** — responsible for security and compliance
- **Architect / Tech Lead** — responsible for technical design
- **Developers** — implement the system
- **Operations / DevOps** — deploy and run the system
- **Support / Back-Office** — use back-office UI to work with customers
- **End Users (Customers)** — use mobile app
