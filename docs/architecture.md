# Qhawe System Architecture

## Overview

Qhawe is an AI-RAN edge agent designed to predict cell-site network loss caused by battery depletion and unscheduled power events, then deliver timely warnings and payment resilience actions to township merchants.

The system prioritises:
- Early detection using tower-side signals (not publicly available)
- Low-latency decision making at the edge
- Practical value for both MTN and small merchants

---

## High-Level Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     Data Sources                            │
│  • Tower Battery Telemetry (Voltage, Current, Temp, SOC)    │
│  • Electrical Load                                          │
│  • Eskom Load Stage (public)                                │
│  • Weather                                                  │
│  • Site Traffic / Network Demand                            │
└────────────────────────────┬────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────┐
│                 Edge Prediction Engine                      │
│  (Jetson-class / O-RAN-aligned edge compute)                │
│                                                             │
│  • Feature processing                                       │
│  • XGBoost models:                                          │
│      - Outage-during-window probability                     │
│      - Time-to-depletion estimation                         │
│  • Confidence scoring & thresholds                          │
└────────────────────────────┬────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────┐
│                     Action Engine                           │
│                                                             │
│  1. Merchant & Customer Alerts                              │
│     - SMS / USSD (vernacular)                               │
│                                                             │
│  2. Payment Resilience (Proposed)                           │
│     - Trigger offline queue mode in paired apps             │
│     - Capped payment intents                                │
│     - Encrypted local storage + auto-reconcile              │
│                                                             │
│  3. Merchant Dashboard (PWA)                                │
│     - Risk level, queued amount, sync status                │
└─────────────────────────────────────────────────────────────┘
```

---

## Key Design Principles

| Principle | Implementation |
|---------|----------------|
| **Tower-side advantage** | Uses battery telemetry that merchant-side apps cannot access |
| **Honesty in scope** | Focuses on unscheduled events + battery health (does not claim to predict public load-shedding schedules) |
| **Explainability** | Uses gradient-boosted models (XGBoost) rather than black-box deep learning |
| **Edge-first** | Inference runs at the site for low latency and resilience during degraded backhaul |
| **Progressive value** | Starts with alerts, then adds proposed payment resilience |

---

## Core Components

### 1. Prediction Models
- **Model type**: XGBoost (chosen for performance + explainability)
- **Outputs**:
  - Probability of network loss within a defined window
  - Estimated time-to-depletion (minutes)
- **Key features**: Battery SOC trend, voltage/current, temperature, electrical load, recent discharge rate, weather, site traffic

### 2. Pairing Mechanism
Merchants are mapped to towers via:
- Serving Cell ID reported by the merchant’s phone at registration
- Cross-checked against the merchant’s pinned business location
- Periodic re-validation as serving cells change

### 3. Alerting
- Primary channel: SMS (via Africa’s Talking in the PoC)
- Secondary: USSD
- Language support: Local languages (isiZulu, Sesotho, etc.) with native-speaker review

### 4. Payment Resilience (Proposed)
- Queued payment intents (not offline authorisation)
- Risk controls: per-transaction and per-merchant caps, 24-hour TTL
- AES-256 encryption at rest
- Automatic reconciliation on network recovery

---

## PoC vs Production

| Layer              | PoC                                      | Production Target                     |
|--------------------|------------------------------------------|---------------------------------------|
| Compute            | Jetson-class / Laptop                    | O-RAN-aligned edge                    |
| Data               | Synthetic + public sources               | Real tower battery telemetry          |
| Alerts             | Africa’s Talking SMS + USSD              | Operator-grade channels               |
| Payment Integration| Simulated queue in PWA                   | Proposed MoMo API integration         |
| Merchant Mapping   | Serving Cell ID + location               | Same + network-assisted targeting     |

---

## Security & Privacy Considerations
- No card data is processed or stored by Qhawe
- Payment intents are encrypted (AES-256)
- POPIA-aligned data handling
- Merchant opt-in for alerts
- Short data retention (TTL) on queued items

---

## Future Extensions
- Closed-loop energy recommendations back to the RAN
- Richer merchant analytics
- Credit-scoring signals derived from successful resilience events
- Multi-market adaptation (Nigeria, Ghana, Uganda, etc.)
```
