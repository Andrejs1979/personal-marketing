# Personal — Product Requirements Document
**Version:** 1.0  
**Date:** 2026-05-18  
**Status:** Draft — Pre-seed  
**Author:** Product team  

---

## Vision

**Democratized concierge medicine at scale, powered by AI.**

A dedicated human GP who always knows you — your history, your trends, your concerns — made possible because AI eliminates the cognitive overhead that previously made this level of care available only to the wealthy.

Previously, continuous access to a doctor who truly knows you cost $3,000–$10,000/year (concierge medicine). Personal delivers the same quality of relationship at a consumer subscription price, because AI pre-digests everything so the physician can maintain genuine relationships with hundreds of patients simultaneously.

**Tagline:** Health that finally knows you.

---

## Problem

Healthcare in the US is episodic, fragmented, and impersonal:

1. **Data without insight.** Health apps (Apple Health, Fitbit, Oura) give you dashboards but no action. No one synthesizes across your wearable, labs, and EHR simultaneously.

2. **Random doctors.** Telemedicine (Teladoc, MDLive) gives you a different doctor every time. No continuity. No relationship. The doctor knows nothing about you before the call.

3. **Reactive, not proactive.** You go to the doctor when something is wrong. No one is watching your trends and catching problems before they become symptoms.

4. **Concierge is inaccessible.** A doctor who knows you, monitors you, and is always available costs $3,000–$10,000/year. Available only to the affluent.

**The result:** The average American has a meaningful relationship with a primary care physician — a doctor who knows their history, patterns, and preferences — only if they are wealthy enough to afford concierge medicine.

---

## Solution

Personal is an AI agentic wellbeing and healthcare superapp with three interlocking layers:

### Layer 1 — AI Health Companion (Consumer)
Always-on AI that monitors your health data, detects problems early, and handles health logistics on your behalf — without you having to ask.

### Layer 2 — Dedicated Human GP
A real, licensed primary care physician who sees your AI-digested health picture before every interaction. Same doctor every time. Async by default, video on demand.

### Layer 3 — Telemedicine + Coordination
Embedded HIPAA-compliant video (Doxy.me or equivalent). AI generates pre-visit summaries and post-visit drafts (Rx, referral letters, follow-up plans) for GP review.

**The moat:** The longer you use Personal, the harder it is to leave. Years of health data, a doctor who knows you deeply, and an AI calibrated to your individual baselines — none of which can be transferred.

---

## Target Users

### Primary: Health-Engaged Adults (25–55)
- Manage chronic conditions (hypertension, diabetes, anxiety, chronic fatigue)
- Own wearables (Apple Watch, Oura, Garmin, Whoop)
- Have employer insurance but frustrated by access gaps and random PCPs
- Willing to pay for quality healthcare
- TAM: ~80M Americans

### Secondary: Caregivers
- Managing health for elderly parents or children with chronic conditions
- Need visibility and coordination across multiple family members
- TAM: ~40M Americans

### Access Tier: Health-Equity Focus
- Medicaid/CHIP recipients, veterans, SNAP/WIC households, students, healthcare workers
- Full Premium access at $0 — funded by cross-subsidy from paying tiers
- TAM: ~100M Americans (also serves as distribution and social mission)

### Tertiary: Employers (B2B)
- Self-insured employers seeking to reduce healthcare costs
- Offer Personal as a benefit (employer pays per-employee subscription)
- TAM: ~30K self-insured employers

---

## Competitive Landscape

| Competitor | Model | Gap Personal Fills |
|-----------|-------|-------------------|
| Amazon One Medical (2026) | AI chatbot + in-person PCPs | No dedicated GP, no continuous monitoring |
| Claude for Healthcare | Apple Health integration, AI health assistant | No physician layer |
| Doctronic | AI authorized to practice in Utah, $19–$39/visit | No continuity, episodic visits |
| K Health | AI triage + async physician ($49/mo) | No dedicated doctor, no longitudinal data |
| Parsley Health | Functional medicine GP ($150–$275/mo) | No AI layer, expensive |
| Teladoc / MDLive | Random on-call doctors | No continuity, no data |
| Forward Health | Concierge + sensors + AI | Collapsed 2024 — hardware (CarePods) killed unit economics |
| Apple Health | Data aggregation | No AI synthesis, no physician |
| Concierge Medicine | Dedicated GP | $3K–$10K/year, no AI, not scalable |

