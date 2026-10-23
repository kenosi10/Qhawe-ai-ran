Qhawe — Helping township shops keep trading when the network dies

MTN Group Technology Hackathon 2026 · Submission · AI & AI-RAN

     
 The schedule tells you when the power will leave. Qhawe tells you when the network will fail.

Track: Primary — AI & AI-RAN | Secondary — Fintech & Digital Infrastructure

1. Executive Summary

When power dips in a township, a merchant often loses the ability to take payment before the shop itself goes dark. The POS freezes, MoMo fails, and customers who came to buy either leave or pull out cash. Frequently the shop still has light, but the network has already died because the tower's batteries could not hold. Nobody warned her — she only finds out when the sale fails.

Qhawe is an AI agent that runs at the cell tower and monitors the one signal merchants cannot see: how long the tower's own batteries will last. Using battery health, temperature, electrical load and local conditions, it estimates when the network is likely to fail — especially during the unscheduled faults and voltage sags that public schedules miss.

Before the network dies, Qhawe sends a clear vernacular alert and moves paired payment apps into a proposed offline mode: signed phone-to-phone vouchers that work with zero connectivity, plus queued payment intents that sync the moment the network returns. Because the switch happens ahead of the predicted failure window, the customer never has to hear "network is down, come back later."

For MTN this means protected MoMo volume, a practical new resilience feature for SME customers, a credible path to true offline mobile money, and a concrete AI-RAN use case that serves both the network and the community around it.
2. The Real Problem

Small merchants face two distinct failure modes, and today they have no advance warning for either. First, the shop loses power through a local fault or load reduction: the POS and phone die and trading stops. Second — and this is the larger blast radius — the area loses connectivity when a tower's batteries deplete after a long or repeated outage. The shop may still have light, but no payment can authorise or sync. Only a tower-side view can predict this second mode, because the tower is the earliest shared vantage point for the whole area.

Public load-shedding schedules do not help with degraded batteries, transformer faults or local load reduction. Power remains the weak link in MTN's platform strategy: connectivity, fintech and digital infrastructure as one platform fails at the last mile when the lights go out.
3. How It Works
3.1 Signal chain and core insight

The signal chain is: tower battery telemetry (voltage, current, temperature, state-of-charge trend) plus load stage, weather and site traffic history, feeding an edge prediction model that drives an action engine. The core insight is stated honestly: a tower's design battery autonomy is typically 4–8 hours, but degraded batteries after repeated outage cycles fall well below that. Qhawe estimates, per site and per event, whether the tower will survive the outage — and if not, when it will die. That gives merchants real minutes-to-hours of lead time that no merchant-side app can see.
3.2 Pairing — knowing who to warn

Qhawe is only useful if it knows which shops depend on which tower. Merchants pair at registration: the phone reports its current serving cell ID, which is mapped against the pinned business location and re-checked periodically as serving cells change. In the PoC, customer alerts go to the merchant's own opted-in list of regulars, managed in the PWA. At production scale, a proposed MTN capability extends alerts to network-initiated SMS targeted at subscribers served by the affected site — an operator-only capability, and one more reason this is a tower-side product.
3.3 Action engine — before predicted network loss

Merchant and customer alerts go out by SMS and USSD in local languages, with copy confirmed by native-speaker review before submission. Because Qhawe predicts the failure window, the merchant's app switches to offline mode before the network dies — we do not just survive the outage, the customer never feels it. The proposed hybrid offline mode has two complementary mechanisms:

     Signed vouchers (phone-to-phone, zero connectivity). The merchant's PWA generates a cryptographically signed QR voucher per sale, containing the amount, merchant ID, a unique serial, 24-hour expiry and a per-merchant daily cap. The customer's phone scans it — no network needed on either side. Vouchers settle at the MoMo cloud when either device regains connectivity: first settlement wins, duplicates are rejected and flagged. Because the tower is never in the transaction loop, this path works in both failure directions — tower dead but shop powered, or shop dead but network alive.
     Queued intents (app-to-cloud). When connectivity is intermittent rather than dead, payment intents are queued locally with capped amounts and short TTL, and reconcile automatically on reconnect, with reversal on settlement failure.

A merchant dashboard (PWA) shows risk level, queued amount and sync status throughout.

Proposed risk controls (subject to MoMo approval): per-transaction cap, per-merchant daily cap, 24-hour TTL, device attestation at pairing, AES-256 encryption at rest, and POPIA-aligned handling — no card data touches Qhawe.
3.4 Honest scope

Scheduled load-shedding is public data (EskomSePush); Qhawe does not pretend to predict it with AI. Its value lies in unscheduled events and per-site battery modelling. The offline mechanisms — vouchers and queueing — are proposed integrations that require MTN's API and risk sign-off. The PoC demonstrates the interaction patterns with simulated payment flows.
4. Why the Tower — and Why This Is AI-RAN

     The moat: battery telemetry is invisible to merchant apps, banks and Eskom. Any competitor building this from the merchant side is blind to the earliest signal — this is a genuinely tower-side product.
     Infrastructure reality: MTN sold its South African passive infrastructure to towercos, so production telemetry access runs through MTN's site-power systems or a towerco partnership — a conversation, not a blocker, and standard commercial territory.
     AI-RAN alignment: intelligence at the tower delivers low latency and resilience during degraded backhaul. Production deployment would be an O-RAN-aligned edge application; the PoC runs on a Jetson-class device or laptop. It is a template for monetising edge AI beyond pure connectivity.
     Fintech upside: resilience is transactional, not just informational. Offline vouchers and queued intents generate clean digital records of merchant trading even through grid events — records that can later support merchant credit scoring. A merchant who kept trading through last month's outages has demonstrated something a bank cannot see. This positions MoMo to lead a capability no mainstream African wallet has shipped.
     Digital infrastructure: every tower becomes a small resilience hub for the local economy — infrastructure earning a second job.

