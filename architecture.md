Qhawe Architecture — AI-RAN Edge Agent
The schedule tells you when power leaves. Qhawe tells you when network dies.

Detailed Architecture
Simple Flow

1. Overview
Qhawe runs at the tower, not in the cloud. It turns tower battery telemetry into merchant resilience.

2. System Layers
Layer 1: Data Ingestion
Tower Battery Telemetry (proposed via MTN site-power systems / towerco): voltage, current, SOC %, temperature, charge/discharge rate
Eskom Load Stage: EskomSePush free tier
Weather API: temp, wind, rainfall
Site Traffic History: anonymized PRB utilization
Layer 2: Edge AI Layer
PoC: Jetson Orin Nano / laptop
Production: O-RAN-aligned edge app on AI Aerial stack
Models: XGBoost - time_to_depletion + outage_probability

Layer 3: Pairing Registry
Maps merchant_id ↔ cell_id via serving Cell ID at PWA registration + pinned location.

Layer 4: Action Engine
SMS via Africa's Talking, rate limited
Offline Queue: capped intents, AES-256, TTL 24h, auto-reversal
Layer 5: Merchant PWA
Dashboard, opt-in customer list (POPIA consent), offline-first

Layer 6: Proposed MoMo Open API
Sync & reconciliation, requires MTN risk sign-off

3. Tech Stack
Component	PoC	Production
Edge	Python, Docker	O-RAN app
Model	XGBoost	Same + drift monitoring
Alerts	Africa's Talking	MTN SMSC
PWA	React + Vite + IndexedDB	Same
4. KPIs
Median TTD error ≤30min
≥60% detection with ≥15min lead
≥70% precision