**Unoccupied position:** The only platform combining continuous AI monitoring + dedicated human GP + telemedicine + longitudinal health graph, at consumer price points.

---

## Product Architecture

```
┌─────────────────────────────────────────────────────────┐
│                   LAYER 4: Platform Services            │
│   Appointments · Rx (GoodRx) · Insurance ·              │
│   Wearable integrations · Third-party health platforms  │
├─────────────────────────────────────────────────────────┤
│                 LAYER 3: Telemedicine                   │
│   AI pre-visit summary → Doxy.me embed →                │
│   AI post-visit draft (notes, Rx, referral)             │
├───────────────────────┬─────────────────────────────────┤
│  LAYER 1              │  LAYER 2                        │
│  Consumer App         │  Provider AI OS                 │
│                       │                                 │
│  HealthKit sync       │  Patient panel dashboard        │
│  Wearable data        │  Priority queue (who needs      │
│  EHR ingestion        │    attention today)             │
│  AI insights          │  Full patient health graph      │
│  Trend alerts         │  AI-drafted messages/notes      │
│  Clinical reports     │  Rx / referral workflow         │
│  Chat with AI         │  Proactive action suggestions   │
│  "Share with GP"      │  Real-time patient alerts       │
└───────────────────────┴─────────────────────────────────┤
│                 SHARED DATA LAYER                       │
│   HealthTrack (FHIR R4) · HealthKit · EHR (Flexpa) ·   │
│   Wearables · Labs · Consent model · Audit log ·        │
│   Longitudinal health graph · AI synthesis              │
└─────────────────────────────────────────────────────────┘
```

**Build order:** Data Layer → Layer 1 → Layer 2 → Layer 3 → Layer 4

Each layer creates standalone value. Consumer data must flow before the provider OS has value. Both must exist before telemedicine is differentiated from generic Teladoc.

---

## Existing Infrastructure

| Component | Status | Location |
|-----------|--------|----------|
| HealthTrack data layer (FHIR R4, HIPAA) | ✅ Deployed | `caremind-ai/healthtrack` |
| HealthKit sync (16 types → FHIR R4) | ✅ Complete | `healthtrack/apple/HealthTrackKit` |
| EHR ingestion via Flexpa | ✅ Complete | `healthtrack/gateway` |
| AI synthesis (Claude via CF binding) | ✅ Deployed | `healthtrack/ai-worker` |
| GraphQL API + subscriptions | ✅ Deployed | `healthtrack/gateway` |
| Clinical report generation | ✅ Backend complete | `caremind-ai/chiron` |
| Consumer iOS shell | ✅ Swift/SwiftUI | `caremind-ai/preventhealth` |
| Marketing site | ✅ Live | `personal.g-a-l-a-c-t-i-c.com` |
| Provider AI OS | ❌ Not started | — |
| Telemedicine integration | ❌ Not started | — |
| Rx / Insurance / Appointments | ❌ Not started | — |

---

## Features by Layer

### Data Layer (Foundation — already built)
- FHIR R4 data model with audit chain and consent records
- HealthKit sync: heart rate, HRV, sleep, steps, SpO2, respiratory rate, active energy, resting energy, body mass, height, blood glucose, blood pressure systolic/diastolic, mindfulness, stand time, vo2max
- EHR ingestion: Flexpa integration (CCDA documents)
- AI synthesis: Claude Sonnet 4.6 via Cloudflare AI Gateway
- GraphQL API with real-time subscriptions (SSE)
- HIPAA: AES-256-GCM encryption on PHI tokens, append-only audit log, soft deletes only, log sanitizer

### Layer 1 — Consumer App (iOS, Swift/SwiftUI)

