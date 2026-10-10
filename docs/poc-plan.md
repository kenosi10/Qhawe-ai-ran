# Qhawe — Proof of Concept Plan

**Status:** Planned for MTN Group Technology Hackathon 2026  
**Approach:** Solo, realistic, and demonstrable

---

## Objective

Build and demonstrate a working end-to-end prototype that:

1. Predicts cell-site network loss caused by battery depletion and unscheduled power events
2. Delivers timely vernacular alerts
3. Shows a simulated offline payment queue and sync flow
4. Proves the core value of tower-side intelligence

The PoC prioritises honesty, measurability, and a live demo that judges can experience directly.

---

## Timeline Overview

| Phase       | Duration     | Focus                                      | Deliverable                          |
|-------------|--------------|--------------------------------------------|--------------------------------------|
| Phase 1     | Weeks 1–3    | Simulator + Prediction Models              | Working models + evaluation results  |
| Phase 2     | Weeks 4–5    | Live Demo Harness                          | SMS alerts + Merchant PWA demo       |
| Phase 3     | Parallel     | Merchant Validation                        | Interview insights                   |

---

## Phase 1 — Simulator + Models (Weeks 1–3)

### Goals
- Create a grounded site simulator
- Train explainable models for time-to-depletion and network-loss risk
- Benchmark against simple baselines

### Key Activities
- Build a site simulator using published parameters:
  - Battery specifications
  - Typical site electrical loads
  - Eskom load stage data (via free EskomSePush tier)
  - GSMA energy benchmarks
  - Weather and synthetic site traffic
- Train XGBoost models for:
  - Probability of network loss within a defined window
  - Estimated time-to-depletion
- Evaluate on held-out simulated scenarios

### Success Targets
| Metric                              | Target                          |
|-------------------------------------|---------------------------------|
| Median time-to-depletion error       | ≤ 30 minutes                    |
| Detection of unscheduled events     | ≥ 60% with ≥ 15 min lead time   |
| Alert precision                     | ≥ 70%                           |

### Baselines for Comparison
1. **Schedule-only alerting** (detects none of the unscheduled events)
2. **Always-alert** (maximum coverage, maximum false alarms)

---

## Phase 2 — Live Demo Harness (Weeks 4–5)

### Goals
Deliver a live, interactive demonstration that a judge can experience on their own phone.

### Demo Flow
1. Judge is registered as a paired merchant (using serving-cell pairing logic)
2. Operator flips the “grid” switch in the simulator
3. Battery runtime estimate updates in real time
4. Judge receives a real SMS alert on their phone
5. Judge queues a payment in the Merchant PWA
6. Power is restored
7. Payment syncs successfully

### Components
- **Alerts**: Africa’s Talking sandbox (real SMS + USSD check-in)
- **Merchant Interface**: Progressive Web App showing:
  - Current risk level
  - Queued amount
  - Sync status
- **Pairing**: Serving Cell ID + pinned business location (same logic planned for production)

---

## Phase 3 — Reality Check (Parallel)

### Goals
Validate the problem and solution direction with real users.

### Activities
- Conduct 5–10 design-partner interviews with township merchants
- Focus areas:
  - Frequency and impact of network-related payment failures
  - Reaction to early-warning alerts
  - Willingness to pay (indicative pricing: R49/month)
  - Feedback on alert language and timing

**Note:** No field pilot is claimed at the hackathon stage. A real pilot would follow MTN engagement.

---

## What is Needed from MTN to Progress Further

1. Conversation about MoMo Open API sandbox access
2. 10–20 site-days of anonymised power/telemetry data, **or** an introduction to the relevant towerco
3. Technical mentorship

---

## Success Criteria for the Hackathon

A successful PoC will demonstrate:

- Clear prediction capability on unscheduled events
- Measurable improvement over naive baselines
- A live, working alert on a real phone
- A believable offline-queue → sync payment flow
- Honest scoping (no overclaiming of production-ready MoMo integration or real telemetry access)

---

## Risks & Mitigations

| Risk                              | Mitigation                                      |
|-----------------------------------|-------------------------------------------------|
| Lack of real telemetry            | Use high-quality synthetic data + public sources |
| Model over-alerting               | Strict precision target (≥70%) + confidence thresholds |
| Demo technical issues             | Keep Phase 2 focused and rehearsable            |
| Merchant pairing complexity       | Start simple (Cell ID + location)               |
```
