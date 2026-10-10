# Qhawe — Helping township shops keep trading when the network dies

**AI-RAN edge agent that turns tower battery telemetry into early warnings and payment resilience for township merchants.**

> The schedule tells you when the power will leave.  
> Qhawe tells you when the network will fail.

---

### The Problem
When a cell tower’s batteries run low during prolonged or unscheduled power issues, the network dies — often while shops still have power. Merchants lose the ability to accept MoMo and card payments with no advance warning.

### The Solution
Qhawe runs at the edge (tower / AI-RAN) and:
- Predicts site-level network loss using battery health, temperature, electrical load, weather and site traffic
- Sends vernacular SMS/USSD alerts to paired merchants
- Triggers a **proposed** offline payment queue mode in paired apps (capped intents, encrypted, short TTL)

Focus is on **unscheduled events** and **per-site battery degradation** — things public load-shedding schedules cannot see.

---

### Current Status (MTN Group Technology Hackathon 2026)
- Full technical proposal completed
- Site simulator + explainable models (XGBoost) in development
- Live demo harness (SMS + merchant PWA) planned

### PoC Roadmap
| Phase | Timeline | Focus |
|-------|----------|-------|
| Phase 1 | Weeks 1–3 | Grounded site simulator + time-to-depletion models |
| Phase 2 | Weeks 4–5 | Live SMS demo + offline queue simulation in PWA |
| Phase 3 | Parallel | Merchant interviews & validation |

### Tech Stack
- **Edge**: Jetson-class / O-RAN-aligned
- **Models**: XGBoost (explainable)
- **Alerts**: Africa’s Talking (SMS + USSD)
- **Interface**: Progressive Web App (Merchant dashboard)
- **Data**: Synthetic + public sources initially

### Why This Matters for MTN
- Protects MoMo transaction volume during grid events
- Creates a practical new feature for SME customers
- Demonstrates real AI-RAN value beyond pure connectivity
- Aligns with Ambition 2030 (Connectivity + Fintech + Digital Infrastructure)

---

### Project Structure

qhawe-ai-ran/
├── docs/               # Architecture & detailed plans
├── simulator/          # Phase 1 – Site & battery simulator
├── models/             # Training notebooks & models
├── demo/               # Phase 2 – PWA & SMS demo
└── notebooks/          # Exploratory analysis

---

### License
MIT License