**Health Monitoring**
- Continuous HealthKit sync (background, anchored queries per type)
- Wearable integrations: Apple Watch (native), Garmin, Oura, Whoop, Dexcom (OAuth)
- Lab result ingestion: manual upload + Flexpa EHR pull
- Unified health graph: single timeline of all data sources

**AI Insights**
- Daily health score (0–100) with trend and explanation
- Proactive alerts: "Your HRV has declined 12% over 5 days"
- Weekly AI health summary (natural language, shareable)
- Trend detection with configurable thresholds
- "Ask Personal" — AI chat grounded in your health data
- Multi-LLM consensus for high-severity findings (Claude primary, Gemini / GPT-4o as independent reviewers; used when AI assigns highest alert urgency)

**Clinical Reports**
- On-demand clinical report generation (PDF + shareable link)
- Templates: annual summary, specialist referral context, pre-visit summary, medication reconciliation
- GP-reviewed and GP-stamped reports (Care tier)
- Share with any provider (QR code, link, fax via Updox)

**Agentic Actions (Plus+)**
- Appointment booking: sync with GP calendar, external provider search
- Prescription refill: GP reviews and authorizes, pharmacy routing via GoodRx
- Lab orders: GP-ordered or standing orders, LabCorp / Quest integration
- Insurance pre-auth: AI drafts, GP reviews, submits
- Referral letters: AI drafts, GP signs

**Safety**
- Emergency escalation: if AI detects clinically critical values (SpO2 < 90%, resting HR > 150 or < 40, etc.) — in-app alert with "Call 911" prompt and notification to GP; app does not attempt to triage emergencies itself
- Alert fatigue prevention: tiered alert severity (info, caution, urgent, emergency); user-configurable thresholds for non-emergency alerts

**Account & Access**
- Sign in with Apple / Google
- Health data consent model (granular per data type, withdrawal at any time)
- Access tier eligibility verification (Medicaid ID, VA ID, `.edu` email, SNAP EBT number, employer badge; income self-attestation + annual re-verification)
- Identity verification: phone number + government ID for Care tier (required for controlled substance Rx and insurance billing)
- Data lifecycle: account deletion → data anonymized (not deleted) per HIPAA 6-year retention requirement; full export available before deletion
- Data export: FHIR bundle, CSV, PDF

### Layer 2 — Provider AI OS (Web, React or Next.js)

**Patient Panel**
- List of all assigned patients, sorted by AI-computed urgency
- Priority queue: "Who needs attention today?" — flagged by AI based on trend alerts, upcoming appointments, overdue follow-ups
- Real-time patient alerts: push notification when a patient's metrics cross thresholds

**Patient Health Graph**
- Full longitudinal view: HealthKit data, labs, EHR documents, AI insights, visit history, medications, referrals
- AI-generated patient summary: "Alex is a 34-year-old male with hypertension and elevated stress markers. HRV has declined 12% over 5 days. Last seen 6 weeks ago."
- Timeline view: all events (data changes, alerts, messages, visits, Rx)

**Clinical Workflow**
- Async messaging: AI drafts replies based on patient context, GP reviews/edits/sends
- Rx authorization: review AI draft, sign and transmit to pharmacy
- Referral letters: review AI draft, sign and send to specialist
- Visit notes: AI transcription + draft SOAP note for GP review
- Lab order creation: GP orders, routes to patient's preferred lab

**Workload Management**
- Panel size guidance: AI flags when GP is approaching capacity
- Time tracking: async vs. sync time per patient
- Billing integration: CPT code suggestions for insurance-billable encounters

### Layer 3 — Telemedicine

**Pre-Visit**
- AI generates pre-visit summary (automatically before every scheduled video call)
- Summary includes: recent trends, alerts since last visit, current medications, open items, patient's stated reason for visit
- GP sees summary before joining the call

**Video Visit**
- Doxy.me embedded HIPAA-compliant video (or equivalent: Daily.co, Whereby)
- No separate app download required
- Waiting room with patient vitals displayed in real-time

