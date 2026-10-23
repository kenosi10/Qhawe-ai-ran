# Qhawe — Helping township shops keep trading when the network dies

> The schedule tells you when the power will leave.
> Qhawe tells you when the network will fail.

**Track:** Primary — AI & AI-RAN | Secondary — Fintech & Digital Infrastructure
**Entrant:** Solo innovator, external

---

## 1. Executive summary

When the power dips in a township, a township merchant doesn't just lose electricity — she loses the ability to take payment. The POS freezes, MoMo stops working, and customers who came to buy either leave or pull out cash. Often the shop itself still has light, but the network has already died because the tower's batteries couldn't hold. Nobody warned her. She only finds out when the sale fails.

Qhawe is an AI agent that lives at the cell tower and watches the one thing merchants can't see: how long the tower's own batteries will last. By reading battery health, temperature, electrical load and local conditions, it estimates when the network is likely to go down — especially during the unscheduled faults and voltage sags that no public schedule can predict.

Before that happens, Qhawe sends a clear message in the local language: "Network may drop around 19:40. Finish payments now." At the same time it moves paired payment apps into a proposed offline mode — signed vouchers for phone-to-phone trading with zero connectivity, plus queued payment intents that sync the moment the network returns.

This is not another app that only works when everything is fine. It turns the tower itself into an early-warning system for the people who depend on it most.

For MTN it means fewer failed MoMo transactions, a practical new feature for SME customers, a shot at being the first African wallet with true offline payment capability — and a concrete example of AI-RAN that serves both the network and the community around it.

---

## 2. The real problem

Small merchants face two distinct failure modes, and today they have no warning for either:

1. **The shop loses power** (load reduction or local fault) — POS and phone die, trading stops.
2. **The area loses connectivity** (tower battery depletion after a long or repeated outage) — the shop may still have power, but no payments can sync or authorise. This is the larger blast radius and the one only a tower-side view can predict.

An illustrative merchant: a township merchant paying for data and MoMo rental loses trading hours every week to these events. Exact figures will be validated in customer interviews, but every township resident recognises the pattern: "Network is down, come back later / bring cash."

Power is the brake on MTN's own platform strategy. Connectivity, fintech and digital infrastructure as one platform fails at the last mile when the lights go out.

---

## 3. The solution — how it works

**Signal chain:**
Tower battery telemetry (voltage, current, temperature, state-of-charge trend) + Eskom load stage + weather + site traffic history (network demand through the tower) → Prediction model (edge) → Action engine

**Core insight (stated honestly):**
A tower's design battery autonomy is typically 4–8 hours, but degraded batteries after repeated outage cycles can fall well below that. The AI's job is to estimate, per site and per event, whether the tower will survive the outage — and if not, when it will die. This gives minutes-to-hours of real lead time. No merchant-side app can see this. The tower is the earliest shared vantage point for the whole area.

**Pairing — knowing who to warn:**
Qhawe is only useful if it knows which shops depend on which tower. Merchants pair at registration: when a shop owner registers on the merchant PWA, her phone reports its current serving cell ID, mapping her to the site most likely to serve her — confirmed against her pinned business location and re-checked periodically as serving cells change. Customer alerts in the PoC go to the merchant's own opted-in list of regulars, managed in the PWA. At production scale, a proposed MTN capability extends this to network-initiated SMS targeted at subscribers served by an affected site — something only the operator can do, and one more reason this is a tower-side product.

**Action engine (before predicted network loss):**

- **Merchant and customer alerts** via SMS and USSD in local languages (copy shown for illustration; final versions confirmed by native-speaker review before submission):
- isiZulu: *"Isaziso: Inethiwekhi ingavalwa ngo-19:40. Sicela uqede izinkokhelo ngaphambi kwalokho."*
- Sesotho: *"Temoso: Marang o ka tima ka 19:40. Qetellang tefo pele ho nako eo."*
- **Payment resilience — proposed hybrid offline mode** *(detailed below)*: true offline mobile money does not exist in any mainstream African wallet today. Qhawe proposes MoMo as the first, activated *proactively* by tower-side AI.
- **Merchant dashboard (PWA):** risk level, queued amount, sync status.

### Payment resilience — proposed hybrid offline mode