5. PoC Plan (Solo, Realistic)
5.1 Phase 1 — Site simulator + model (weeks 1–3)

A site simulator will be grounded in published parameters: battery specifications, typical site loads, Eskom schedules via the free EskomSePush tier, and GSMA energy benchmarks. XGBoost models estimate time-to-depletion and outage probability — explainable, no LSTM needed. KPIs are measured on held-out simulated scenarios against two naive baselines: schedule-only alerting (which by definition detects none of these unscheduled events) and always-alert (maximum coverage, maximum false alarms).

Success targets are set now and measured honestly:

Table 1: Phase 1 success targets, measured on held-out simulated scenarios
Metric
	
Target
Median time-to-depletion error	≤ 30 minutes
Simulated unscheduled network-loss events detected with ≥ 15 minutes of lead time	≥ 60%
Alert precision (warnings matching a real event within the window)	≥ 70%
 
 
5.2 Phase 2 — Live demo harness (weeks 4–5)

An Africa's Talking sandbox pushes real SMS and a USSD check-in to a judge's own phone, alongside the proposed signed-voucher demo: two handsets (customer and merchant PWA) exchange a QR voucher with no connectivity, then settle on reconnect. The demo flow: you are registered as the paired merchant (serving-cell pairing, exactly as designed for production) → flip the "grid" switch → watch the battery runtime estimate update → receive the alert on your handset → the app pre-activates offline mode → scan a real voucher between the two phones → restore power → watch the voucher settle. Queued-intent sync is shown in the simulated MoMo sandbox alongside.
5.3 Phase 3 — Reality check (parallel)

Five to ten design-partner interviews with township merchants validate the problem, alert copy and willingness to pay. No field pilot is claimed at this stage — that comes after MTN engagement.
5.4 What we would need from MTN

Three things, all standard commercial territory:

     A conversation about MoMo Open API sandbox access.
     Ten to twenty site-days of anonymised power/telemetry data, or an introduction to the relevant towerco.
     Technical mentorship.

6. Impact & Business Model

Merchant: fewer lost sales and better customer goodwill, at indicative pricing of R49/month via MoMo — to be validated in interviews.

MTN: recovered MoMo transaction volume during grid events; a differentiating value-add for SME bundles; a first-mover position on true offline mobile money — a capability proven in pilots but shipped by no mainstream African operator; a flagship AI-RAN monetisation story aligned with Ambition 2030; and portability to other grid-unstable markets such as Nigeria, Ghana, Uganda and Cameroon.

Platform vision (one paragraph, not a pivot): the same edge-resilience engine — site telemetry, predicted failure, automated offline fallback — extends naturally beyond merchants: clinics caching vaccine-fridge telemetry, estates failing over CCTV backhaul, irrigation systems receiving pre-outage safe schedules. Qhawe's Phase 1 beachhead stays deliberately narrow, but the architecture turns each tower into a proactive resilience hub.

Continent: proof that Africa can host useful AI where the problem lives, not only consume it from the cloud.


Africa should not simply follow the next wave of technology. We can host it where the problem lives: at the tower, where power, payment and people meet.

APPENDIX — NOT FOR FORM SUBMISSION

This appendix supports the live pitch and Q&A. It is deliberately excluded from the form-ready submission above — do not paste it into the submission form.
Appendix — Objection Handling (Not for Form Submission)

"How do you know who to alert?"

Merchants pair at registration via serving cell ID, verified against their pinned business location and re-checked periodically. In the PoC, customer alerts go to the merchant's opted-in regulars; at production scale, MTN could extend this to cell-targeted network SMS — an operator-only capability.

"What if the merchant's phone is also dead?"

That is the shop-power-loss case. The network typically stores SMS for a limited window (operator-dependent, roughly 24–72 hours), so delayed delivery is possible but not guaranteed; queued transactions and signed vouchers reconcile whenever any device comes back online. Our primary case is the tower-side failure — shop has power, network does not — where phones work and alerts arrive in time.

"Shops lose power even when towers don't."

True. Qhawe's alerts cover that case and queueing helps. But the tower-side case is the bigger blast radius and the one nobody else can see — that is where we build first.

"Why not just push EskomSePush to merchants?"

Schedules do not tell you whether this tower's degraded batteries will survive the outage, and they say nothing about transformer faults or load reduction. That gap is our product.

"Doesn't MoMo already do offline payments?"

No. MoMo QR requires an app session, PIN and SMS code; USSD needs a live gateway connection, which dies with the tower. Offline payment exists only in niche products such as NFC wearables and CBDC pilots. The PoC demonstrates the interaction pattern with simulated payment flows; production requires MTN's API and risk sign-off.

"Can you authorise payments offline?"

No — and we do not claim to. Vouchers and queued intents are proposed mechanisms: capped, time-limited, reconciled on reconnect, with risk controls MTN approves. We are explicit that this is a proposed integration.

"How do you prevent double-spending on vouchers?"

Each voucher carries a unique serial signed by the merchant's paired key. Settlement is one-time: the first redemption at the MoMo cloud wins, duplicates are rejected and flagged against the merchant's cap. Capped amounts and the 24-hour TTL bound the maximum exposure of any abuse.

"What if the model cries wolf?"

Confidence thresholds, alert caps and merchant opt-in — and precision is a headline PoC metric (≥ 70% target), measured against both naive baselines.