**Post-Visit**
- AI transcribes and generates draft SOAP note
- AI drafts follow-up plan, Rx requests, referral letters for GP review
- Patient receives AI-generated after-visit summary
- Follow-up scheduling suggested based on clinical context

### Layer 4 — Platform Services

**Appointments**
- GP calendar management (availability, buffer time, visit types)
- Patient scheduling (async message, video, urgent same-day)
- Reminders (push, SMS, email)
- Integration with external calendars (Google, Apple)

**Prescriptions**
- GoodRx integration: price comparison and pharmacy routing
- Electronic prescribing (eRx via Surescripts or DrFirst)
- Refill tracking and reminders
- Medication adherence monitoring (self-reported + wearable proxy signals)
- **Controlled substances (Schedule II–V): not supported at launch.** DEA telemedicine prescribing rules for controlled substances require in-person evaluation in most cases (post-COVID waivers are being phased out as of 2025). Agentic Rx refill is limited to non-controlled medications. Add to roadmap only after obtaining legal opinion on current DEA rules.

**Insurance** *(deferred — v2 at earliest; see Open Decisions)*
- Benefits verification: confirm patient coverage before visit
- Claims submission: AI drafts CMS-1500 or electronic claim
- ERA/EOB parsing: reconcile payments
- Prior authorization: AI drafts, GP reviews
- *Note: Recommendation is to launch cash-pay only. Insurance billing adds significant operational complexity (credentialing, billing staff, claim denials, state-by-state contract negotiations). Revisit after reaching 1,000 Care subscribers.*

**Wearable Integrations (OAuth)**
- Garmin Connect
- Oura Ring
- Whoop
- Dexcom (CGM data for diabetes management)
- Withings (blood pressure, weight)

---

## Pricing