True offline mobile money does not exist in any mainstream African wallet today — MoMo QR payments require an app session, PIN and SMS code, and USSD requires a live connection to the gateway, which dies with the tower. Offline payment patterns exist only in niche products (NFC wearables, offline CBDC pilots). **That gap is the opportunity:** Qhawe proposes MoMo as the first major wallet with offline resilience, activated *proactively* when the model predicts a failure window (not after the network dies). Two complementary mechanisms:

1. **Proposed signed vouchers (phone-to-phone, zero connectivity).**
During the dead window the merchant's PWA generates a QR voucher for each sale: a cryptographically signed intent containing amount, merchant ID, voucher serial, expiry (24h), and a per-merchant daily cap. The customer's phone scans it — no network needed on either side. Vouchers settle at the MoMo cloud when *either* device regains connectivity. Double-spend is prevented by unique serials with one-time redemption: first settlement wins, duplicates are rejected and flagged. Because the tower is never in the transaction loop, this path works in both failure directions — tower dead but shop powered, or shop dead but network alive.
2. **Queued intents (app-to-cloud).**
When connectivity is intermittent rather than dead, payment intents are queued locally with capped amounts, short TTL, and automatic reconciliation — reversal on settlement failure. (Proposed MoMo Open API integration, subject to MTN risk sign-off; demonstrated with a simulated payment flow in the PoC.)

**Proposed risk controls (subject to MoMo approval):** per-transaction cap, per-merchant daily cap, 24-hour TTL, device attestation at pairing, AES-256 encryption at rest, POPIA-aligned — no card data touches Qhawe.

**Why proactive matters:** because Qhawe predicts the failure window, the merchant's app switches to offline mode *before* the network dies — customers are never told "network is down, come back later." We don't just survive the outage; **the customer never feels it.**

**Honest scope — what we are not claiming:**

- Scheduled load-shedding is public data (EskomSePush). Qhawe does not pretend to "predict" it with AI. Its value lies in unscheduled events and per-site battery modelling.
- Offline payments (vouchers and queueing) are **proposed integrations** requiring MTN's API and risk sign-off. The PoC demonstrates the interaction patterns with simulated payment flows.

---

## 4. Why the tower — and why this is AI-RAN

- **The moat:** Battery telemetry is invisible to merchant apps, banks or Eskom. Any competitor building this from the merchant side is blind to the earliest signal. This is a genuinely tower-side product.
- **Honest infrastructure note:** MTN sold its South African passive infrastructure to towercos, so production telemetry access runs through MTN's site-power systems or a towerco partnership — a conversation, not a blocker, and standard commercial territory.
- **AI-RAN alignment:** Intelligence at the tower delivers low latency, resilience during degraded backhaul, and a template for monetising edge AI beyond pure connectivity. Production deployment as an O-RAN-aligned edge application; PoC on a Jetson-class device or laptop.
- **Fintech:** Resilience is transactional, not just informational. Proposed signed vouchers protect revenue during full outages while queued intents handle intermittent connectivity. Both generate clean digital records of merchant trading even through grid events — records that can later support merchant credit scoring (a merchant who kept trading through last month's outages has demonstrated something a bank can't see). This also positions MTN to lead a capability no African operator has shipped: offline mobile money, proven in pilots but absent from every mainstream wallet.
- **Digital infrastructure:** Every tower becomes a small resilience hub for the local economy — infrastructure earning a second job.

---

## 5. PoC plan (solo, realistic)

**Phase 1 — Simulator + model (weeks 1–3)**
Site simulator grounded in published parameters (battery specs, typical site loads, Eskom schedules via free EskomSePush tier, GSMA energy benchmarks). XGBoost models for outage-during-window probability and time-to-depletion — explainable, no LSTM needed. KPIs measured on held-out simulated scenarios against two naive baselines: schedule-only alerting (which by definition detects none of these unscheduled events) and always-alert (maximum coverage, maximum false alarms).

Success targets (set now, measured honestly):

- Median time-to-depletion error ≤ 30 minutes on held-out scenarios
- ≥ 60% of simulated unscheduled network-loss events detected with ≥ 15 minutes of lead time
- ≥ 70% alert precision — at least 7 in 10 warnings correspond to a real event within the warning window

**Phase 2 — Live demo harness (weeks 4–5)**
Africa's Talking sandbox pushing real SMS and a USSD check-in to a judge's own phone, plus the proposed signed-voucher demo: two handsets (customer + merchant PWA) exchange a QR voucher with no connectivity, then settle on reconnect.

