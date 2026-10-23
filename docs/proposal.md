**Qhawe — Helping township shops keep trading when the network dies**

*The schedule tells you when the power will leave.  
Qhawe tells you when the network will fail.*

**Track:** Primary — AI & AI-RAN | Secondary — Fintech & Digital Infrastructure  


---

### 1. Executive Summary

When power dips in a township, a merchant often loses the ability to take payment before the shop itself goes dark. The POS freezes, MoMo fails, and customers leave or switch to cash. Frequently the shop still has light, but the network has already died because the tower’s batteries could not hold. Nobody warned her.

Qhawe is an AI agent that runs at the cell tower and monitors the one signal merchants cannot see: how long the tower’s own batteries will last. Using battery health, temperature, electrical load and local conditions, it estimates when the network is likely to fail — especially during unscheduled faults and voltage sags that public schedules miss.

Before the network dies, Qhawe sends a clear vernacular alert and moves paired payment apps into a proposed offline mode: signed phone-to-phone vouchers that work with zero connectivity, plus queued payment intents that sync when the network returns. Because the switch happens ahead of the predicted failure window, the customer never has to hear “network is down, come back later.”

For MTN this means protected MoMo volume, a practical new resilience feature for SME customers, a credible path to true offline mobile money, and a concrete AI-RAN use case that serves both the network and the community around it.

---

### 2. The Real Problem

Small merchants face two distinct failure modes with no advance warning:

1. The shop loses power (local fault or load reduction) — POS and phone die, trading stops.  
2. The area loses connectivity when a tower’s batteries deplete after a long or repeated outage. The shop may still have light, but payments cannot authorise or sync. This is the larger blast radius and the one only a tower-side view can predict.

Public load-shedding schedules do not help with degraded batteries, transformer faults or local load reduction. Power remains the weak link in MTN’s platform strategy of connectivity + fintech + digital infrastructure.

---

### 3. How It Works

**3.1 Signal chain and core insight**  
Tower battery telemetry (voltage, current, temperature, state-of-charge trend) + load stage + weather + site traffic → edge prediction model → action engine.

A tower’s design battery autonomy is typically 4–8 hours, but degraded batteries after repeated outages fall well below that. Qhawe estimates, per site and per event, whether the tower will survive — and if not, when it will die. This gives real minutes-to-hours of lead time that no merchant-side app can see.

**3.2 Pairing**  
Merchants register via the PWA. The phone reports its serving cell ID, mapped against the pinned business location and re-checked periodically. In the PoC, customer alerts go to the merchant’s opted-in list of regulars. At production scale, MTN could extend this to network-initiated SMS targeted at subscribers served by the affected site.

**3.3 Action engine (before predicted network loss)**  
- Vernacular SMS and USSD alerts (native-speaker reviewed).  
- Proposed hybrid offline mode:  
  1. **Signed vouchers** (phone-to-phone, zero connectivity) — cryptographically signed QR containing amount, merchant ID, unique serial, 24-hour expiry and daily cap. First settlement wins; duplicates are rejected.  
  2. **Queued intents** (app-to-cloud) — capped, short-TTL intents that reconcile automatically on reconnect.  
- Merchant PWA showing risk level, queued amount and sync status.

Proposed risk controls (subject to MoMo approval): per-transaction and per-merchant caps, 24-hour TTL, device attestation, AES-256 at rest, POPIA-aligned. No card data touches Qhawe.

**3.4 Honest scope**  
- Scheduled load-shedding is public data. Qhawe targets unscheduled events and per-site battery health.  
- Offline mechanisms are proposed and require MTN API + risk sign-off. The PoC demonstrates the patterns with simulated flows.

---

### 4. Why the Tower — and Why This Is AI-RAN

- **Moat**: Battery telemetry is invisible to merchant apps, banks and Eskom. This is a genuine tower-side product.  
- **Infrastructure reality**: Telemetry access runs through MTN site-power systems or a towerco partnership — standard commercial territory.  
- **AI-RAN alignment**: Edge intelligence delivers low latency and resilience during degraded backhaul. Production would be O-RAN-aligned; the PoC runs on a Jetson-class device or laptop.  
- **Fintech upside**: Protects transaction volume and creates cleaner trading records that can later support merchant credit scoring. Positions MoMo for a capability no mainstream African wallet has shipped.  
- **Digital infrastructure**: Turns each tower into a local resilience hub.

---

### 5. PoC Plan (Solo, Realistic)

**Phase 1 (Weeks 1–3)** — Site simulator grounded in published parameters + XGBoost models for time-to-depletion and outage probability. Benchmarked against schedule-only and always-alert baselines.  

Success targets:  
- Median time-to-depletion error ≤ 30 minutes  
- ≥ 60% of unscheduled events detected with ≥ 15 minutes lead time  
- ≥ 70% alert precision  

**Phase 2 (Weeks 4–5)** — Live demo: real SMS/USSD via Africa’s Talking + signed-voucher exchange between two handsets with zero connectivity, followed by settlement on reconnect. Serving-cell pairing and offline-mode pre-activation included.

**Phase 3 (Parallel)** — 5–10 merchant interviews to validate the problem, messaging and willingness to pay. No field pilot claimed at this stage.

**Needed from MTN**  
1. Conversation about MoMo Open API sandbox access  
2. 10–20 site-days of anonymised power/telemetry data (or towerco introduction)  
3. Technical mentorship  

---

### 6. Impact & Business Model

- **Merchant**: Fewer lost sales, better customer experience. Indicative pricing R49/month via MoMo (to be validated).  
- **MTN**: Recovered transaction volume, SME bundle differentiator, first-mover path on offline mobile money, flagship AI-RAN story aligned with Ambition 2030, portable to other markets.  
- **Platform vision** (kept secondary): The same edge-resilience engine can later support clinics, estates and agriculture, turning towers into proactive resilience hubs.

---


*Africa should not simply follow the next wave of technology. We can host it where the problem lives: at the tower, where power, payment and people meet.*

---

**APPENDIX — NOT FOR FORM SUBMISSION**  
(Use only for pitch preparation and Q&A)

**How do you know who to alert?**  
Merchants pair via serving cell ID + pinned location. PoC uses the merchant’s opted-in regulars; at scale MTN could use cell-targeted network SMS.

**What if the merchant’s phone is also dead?**  
That is the shop-power-loss case. SMS may be delayed; vouchers and queued intents reconcile whenever any device returns online. Primary case is tower-side failure (shop has power, network does not).

**Shops lose power even when towers don’t.**  
True. Alerts and queueing still help, but the tower-side case is the bigger blast radius and the unique opportunity.

**Why not just push EskomSePush?**  
Schedules do not reveal per-site battery health, transformer faults or load reduction.

**Doesn’t MoMo already do offline payments?**  
No. Current MoMo QR and USSD require live connectivity. True offline exists only in niche pilots.

**Can you authorise payments offline?**  
No. Vouchers and queued intents are proposed, capped, time-limited mechanisms that reconcile on reconnect, subject to MTN approval.

**How do you prevent double-spending?**  
Unique serials, first-settlement-wins rule, daily caps and 24-hour TTL.

**What if the model cries wolf?**  
Confidence thresholds, alert caps, merchant opt-in, and precision measured as a headline PoC metric (≥70%).
