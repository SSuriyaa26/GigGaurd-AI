# GigGuard AI
## AI-Powered Parametric Income Shield for Q-Commerce Delivery Partners

[![Tech Stack](https://img.shields.io/badge/stack-React%20Native%20%7C%20FastAPI%20%7C%20XGBoost-blue?style=for-the-badge)](https://github.com/gigguard-ai)
[![Hackathon](https://img.shields.io/badge/Guidewire-DEVTrails%202026-orange?style=for-the-badge)](https://devtrails.guidewire.com)
[![Phase](https://img.shields.io/badge/Phase-1%20Ideation%20%26%20Foundation-purple?style=for-the-badge)](#)
[![Status](https://img.shields.io/badge/status-Active%20Development-brightgreen?style=for-the-badge)](#)

> *"Zero-touch income protection for India's gig delivery heroes"*

---

We want to be honest about where this idea came from before we show you any architecture.

We talked to three Blinkit delivery partners in Anna Nagar before writing a single line of code. One of them, Karthik, told us about a week in October when he earned Rs. 1,140 instead of his expected Rs. 3,200 because the Northeast Monsoon flooded his zone on a Tuesday evening and again on Wednesday. He had Rs. 1,800 in rent due. He borrowed from his cousin. The platform offered no compensation. No insurance product he could afford covered this. He just absorbed the loss and went back out on Thursday.

That conversation is what GigGuard is built around. Not the problem statement. Not a market sizing exercise. Karthik's Tuesday evening.

---

## The Problem

India has 12 million+ gig delivery workers. That is roughly the population of Chennai. Every single one of them is exposed to income loss from external disruptions they cannot control — rain, floods, extreme heat, bandhs, poor air quality — and almost none of them have any protection against it. Fewer than 3.2% carry any form of income continuity coverage beyond whatever the platform decides to offer that week.

The Q-commerce segment has it worst. These are the Blinkit and Zepto partners who make 10-minute grocery delivery possible. Their entire working life is tied to a 2-km radius around a single dark store. When that zone floods, there is nowhere else to go. A food delivery rider who cannot work in Anna Nagar can ride to T. Nagar. A Blinkit partner cannot. That radius is not a feature of the job. It is the whole job.

When disruptions hit, they hit completely. During Chennai's Northeast Monsoon season, a Q-commerce partner in a flood-prone zone loses somewhere between Rs. 2,200 and Rs. 3,800 in a single week. That is 18 to 24% of their monthly earnings, gone in weather they could not have worked through regardless of how hard they tried. The industry treats this as their personal problem.

GigGuard is a simple idea with hard engineering behind it: when the rain makes it impossible for Karthik to work, his phone should know before he does, and his wallet should fill before he has to ask anyone for help.

---

## Who We Built This For

Most hackathon teams will pick Zomato or Swiggy food delivery. It is the obvious choice and we considered it. We switched to Q-commerce after talking to Karthik, because the income volatility profile is structurally worse and, crucially, more precisely addressable. Every disruption signal we monitor maps directly to a specific dark store catchment zone. That is not true for food delivery.

| Dimension | Food Delivery | Q-Commerce — our persona |
|---|---|---|
| Delivery window | 30 to 45 minutes | 10 to 15 minutes |
| Operating radius | 5 to 8 km | 1 to 3 km from one dark store |
| Weather buffer | Platform can extend SLA | A 10-minute SLA cannot stretch |
| Orders per hour during disruption | Drops 40 to 50% | Drops 70 to 85% |
| Platform income replacement | Partial surge bonuses | None during shutdowns |

### How the day actually works

A Q-commerce partner's income is not evenly distributed across the day. The morning window (7 AM to 11 AM) accounts for roughly 35% of daily earnings. The evening window (5 PM to 10 PM) accounts for the other 65%.

This asymmetry matters enormously. A disruption that hits at 5:30 PM does not cause a 50% income loss. It causes a rider who earned Rs. 180 in the morning to earn Rs. 180 for the entire day instead of Rs. 650. That Rs. 470 gap is not an inconvenience. It is the rent payment that does not happen.

| City tier | Daily range | Weekly range | Monthly range |
|---|---|---|---|
| Tier-1 (Chennai, Bengaluru) | Rs. 550 to Rs. 800 | Rs. 3,850 to Rs. 5,600 | Rs. 15,400 to Rs. 22,400 |
| Tier-2 (Coimbatore, Mysuru) | Rs. 380 to Rs. 560 | Rs. 2,660 to Rs. 3,920 | Rs. 10,640 to Rs. 15,680 |

### What Karthik is actually worried about

He is not worried about hospital bills or whether his bike gets repaired. He is thinking about the gap between what he needs this week and what he will earn. He has no savings buffer beyond a week or two. He has rent. Maybe a loan EMI. Maybe school fees. When a disruption week comes, those commitments do not flex.

This shaped every product decision we made:

- Premium deducted via UPI AutoPay on Monday morning. Asking a rider to actively pay each week means some weeks he forgets or skips it. We cannot let that happen on a week when it rains.
- Payout in under 90 seconds. By the time Karthik has figured out he cannot work tonight, the money should already be moving. Not after he calls someone. Not after he files a form.
- Works on a mid-range Android with patchy connectivity. Karthik does not have an iPhone. He does not have stable WiFi while on shift. The app has to work for him, not for us.
- Onboarding under 3 minutes. He is doing this between deliveries. If it takes longer than that he closes it and never comes back.

### Karthik's story

It was the second week of October 2024. Northeast Monsoon rainfall hit Anna Nagar at 42mm in 3 hours on a Tuesday evening. Karthik had completed 6 deliveries by 5 PM and was on track for a Rs. 680 day. By 5:45 PM, the Blinkit app showed zero active orders. Waterlogged streets had triggered dark store dispatch suspension. He waited under a petrol bunk until 8 PM. Nothing. Wednesday was the same. That week he earned Rs. 1,140 instead of Rs. 3,200. He had Rs. 1,800 in rent due Friday. He borrowed from his cousin.

GigGuard would have triggered at the 3-hour rainfall mark Tuesday evening. By 6:10 PM, Rs. 960 would have been in his UPI wallet. By Wednesday evening, another Rs. 960. His rent would have been covered before he asked anyone for help.

---

## What GigGuard Does

GigGuard monitors five categories of disruption signals at pin-code level using live weather, AQI, and civic alert feeds. When a parameter crosses a validated threshold, we cross-check it against a second source — our mock dark store order-count API — before doing anything else. If both sources confirm, we run the claim through a multi-layer fraud stack, calculate the income loss for the rider's active shift window, and send the money. The whole path from trigger to UPI credit takes under 90 seconds for clean claims.

Riders pay a weekly premium set by our XGBoost model. No forms. No phone calls. No waiting.

---

## Complete User Workflow

```mermaid
flowchart TD
    A([Rider downloads GigGuard on Android]) --> B[Onboarding\nPhone OTP + Aadhaar verify\n+ Blinkit or Zepto Rider ID]
    B --> C[Risk Profiling\n5-question quiz captures zone,\nshift hours, avg. weekly earnings]
    C --> D[XGBoost Scores the Rider\nRisk score 0 to 100\nbased on zone + history + season]
    D --> E[Weekly Policy Offered\nDynamic premium Rs. 28 to Rs. 72\nexplained in plain language]
    E --> F{Rider accepts?}
    F -- Yes --> G[UPI AutoPay mandate set\nPremium deducted Monday 8 AM]
    F -- No --> H([No coverage this week\nApp nudges next Monday])
    G --> I[Coverage active\nReal-time monitoring begins]
    I --> J{Parametric monitor\nPolls every 15 minutes:\nRainfall / AQI / Heat / Civic}
    J -- No threshold crossed --> J
    J -- Threshold crossed --> K[Dual-source validation\nEnvironmental API +\nDark store order-count API]
    K --> L{Coherence Score check\nGPS + sensor fusion +\nzone history validation}
    L -- Score above threshold: clean --> M[Claim auto-raised\nIncome loss calculated\nfor rider's active shift window]
    L -- Score below threshold: flagged --> N[Provisional Payout state\n50% released instantly\n50% held in 4-hour review]
    M --> O[Full payout disbursed\nto UPI wallet in under 90 sec\nPush notification sent]
    N --> P{Signals self-correct\nwithin 4 hours?}
    P -- Yes --> O
    P -- No --> Q[Soft verification prompt\nor human review queue\n24-hour SLA]
    O --> R[Dashboard updated\nEarnings protected this week\nNext week premium recalculated]
    R --> I
```

---

## Weekly Premium Model

We spent longer on the premium formula than on anything else in Phase 1. The core argument we kept coming back to: a rider working the dry OMR corridor in February should not pay the same rate as a rider in T. Nagar during the Northeast Monsoon. Flat-rate pricing is what existing products do, and it is exactly why existing products do not work for this segment. The safer rider subsidises the riskier one, resents it, and churns.

Our formula has four components that multiply together.

### Formula

```
Weekly_Premium = Base_Premium x Zone_Risk_Multiplier x Rider_Risk_Score_Factor x Season_Index

Components:

  Base_Premium = Rs. 35
    Anchors max weekly payout at Rs. 1,000 at a 3.5% expected loss ratio.
    Derived from historical Chennai disruption frequency data (IMD 2019 to 2024).
    We are not guessing at 3.5%. We calculated it from 5 years of event data.

  Zone_Risk_Multiplier (ZRM)
    Updated quarterly from IMD event logs.
    Low risk zone    (fewer than 5 disruption days per quarter)   ->  ZRM = 0.80
    Medium risk zone (5 to 12 disruption days per quarter)        ->  ZRM = 1.00
    High risk zone   (more than 12 disruption days per quarter)   ->  ZRM = 1.45

  Rider_Risk_Score_Factor (RSF)
    Output of our XGBoost model, converted to a multiplier.
    RSF = 0.85 + (RiderScore / 100) x 0.30
    Score of 0   ->  RSF = 0.85  (consistent, low-volatility rider)
    Score of 100 ->  RSF = 1.15  (high earnings volatility, irregular zones)

  Season_Index (SI)
    January to May   (dry season)              ->  SI = 0.90
    June to September (Southwest Monsoon)      ->  SI = 1.20
    October to December (Northeast Monsoon)    ->  SI = 1.35
    The NEM index of 1.35 reflects Chennai specifically.
    Bengaluru gets a different seasonal curve.
```

### Three riders, same week, October, Chennai

| Rider | Zone | ZRM | XGBoost Score | RSF | SI | Weekly Premium | Max Payout |
|---|---|---|---|---|---|---|---|
| Karthik, Anna Nagar | Medium | 1.00 | 62 | 1.04 | 1.35 | **Rs. 49** | Rs. 1,400 |
| Priya, T. Nagar core | High | 1.45 | 74 | 1.07 | 1.35 | **Rs. 73** | Rs. 2,000 |
| Manoj, OMR corridor | Low | 0.80 | 38 | 0.96 | 1.35 | **Rs. 36** | Rs. 1,000 |

All three pay under Rs. 75 per week. Karthik pays Rs. 49 for coverage that pays out Rs. 1,400 in a bad week. That is roughly one Zepto platform convenience fee for 28 times the value in protection. We think that ratio is the product.

### Safe Zone Streak Discount

We added this after one of our field conversations. A rider we spoke to said something like: "if I know this week will cost me the same as last week, I can plan for it." Rate certainty matters to someone with a fixed weekly budget. So we built it in.

Riders who stay in low-disruption corridors accumulate a streak:

- 2 clean consecutive weeks: Rs. 8 off the next premium
- 4 clean consecutive weeks: Rs. 15 off plus a Safe Rider badge
- Week 5 of unbroken low-risk operation: GigGuard Gold status, premium frozen at the initial rate for the next 4 weeks regardless of seasonal index movement

Streaks reset only if the rider voluntarily moves to a higher-risk zone. The important thing: Gold is not a loyalty gimmick. It is the one thing a flat-rate insurance product structurally cannot offer, because flat-rate products have no rate to lock. We can offer it because our pricing is dynamic.

---

## Parametric Triggers

These are income-loss triggers only. We are insuring lost earnings, not property, health, or vehicles.

| Trigger | Data Source | Threshold | Income Loss Estimate | How We Detect It |
|---|---|---|---|---|
| Heavy rainfall | OpenWeatherMap free tier + IMD district alerts | More than 25mm in any 3-hour rolling window | 65 to 80% drop in dark store dispatch | 15-min rolling precipitation sum per pin-code geofence |
| Severe AQI | CPCB Open Data API via data.gov.in | AQI above 250 sustained for 2+ hours | 35 to 50% drop | Hourly station pull; nearest-station value interpolated to rider's pin code |
| Extreme heat index | OpenWeatherMap temperature + relative humidity | Apparent temperature above 42 degrees C between 11 AM and 4 PM, sustained 90 min | 40 to 55% drop in afternoon window | Heat index computed server-side; zone alert on sustained breach |
| Flash flood or civic Red Alert | NDMA Disaster Alert API + state emergency feeds (mock, Phase 1) | Official Red or Orange advisory for rider's pin code polygon | 85 to 100% drop | Webhook listener on NDMA feed; polygon intersection against rider's pin code |
| Unplanned curfew or bandh | Local civic RSS + NLP classifier | Verified movement restriction exceeding 3 hours in rider's zone | 90 to 100% drop | NLP on civic news feeds, corroborated by dark store order-count anomaly |

One rule applies to all five: no trigger fires on a single source. The environmental reading must be confirmed by a second signal, usually the dark store order-count drop, before a payout initiates. This is both our quality gate and the first line of our fraud defense.

---

## How the AI Actually Works

We have tried to be specific here because "AI-powered" means nothing by itself. This is what we are actually building and training, what data it runs on, and where we are honest about what Phase 1 does not yet have.

### The full data pipeline

```
Raw Data Sources
  IMD historical rainfall CSVs (2015 to 2024, station-level)      --+
  CPCB AQI station archives (data.gov.in, 2018 to 2024)           --+--> Ingestion Layer
  Synthetic gig earnings dataset (NITI Aayog distribution           --+
    parameters + field survey from 3 Blinkit partners)             --+
  GigGuard onboarding survey responses (zone, hours, platform)     --+

Ingestion Layer (FastAPI background worker)
  - Normalizes all datasets to a common schema:
    (date, pin_code, metric_type, metric_value)
  - Stores in PostgreSQL time-series table with pin_code index
  - Runs nightly at 1 AM to append latest IMD/CPCB data

Feature Engineering (runs every Sunday 11 PM, per rider)
  For each active rider, compute a feature vector:
  - rain_variance_90d: std. dev. of daily rainfall in rider's pin code
    over the past 90 days
  - disruption_freq_q: trigger-eligible days in current quarter
    for rider's pin code
  - aqi_severity_7d: count of AQI-above-200 hours in past 7 days
  - zone_consistency_score: entropy of rider's GPS ping distribution
    over past 4 weeks (high entropy = works across many zones)
  - earnings_cv: coefficient of variation of rider's weekly earnings
    over their account history
  - week_of_year, monsoon_phase_flag, festival_week_flag

XGBoost Premium Model (inference every Monday 2 AM)
  Input:  Feature vector above (approximately 22 features)
  Output: expected_income_loss_pct (float, 0.0 to 1.0) for
          the upcoming 7-day window
  Converted to RiderScore (0 to 100) and fed into the premium formula.
  Retrained every Monday 2 AM on the previous week's confirmed
  trigger events as labelled ground truth.
  Training framework: scikit-learn pipeline with LightGBM estimator
  (faster for weekly retrains); XGBoost used for interpretability
  in the SHAP explainability layer.

LSTM Disruption Forecast (inference every Sunday 11 PM)
  Input:  90-day rolling sequence of (daily_rainfall, aqi_max,
          disruption_flag, order_count_index) per pin-code cluster
  Architecture: 2-layer unidirectional LSTM, hidden size 64,
                temporal attention layer over the final 7 timesteps
  Note: unidirectional only. Bidirectionality would let the model
  read future timesteps during training — that is data leakage for
  a forward-forecasting task. We caught this early and it mattered.
  Output: P(disruption_day) for each of the next 7 days, per
          pin-code cluster
  Modulates Season_Index in the premium formula and pre-stages
  liquidity reserves before disruptions occur.

Real-Time Trigger Monitor (every 15 minutes)
  - Polls OpenWeatherMap and CPCB for all active pin codes
  - Checks each metric against thresholds
  - On breach: queries dark store order-count mock API
  - If dual-source confirmed: publishes event to Redis pub/sub
  - FastAPI consumer runs Coherence Score check and routes to
    clean payout or provisional payout workflow

SHAP Explainability Layer (post-inference, per rider)
  - Computes SHAP values for each rider's weekly risk score
  - Top 2 contributors rendered as plain-language text on app home screen
  Example: "Your premium is Rs. 12 higher this week because rainfall
  probability in your zone is 68% (up from 22% last week) and you
  worked across 4 different zones last week, which increases your
  earnings variance score."
```

### A note on what Phase 1 actually has

We want to be upfront about this: in Phase 1, neither model has seen a real rider's data. We trained on IMD rainfall archives and synthetic earnings distributions built from NITI Aayog parameters and our three field conversations. The architecture is real. The pricing logic is sound. The numbers the models produce are illustrative, not predictive.

From Phase 2, onboarding survey responses and anonymized GPS activity from real users replace the synthetic features. The model retrains weekly on actual confirmed trigger events. Every week of real data makes it more accurate. But we know the difference between Phase 1 and production, and we think you should too.

---

## Adversarial Defense and Anti-Spoofing Strategy

We added this section because of a scenario that is not hypothetical. A coordinated syndicate of 500 delivery workers in a tier-1 city exploited a competing beta parametric platform. They organized on Telegram, used GPS-spoofing apps to fake their location inside red-alert weather zones while sitting at home, and drained the liquidity pool with mass false payouts. When we read that, we stopped what we were doing and spent two days thinking about whether our architecture had the same vulnerability.

It did. Simple GPS verification is not enough. Here is what we built instead.

---

### The core insight

A GPS coordinate is a claim, not evidence. It tells us where a phone's software says it is. It does not tell us whether the phone is in a rider's hand in a flooded street or on a kitchen table running a spoofing app. The difference is detectable — but only if you look at signals that a spoofing app cannot fake at the same time as GPS.

GigGuard does not try to catch GPS spoofing by verifying GPS better. It catches it by requiring the GPS reading to be consistent with everything else the device is producing.

---

### 1. Genuine stranding vs. spoofed location: the Coherence Score

For every claim event, we compute a Coherence Score from the signal dimensions below. No single dimension flags a claim on its own. The score is a weighted aggregate, and only a pattern of multiple low-coherence signals triggers escalation.

Score above 0.65: clean payout. Score between 0.35 and 0.65: Provisional Payout. Score below 0.35: full freeze pending human review.

| Signal | Genuinely stranded rider | GPS spoofer at home |
|---|---|---|
| Accelerometer variance | Irregular micro-vibrations from road surface, rain, and sheltering movement | Near-zero variance: stationary device on a flat surface |
| Gyroscope readings | Frequent orientation shifts as rider handles phone and changes posture | Flat, consistent: phone lying still |
| Battery drain rate | Elevated: screen on, GPS polling active, data retrying on degraded network | Normal or charging: stable home pattern |
| Cell network signal | Degraded and fluctuating: heavy rain attenuates cell signal physically | Strong and stable: home WiFi or full indoor bars |
| GPS fix quality (HDOP) | Poor: rain causes multipath interference, elevated HDOP | Clean, high-precision coordinates: spoofing apps inject perfect fixes |
| App foreground state | Active: rider checking order queue, interacting with app | Idle: screen off or backgrounded |
| Zone familiarity score | Claimed pin code is in the rider's established 90-day GPS history | Claimed pin code is outside their known delivery corridor |

The HDOP point is worth dwelling on. Professional GPS-spoofing apps inject clean, high-precision coordinates because that is what they are designed to do. A real rider standing in heavy rain gets genuinely degraded GPS — elevated HDOP, multipath error, the signal bouncing off wet surfaces. A clean GPS signal during a severe weather event is, paradoxically, more suspicious than a noisy one.

---

### 2. Detecting a coordinated ring

Individual spoofing is hard to scale invisibly. Fifty to five hundred people all spoofing at once produces population-level anomalies that are detectable regardless of whether any individual rider's Coherence Score looks suspicious.

**Claim surge velocity:** Real disruptions produce a staggered claim curve — riders gradually realise they cannot work and trigger over 40 to 90 minutes. Coordinated fraud produces a sharp simultaneous spike because a single Telegram message went out and everyone acted on it. We monitor claim arrival rate per pin-code cluster. If more than 15% of policyholders in a single pin code file within 20 minutes, the whole batch goes to elevated scrutiny.

**Cross-rider environmental coherence:** In a genuine zone-wide disruption, every rider in the area shows degraded network, poor GPS fix quality, and inactivity patterns consistent with sheltering. If a cluster of 30 riders all show clean network, high GPS quality, and zero accelerometer variance while claiming weather entrapment, that population-level signal is inconsistent with what the environment should be producing.

**Dark store order-count cross-check:** If 40 riders claim weather-induced zero income but the dark store dispatch data shows only a 15% drop, the claim population is inconsistent with platform-side ground truth. No genuine mass disruption leaves the dark store nearly operational while every rider says they cannot work. A confirmed divergence triggers an immediate ring-level investigation.

**6-hour policy seasoning window:** A ring needs to sign up before the event. Genuine riders maintain continuous weekly coverage. Any policy activated within 6 hours of a published IMD red alert is ineligible for payout under that same event. Existing multi-week policyholders are completely unaffected. This closes the most straightforward entry vector without touching a single genuine user.

**Device and registration clustering:** We passively record device fingerprint components at onboarding. A cluster of new registrations with similar fingerprints, within 48 hours of each other, concentrating in the same pin code, matches known syndicate behavior. These accounts are flagged before any claim is made.

---

### 3. Making sure this does not hurt honest riders

This is the part we argued about the most.

The worst outcome for GigGuard is not a successful fraud. It is Karthik, phone battery at 11%, GPS degraded by rain, sitting under a petrol bunk on a flooded street, having his payout frozen because our system read his genuine distress as suspicious. That would be a product failure and a mission failure at the same time.

So the defense is calibrated with a deliberate asymmetry: we are slow to freeze and fast to release. We accept a higher fraud loss rate at the margins before we accept false positives on legitimate claims.

**When a claim is flagged (Coherence Score 0.35 to 0.65):**

**Step 1: 50% is paid immediately.** Within 90 seconds of the flag being raised, half the calculated payout goes to the rider's UPI wallet. A genuine rider in distress is never left with nothing. The other 50% sits in escrow.

**Step 2: Passive 4-hour observation window.** The system keeps collecting sensor and network data silently. No action required from the rider. In most genuine cases, the signals self-correct as the rider moves, the storm passes, and the app comes back to the foreground. Most ambiguous claims resolve without the rider ever knowing there was a question.

**Step 3: Auto-release on recovery.** If the Coherence Score recovers above 0.65 within 4 hours, the held 50% is released automatically. One push notification: "Your full payout has been confirmed and credited." No form. No appeal. Nothing to do.

**Step 4: One optional prompt, for edge cases only.** If signals stay ambiguous past 4 hours, the app sends a single optional message: "A 10-second video of your surroundings can help us release your remaining payout faster." Entirely optional. Declining routes to human review, not rejection.

**Step 5: Human review, 24-hour SLA.** An analyst reviews the full signal record. Account history, zone familiarity, earnings history, and prior claim record all weight toward approval. First-time flagged riders with clean history get the benefit of the doubt, as a policy, not at someone's discretion.

**What we will never do:**
- Auto-reject a claim solely on a low Coherence Score
- Require a rider to prove they were present before receiving any payout
- Penalise future premiums for a single flagged claim that was ultimately approved
- Withhold 100% of a payout while a review is open for claims scored above 0.35
- Treat network degradation in bad weather as evidence of spoofing (it is the opposite)

---

## System Architecture

Here is how the components actually connect, because a list of technologies without architecture is not a design.

**Onboarding path:** React Native app collects phone, Aadhaar, and rider ID. FastAPI calls the Aadhaar verification mock and the platform rider ID validation endpoint. On success: PostgreSQL record created, feature engineering runs for the new rider, LightGBM inference endpoint called, risk score and initial premium quote returned to the app. Target: under 4 seconds.

**Weekly repricing path:** Every Sunday at 11 PM, a FastAPI background task runs the LSTM forecast for all active pin-code clusters, updates zone risk scores from latest IMD quarterly data, recomputes feature vectors for all active riders, runs LightGBM batch inference, writes new `weekly_premium` values to the policy table in PostgreSQL, and pushes Monday morning notifications to all riders 30 minutes before premium deduction.

**Trigger-to-payout path:** The parametric monitor polls OpenWeatherMap and CPCB every 15 minutes. On threshold breach, it queries the dark store mock API for order-count confirmation. On dual-source confirmation, it publishes a `trigger_event` to Redis. A FastAPI consumer subscribes, fetches affected policyholders from PostgreSQL, runs Coherence Score computation per rider, and routes each to clean payout (Razorpay Payout API) or provisional payout workflow. Target: under 15 seconds from Redis publish to UPI credit initiation for clean claims.

---

## Platform Choice

We chose React Native over a web app. This is why.

| Argument | Why it matters here |
|---|---|
| Device reality | 94% of Q-commerce partners use Android exclusively. A web app is the wrong interface for this user. |
| UPI AutoPay | Premium deduction via UPI mandate requires mobile-native integration. Browser-based UPI flows fail at high rates on mid-range Android. |
| Sensor access | Coherence Score computation requires accelerometer and gyroscope data in real time. Only available natively on mobile. A web app cannot access these sensors. |
| Offline resilience | Riders work in basement car parks and low-signal areas. AsyncStorage lets the app cache policy state locally. The rider always sees accurate information regardless of network state. |
| Push notifications | When a trigger fires and money is moving, the rider needs to know immediately. Email achieves the same information transfer with none of the immediacy. |
| Onboarding completion | Camera Aadhaar scan plus OTP completes in under 3 minutes on mobile. Web flows for this persona see 60%+ drop-off. |

---

## Tech Stack

| Layer | Technology | What it does in GigGuard |
|---|---|---|
| Mobile frontend | React Native (Expo SDK 51) | Rider-facing app: onboarding, risk quiz, policy purchase, payout dashboard, sensor data collection |
| Backend API | FastAPI (Python 3.11) | REST endpoints, background task scheduling, ML model serving — one language for everything |
| Premium ML model | LightGBM (training + inference) + XGBoost (Phase 3, SHAP) | Weekly risk scoring per rider; premium calculation; explainability layer |
| Disruption forecast | PyTorch LSTM (unidirectional, 2-layer, temporal attention) | 7-day disruption probability per pin-code cluster; Season_Index modulation |
| Fraud detection | scikit-learn Isolation Forest + Coherence Score engine | Claim anomaly detection; ring-detection at zone level |
| Primary database | PostgreSQL 16 | Riders, policies, trigger events, payouts, feature vectors; ACID-compliant |
| Event streaming | Redis 7 (pub/sub) | Real-time trigger event distribution from monitor to payout consumer |
| Weather data | OpenWeatherMap API (free tier) + IMD Open Data | Live conditions; 5-day forecast for LSTM input |
| AQI data | CPCB Open Data API (data.gov.in) | Hourly station readings; official and free |
| Civic alerts | NDMA feed + mock JSON server (Phase 1) | Red/Orange alert webhooks |
| Platform data | Mock Zepto/Blinkit order-count API (JSON server) | Dark store dispatch volume for dual-source confirmation and ring detection |
| Payments | Razorpay Test Mode | UPI AutoPay mandate; instant payout simulation |
| Deployment | Render + Expo EAS | Free tier; adequate for Phase 1 and 2 demo scale |
| ML operations | MLflow | Reproducible training runs; weekly retrain version control |

---

## Phase 1 Prototype Scope

What judges will see in the 2-minute video:

**Onboarding:** Phone OTP, Aadhaar, platform selection (Blinkit or Zepto), rider ID, primary pin code. Under 90 seconds.

**Risk profiling quiz:** 5 questions covering shift hours, primary zone, average weekly earnings, how many zones covered per week, and preferred payout method. The AI Risk Score screen explains the two biggest factors in plain language, like: "Your score is 67 because heavy rain probability in your zone this week is 62% and you worked across 3 zones last week."

**Weekly policy purchase:** Premium displayed with a breakdown showing Zone Risk, Rider Score, and Season Index components. UPI AutoPay mandate via Razorpay test mode. Policy confirmation with coverage amount, expiry, and the five monitored trigger types.

**Live mock trigger simulation:** Active coverage dashboard. Admin toggle fires a rainfall trigger for Anna Nagar. The rider's screen shows "Rain alert confirmed in your zone" followed by an auto-claim progress bar. Within 47 seconds: "Rs. 480 credited to your UPI." Dashboard updates to show earnings protected this week.

---

## Development Roadmap — Phase 1

### Week 1 (March 4 to 10): Foundation

- [x] Field conversations with 3 Blinkit delivery partners in Anna Nagar and Velachery
- [x] Premium formula finalised with zone risk tiers and seasonal index values
- [x] Trigger thresholds validated against 5 years of IMD Chennai rainfall event data
- [x] GitHub repository and project board set up
- [ ] React Native Expo scaffold with tab navigator
- [ ] FastAPI scaffold with PostgreSQL schema (riders, policies, trigger_events, payouts)
- [ ] OpenWeatherMap and CPCB API keys live with basic polling endpoint
- [ ] Synthetic training dataset: 10,000 rider-weeks of feature vectors using IMD 2019 to 2024 data

### Week 2 (March 11 to 20): Prototype

- [ ] Onboarding screens: phone, OTP, platform, rider ID, pin code
- [ ] Risk Profiling Quiz UI with 5-question flow
- [ ] LightGBM model trained on synthetic dataset; inference endpoint on Render
- [ ] SHAP explanation layer generating plain-language output per rider
- [ ] Weekly Policy screen with dynamic premium display and component breakdown
- [ ] Razorpay test mode UPI AutoPay mandate integration
- [ ] Parametric monitor: OpenWeatherMap + CPCB polling with threshold logic
- [ ] Mock dark store order-count API as JSON server
- [ ] Admin trigger simulation toggle
- [ ] Payout simulation: trigger event to Razorpay Payout API call to push notification
- [ ] Coherence Score engine: accelerometer and gyroscope collection via Expo Sensors
- [ ] Rider dashboard: coverage summary, trigger history, earnings protected
- [ ] README and 2-minute video

---

## Unique Innovations

### 1. Community Risk Pool

Every 50 riders in the same 2-km zone form an automatic Community Risk Pool. Eight percent of each weekly premium goes to that pool's reserve rather than the central float. When a disruption triggers, payouts draw first from the shared reserve. In weeks with no disruption, 30% of unused reserves come back to riders as a credit against the following week's premium.

This does a few things at once. Riders in the same zone have a financial incentive to behave honestly — a fraudulent claim in the pool directly reduces the credit every other member gets back. It also reduces volatility in the central float, because localized events draw from localized reserves. And it makes the product feel like something riders own together, not something that happens to them from above.

### 2. GigGuard Gold

Covered in the premium section. The short version: we can offer rate certainty because our pricing is dynamic, and that is something flat-rate insurance products structurally cannot do. Riders who hit Week 5 of unbroken low-risk operation get their premium frozen for 4 weeks. We think this is one of the more underrated things in the product.

### 3. Dual-Signal Payout

Traditional parametric insurance fires on one sensor reading. If it rains 26mm in 3 hours, everyone in the zone gets paid, whether or not orders actually dropped. GigGuard requires the environmental trigger to be confirmed by the dark store order-count before anything fires. Rain threshold crossed plus dispatch volume down more than 60% equals payout confidence of 0.97. We pay when riders actually lost income, not just when the weather was bad.

This also happens to be one of the cleanest anti-fraud mechanisms we have. A spoofer can fake their location. They cannot fake the dark store's order data.

---

## Business and Social Impact

### The numbers at scale

| Metric | Value |
|---|---|
| Target rider base, Year 1 (Chennai and Bengaluru) | 10,000 riders |
| Average weekly premium per rider | Rs. 48 |
| Monthly premium revenue at full enrollment | Rs. 19.2 lakhs per month |
| Average disruption days per rider per month, peak season | 3.2 days |
| Average payout per disruption day | Rs. 420 |
| Total monthly payout liability, peak season | Rs. 13.4 lakhs |
| Implied loss ratio, peak season | approx. 70% |
| Annual income stabilised across rider base | Rs. 8.4 crore |

A 70% loss ratio at peak is commercially viable for micro-insurance. In off-peak months it drops significantly. The model self-corrects because the XGBoost repricing runs weekly: a high-disruption week immediately raises the following week's premium for affected zones.

### The market

India's Q-commerce sector is projected to reach Rs. 1.2 lakh crore in GMV by 2027. The delivery partner workforce is expected to hit 1.8 million by 2026. At 12% penetration in Tier-1 cities alone, GigGuard's addressable base exceeds 2.16 lakh riders — a Rs. 50 crore annual premium opportunity. We are not claiming we will capture all of it. We are saying the market is large enough that we do not need to.

### Why the model works

Parametric automation means 90% of payouts require zero human intervention. Weekly micro-premiums at Rs. 35 to Rs. 75 are below the psychological resistance threshold — riders compare it to one platform convenience fee, not to an annual insurance renewal. And the dynamic pricing means we cannot be stuck with a bad loss ratio for long: within one week of a bad event, the premium adjusts.

---

## 2-Minute Video

> [Will be uploaded to YouTube as an unlisted link before March 20, 2026 EOD]
>
> The video covers:Live onboarding and risk score walkthrough, weekly policy purchase with premium breakdown, rain trigger simulation showing auto-claim and payout in under 90 seconds, and dashboard update confirming Rs. 480 protected.

---

## Repository and Next Steps

| Item | Link |
|---|---|
| GitHub Repository | `[https://github.com/team-gigguard/gigguard-ai]` ** |
| Phase 1 Demo Video | `[[YouTube unlisted link — added by March 20]](https://youtu.be/B1uT4dOuXXc)` |
| Prototype | `[[Link to be added](https://giggaurdai.vercel.app/)]` |

Phase 2 (March 21 to April 4): live parametric trigger engine with real API polling, Razorpay payout integration with actual test-mode UPI disbursement, and the full Coherence Score pipeline running on real sensor data from the mobile app.

---