Demo flow: you're registered as the paired merchant (serving-cell pairing, exactly as designed for production) → flip the "grid" switch → watch the battery runtime estimate update → receive the alert on your handset → app pre-activates offline mode → scan a real voucher between the two phones → restore power → watch the voucher settle. (Queued-intent sync shown in the simulated MoMo sandbox alongside.)

**Phase 3 — Reality check (parallel)**
5–10 design-partner interviews with township merchants to validate the problem, alert copy and willingness to pay. No field pilot is claimed at this stage — that comes after MTN engagement.

**What I would need from MTN to go further:**

- A conversation about MoMo Open API sandbox access
- 10–20 site-days of anonymised power/telemetry data, or an introduction to the relevant towerco
- Technical mentorship

---

## 6. Impact & business model

**Merchant:** Fewer lost sales, better customer goodwill. Indicative pricing R49/month via MoMo — to be validated in interviews.

**MTN:**

- Recovered MoMo transaction volume during grid events
- A differentiating value-add for SME bundles
- First-mover position on true offline mobile money — a capability proven in pilots but shipped by no African operator
- A flagship AI-RAN monetisation story beyond connectivity, aligned with MTN's Ambition 2030 and its view of connectivity, fintech and digital infrastructure as one platform
- Portable to other grid-unstable markets (Nigeria, Ghana, Uganda, Cameroon)

**Continent:** Proof that Africa can host useful AI where the problem lives, not only consume it from the cloud.

**Platform vision (one paragraph, not a pivot):** The same edge-resilience engine — site telemetry → predicted failure → automated offline fallback — extends naturally beyond merchants: clinics caching vaccine-fridge telemetry, estates failing over CCTV backhaul, irrigation systems receiving pre-outage safe schedules. Qhawe's Phase 1 beachhead stays deliberately narrow, but the architecture turns each tower into a proactive resilience hub and lays groundwork for a broader network-as-a-service edge platform.

---


Africa should not simply follow the next wave of technology. We can host it where the problem lives: at the tower, where power, payment and people meet.

---

## Appendix — Questions we expect (for the pitch, not the form)

**"How do you know who to alert?"**
→ Merchants pair at registration via serving cell ID, verified against their pinned business location. In the PoC, customer alerts go to the merchant's opted-in regulars; at production scale, MTN could extend this to cell-targeted network SMS — an operator-only capability.

**"What if the merchant's phone is also dead?"**
→ That's the shop-power-loss case. The network typically stores SMS for a limited window (operator-dependent, ~24–72h) so delayed delivery is possible but not guaranteed; queued transactions and signed vouchers reconcile whenever *any* device comes back online. Our primary case is the tower-side failure — shop has power, network doesn't — where phones work and alerts arrive in time.

**"Shops lose power even when towers don't."**
→ True. Qhawe's alerts cover that case and queueing helps. But the tower-side case is the bigger blast radius and the one nobody else can see — that's where we build first.

**"Why not just push EskomSePush to merchants?"**
→ Schedules don't tell you if this tower's degraded batteries will survive the outage, and they say nothing about transformer faults or load reduction. That gap is our product.

**"Doesn't MoMo already do offline payments?"**
→ No. MoMo QR requires an app session, PIN and SMS code; USSD needs a live gateway connection, which dies with the tower. Offline payment exists only in niche products (NFC wearables, CBDC pilots). The PoC demonstrates the interaction pattern with simulated payment flows; production requires MTN's API and risk sign-off.

**"Can you authorise payments offline?"**
→ No — and we don't claim to. Vouchers and queued intents are *proposed* mechanisms: capped, time-limited, reconciled on reconnect, with risk controls MTN approves. We are explicit that this is a proposed integration.

**"How do you prevent double-spending on vouchers?"**
→ Each voucher carries a unique serial signed by the merchant's paired key. Settlement is one-time: the first redemption at the MoMo cloud wins, duplicates are rejected and flagged against the merchant's cap. Capped amounts and 24h TTL bound the maximum exposure of any abuse.

**"What if the model cries wolf?"**
→ Confidence thresholds, alert caps, merchant opt-in — and precision is a headline PoC metric (≥70% target), measured against both naive baselines.