| Tier | Price | Who It's For | Key Limits |
|------|-------|-------------|------------|
| **Free** | $0/mo | Everyone | HealthKit sync, weekly summary, health score, 1 clinical report/month |
| **Plus** | $9/mo | AI-first users | Real-time alerts, unlimited reports, agentic actions, unlimited AI chat |
| **Premium** | $29/mo | Full wellbeing | Multi-AI consensus, predictive risk, mental health + nutrition AI, GoodRx |
| **Care** | $79/mo | GP relationship | Everything + dedicated licensed GP, unlimited async, 2 video visits/month |
| **Family** | $79 + $35/member | Families | Care for entire family; shared GP or individual GPs (user's choice) |
| **Access** | $0/mo | Medicaid, veterans, students, SNAP/WIC, healthcare workers, low-income | Full Premium features — no compromise |

**Family plan details:**
- Base $79 covers primary member (Care tier)
- +$35/member for additional adults
- Elderly parents qualify as members
- Children (minors) in v2
- Choice: shared family GP or individual GP per member

**Access tier verification:** Medicaid ID, VA ID, `.edu` email, SNAP EBT card number, employer badge (healthcare worker), income self-attestation + periodic re-verification

---

## Monetization (Multi-Stream)

Personal does not rely solely on subscriptions. The free tier is a strategic asset — distribution, data, and a social mission that drives PR.

| Stream | Mechanism | Est. Revenue per User/Year |
|--------|-----------|---------------------------|
| Consumer subscriptions | Monthly recurring | $108–$948/yr |
| Employer B2B | Per-employee bulk license | $200–$600/seat/yr |
| GoodRx pharmacy referral | Commission per Rx filled | $5–$15/Rx |
| Lab referral fees | LabCorp/Quest per order | $3–$10/order |
| Specialist booking | Commission on booked referrals | $20–$50/referral |
| Anonymized data licensing | Aggregate de-identified cohort data | Long-term, post-scale |
| Insurance leads | Warm referrals to health plans | $50–$200/qualified lead |

**Unit economics target (blended, steady state):**

Care tier ($79/mo) has the highest GP cost and lowest margin — it anchors the GP relationship. Higher tiers cross-subsidize it.

| Cost item | Care tier | Plus/Premium |
|-----------|-----------|-------------|
| Revenue | $79/mo | $9–$29/mo |
| GP cost | ~$20–25/mo* | $0 |
| AI inference | ~$0.50/mo | ~$0.30/mo |
| Infrastructure | ~$2/mo | ~$1/mo |
| **Gross margin** | **~65–70%** | **>90%** |

GP cost model: A GP earning $150K/year carrying a 500-patient async panel (AI-enabled; traditional panels are 1,500–2,500 patients with in-person visits) = $25/patient/month. At 1,000 patients with AI efficiency, cost drops to ~$12.50/patient/month. The 200-patient figure in CONCEPT.md refers to the GP's active panel at launch — actual cost at that panel size is ~$62/patient/month, which yields only ~15% gross margin on the $79 Care tier. Margin expands as panel scales to 500–1,000 patients. GP compensation should be structured as base salary + per-patient panel bonus to align incentives with growth.

**Gross margin target: >70% blended** (across all tiers; Care alone targets >65% at 500-patient panel scale)

---

## Go-to-Market

### Phase 1 — Waitlist + Beta (Q3 2026)
- Launch `personal.g-a-l-a-c-t-i-c.com` to capture waitlist (done ✅)
- Target: 5,000 waitlist signups
- Channels: health/wellness communities (Reddit r/QuantifiedSelf, Oura/Garmin forums), founder-led content, Product Hunt
- Beta: 200 users, 2 GPs, Tier 1 market (California, New York, Texas — telemedicine-friendly states)

### Phase 2 — Consumer Launch (Q4 2026)
- Free + Plus tiers on iOS
- No Care tier yet — AI companion + data flywheel first
- Target: 10,000 MAU
- App Store featuring strategy: "Health & Fitness" + "Medical"
- PR angle: "The only health app that actually knows you"

### Phase 3 — GP Layer (Q1 2027)
- Care tier launch with 10 contracted GPs
- Telemedicine via Doxy.me
- Target: 500 Care subscribers
- GP sourcing: direct outreach to PCPs + NPs interested in async/remote practice

### Phase 4 — Scale (2027)
- Family plan ($79 + $35/member)
- Android app (Google Health Connect) — if Android decision made
- Employer B2B sales motion
- Geographic expansion: 50-state telemedicine coverage
- GP marketplace model (platform vs. contracted panel)
- Insurance billing (optional, post-1,000 Care subscribers)

---

## Success Metrics

### North Star
**Monthly Active Patients with a meaningful GP interaction** — async message read by GP, GP-reviewed alert, or video visit. This metric only moves if both the data layer and the GP relationship are working.

*Note: This metric is unmeasurable until M7–M8 (GP layer launch). Leading indicators for M1–M6:*
- *M1–M4: D30 retention, HealthKit sync enabled %, daily health score views*
- *M5–M6: Plus conversion rate, agentic action completions, AI chat engagement*
- *M7+: GP interaction rate, response time, patient NPS*

### Consumer App
| Metric | 3 months | 12 months |
|--------|----------|-----------|
| Registered users | 2,000 | 25,000 |
| MAU | 800 | 15,000 |
| HealthKit sync enabled | 70% | 75% |
| D30 retention | 35% | 45% |
| Weekly AI summary opens | 60% | 65% |
| Plus conversion | 8% | 12% |

### Care Tier
| Metric | 3 months | 12 months |
|--------|----------|-----------|
| Care subscribers | 50 | 500 |
| GP panel utilization | 60% | 85% |
| Avg async response time (GP) | <4hrs | <2hrs |
| Patient NPS | >50 | >60 |
| Monthly GP interactions | 2/patient | 3/patient |

### Business
| Metric | 12 months |
|--------|-----------|
| MRR | $150K |
| ARR run rate | $1.8M |
| Gross margin | >65% |
| CAC (blended) | <$40 |
| LTV (Care, 12-month cohort) | >$800 |

---

## Regulatory & Compliance

### HIPAA
- BAA required: Anthropic (AI synthesis), Cloudflare (Gateway/Workers/KV), Fly.io (Rust Core), Supabase (PostgreSQL) — **currently blocking PHI in production**
- PHI handling: AES-256-GCM encryption, audit chain (tamper-evident), consent model, soft deletes, log sanitizer
- Patient rights: data export (FHIR bundle), deletion requests, consent withdrawal

### AI Regulatory Posture
- AI is Clinical Decision Support (CDS), not diagnosis — all clinical decisions made by licensed GP
- AI outputs are "suggestions for physician review," never direct patient-facing clinical advice
- Framing: "Personal noticed something — your doctor has been notified" (AI flags; GP acts)
- Exemption basis: Qualifying CDS is excluded from FDA device regulation under 21st Century Cures Act §3060 (not the De Novo pathway, which is a classification route for novel Class II devices — distinct). Personal's AI must meet all four CDS criteria: (1) not intended to replace clinical judgment, (2) physician can independently review basis, (3) not for acquiring, processing, or analyzing medical images/signals, (4) intended for use by a healthcare professional.
- Risk: If AI outputs are shown directly to patients without GP review (as intended in free/plus tiers), criteria (4) may not be met — the "intended for HCP" requirement is unsatisfied. Confirm framing with healthcare counsel before launch. Likely resolution: label all direct-to-patient AI as "wellness information" not CDS; CDS framing applies only to GP-facing AI OS.

### Telemedicine
- Launch states: California, New York, Texas, Florida — largest populations + telemedicine-friendly regulation
- GP licensure: GPs must hold licenses in patient's state; multi-state IMLC licensure for panel GPs
- Prescribing: DEA telemedicine rules (post-COVID waiver status — confirm current)
- Malpractice: GPs covered under Personal's group policy or verified to carry own coverage

### Data
- EHR access: Flexpa (patient-directed, CARIN Blue Button compliant)
- HealthKit: Apple HealthKit API, patient-initiated sync, background delivery
- Labs: patient-directed upload + HL7 FHIR R4 results from supported labs
- Anonymized data licensing: IRB review required before any commercial use

---

## Build Roadmap

### M1–M2 (June–July 2026): HealthTrackKit Integration + BAAs
- **BAA procurement (blocking):** Sign BAAs with Anthropic, Cloudflare, Fly.io, Supabase before any real user PHI enters the system. Target: all BAAs signed by end of M1.
- Integrate HealthTrackKit SPM into PreventHealth iOS app
- Enable background sync, permissions request, incremental + historical sync
- End-to-end smoke test: HealthKit → FHIR → GraphQL → AI synthesis
- Waitlist backend: email capture at `personal.g-a-l-a-c-t-i-c.com` → Mailchimp / Loops (no PHI)
- Ship: free tier on TestFlight to internal users (post-BAA)

### M3–M4 (August–September 2026): Consumer App MVP
- Layer 1 feature-complete: health score, AI insights, proactive alerts, clinical reports (PDF)
- AI chat ("Ask Personal") grounded in user's health graph
- Waitlist-to-beta conversion: 200 users
- Goal: 35% D30 retention

### M5–M6 (October–November 2026): Plus + Premium Tiers + Agentic
- Agentic actions: appointment booking, Rx refill request (non-controlled Rx, pending GP authorization)
- Multi-LLM consensus for high-severity findings (Gemini + GPT-4o validators)
- Predictive risk modeling, nutrition AI, mental health integrations (Premium features)
- Plus ($9) and Premium ($29) subscription billing (RevenueCat + Stripe)
- App Store submission

### M7–M8 (December 2026–January 2027): Provider AI OS v1
- GP dashboard: patient panel, priority queue, health graph view
- Async messaging with AI draft assist
- GP alert review and response
- Rx and referral authorization workflow
- 2 contracted GPs in beta

### M9–M10 (February–March 2027): Care Tier + Telemedicine
- Doxy.me video integration
- AI pre-visit summary generation
- Post-visit draft (SOAP note, Rx, follow-up plan)
- Care tier billing
- 10 GPs, 500 Care subscribers target

### M11–M12 (April–May 2027): Scale Foundations
- Family plan
- Employer B2B pilot (3 employers)
- GoodRx Rx integration
- Lab order integration (LabCorp/Quest)
- 50-state telemedicine coverage (GP multi-state licensure)

---

## Open Decisions

- [ ] **Product name:** "Personal" — trademark as design mark (word alone too generic for USPTO); explore `personal.health` and `personal.care` domains
- [ ] **GP sourcing model:** Platform marketplace (GP credentialing + revenue share) vs. contracted panel (GPs as W-2 or 1099); start contracted for quality control
- [ ] **BAA procurement:** Anthropic, Cloudflare, Fly.io, Supabase — blocking PHI in production; **must sign before M3 beta launch** (30 days, high priority)
- [ ] **Regulatory counsel:** Confirm AI-as-CDS framing (§3060 criteria), telemedicine prescribing rules, and DEA controlled substance position before Care tier launch
- [ ] **Geographic launch:** Finalize Tier 1 state list (CA, NY, TX, FL recommended); confirm IMLC participation and state-specific telemedicine rules
- [ ] **GP compensation model:** Salary vs. per-panel vs. per-interaction; model must account for panel ramp — GPs earn below target at <200 patients; structure accordingly (base + per-patient bonus)
- [ ] **GP after-hours coverage:** Async-first means non-urgent care is fine; urgent alerts at 2am need escalation. Options: on-call rotation, after-hours nurse line, partner urgent care network.
- [ ] **Insurance:** Recommend starting cash-pay only; revisit after 1,000 Care subscribers
- [ ] **Access tier funding:** Cross-subsidy from paying tiers initially; explore Medicaid MCO contracts and CMMI grants at scale; monitor fraud risk on self-attestation
- [ ] **Android:** HealthKit is iOS-only. Access tier (Medicaid, SNAP) skews Android. Options: (1) Google Health Connect integration in v2, (2) accept iOS-only initially and acknowledge equity gap, (3) build Android in parallel (2x cost). Decide before public launch.
- [ ] **AI training consent:** Define whether patient data is ever used for model fine-tuning and build explicit opt-in consent flow if so

---

## Appendix A — Technical Stack

| Component | Technology | Notes |
|-----------|-----------|-------|
| iOS app | Swift/SwiftUI | PreventHealth codebase as foundation; iOS only at launch |
| HealthKit sync | HealthTrackKit (SPM) | Already built; 16 types, FHIR R4 |
| Android health sync | Google Health Connect | Not in scope for v1; revisit with Android decision |
| Backend gateway | Cloudflare Workers (Hono/TypeScript) | Already deployed |
| Database | Supabase (PostgreSQL, HIPAA plan) | FHIR R4 schema; BAA required before PHI |
| AI synthesis | Claude Sonnet 4.6 via CF AI Gateway | Already wired; BAA required before PHI |
| Secondary AI | Gemini (Vertex AI), GPT-4o (OpenRouter) | Consensus for high-severity findings; BAA required |
| EHR ingestion | Flexpa | Already integrated (CARIN Blue Button) |
| Telemedicine | Doxy.me (primary), Daily.co (fallback) | Not yet integrated; both have HIPAA BAAs available |
| eRx | Surescripts or DrFirst | Not yet integrated; DEA EPCS certification required for controlled Rx |
| Rx pricing | GoodRx API | Not yet integrated; non-controlled Rx only at launch |
| Billing (consumer) | RevenueCat (in-app) + Stripe (web/B2B) | Not yet integrated |
| Provider OS | Next.js (App Router) + same GraphQL API | Not yet started; Next.js chosen for SSR patient data pages |
| Push notifications | APNs (iOS) | Critical for proactive alerts; background delivery |
| Waitlist | Loops.so (email) | Needed before M3 beta; no PHI |

## Appendix B — Prior Art

- `caremind-ai/chiron` PRD v1.2 (Jan 2026) — clinical coordination platform, appointment-first
- `caremind-ai/preventhealth` PRD v4.0 (Aug 2025) — preventive health, multi-LLM, HealthKit-first
- This PRD supersedes both. Personal is the merged vision.

---

*Next: Epic decomposition and sprint planning.*
