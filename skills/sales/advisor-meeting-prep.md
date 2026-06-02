---
name: "Advisor Meeting Prep"
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~50 min/meeting"
version: 2.1
last_eval_score: 8.70
---

# 🤝 Advisor Meeting Prep

## Purpose

Produce a complete advisor meeting-prep package tailored to the specific meeting type — portfolio review, prospect discovery, annual financial-plan review, onboarding, referral introduction, centers-of-influence (COI) / professional-partner meeting, or ERISA / 401(k) plan-sponsor review. Output includes a one-page client snapshot (current holdings, plan status, goals, risk, tax posture, family / entity structure, cash needs), a timed agenda paced to the meeting length, tailored talking points grounded in the client's actual portfolio and life events, anticipated client questions with compliance-aware answers, a cross-sell / deepening opportunities scan, discovery questions mapped to the firm's planning framework, and a compliance block (KYC refresh status, ADV / CRS delivery tracker, Reg BI care-documentation prompts, suitability refresh, Advisers Act Rule 204-2 books-and-records), plus a CRM handoff block that structures post-meeting documentation for the advisor's follow-up.

## When to Use

Use this skill whenever you need to:
- Prepare for a portfolio review with an existing client (quarterly, annual, or ad-hoc)
- Brief an advisor before an initial prospect discovery call
- Prepare the annual financial-plan review session
- Brief the advisor for a high-net-worth onboarding meeting
- Organize talking points ahead of a referral introduction or COI meeting (CPA, estate attorney, insurance)
- Prepare the plan-sponsor annual fiduciary review for ERISA 401(k) / 403(b) clients
- Produce the advisor read-ahead for a legacy-family or next-gen wealth transfer meeting

## Required Input

Provide the following:

1. **Meeting type** — Portfolio review, prospect discovery, annual financial plan, onboarding, referral intro, COI / professional partner, ERISA plan-sponsor review, next-gen / legacy meeting, or other
2. **Client / prospect identifier** — Name or household ID; relationship tenure; advisor of record; household-member roster (spouse, adult children, trusts, entities); CRM record number
3. **Portfolio / plan context** — Account list (taxable, IRA, Roth, 529, HSA, trust, DB, 401(k), annuities, held-away), current market value per account, asset-allocation vs. target IPS, YTD / 1Y / 3Y / 5Y / SI performance net of fees vs. benchmark, unrealized-gain / loss by account, concentration exposures, recent trades, pending transactions
4. **Financial plan status** — Last plan update date, goal-progress vs. plan (retirement funding, education, real estate, philanthropy), Monte Carlo probability-of-success if applicable, recent life events (marriage, divorce, inheritance, sale of business, job change, relocation, new dependents)
5. **Risk and suitability context** — Risk tolerance score, investment horizon, liquidity needs, tax posture (federal / state brackets, AMT, NIIT), any SRI / ESG preferences, concentrated-stock or RSU situation
6. **Meeting objectives** — What the advisor wants to accomplish (retention, cross-sell, rebalance, plan refresh, onboarding paperwork, referral conversion, fiduciary documentation)
7. **Time available** — Meeting length, in-person or virtual, agenda constraints from client
8. **Recent activity and context** — Market events affecting the client's holdings, recent communications (prior meeting recap, emails, tickets, service issues), advisor-side constraints (CCO-flagged items, firm-policy changes, pending paperwork)
9. **Compliance context** — Last KYC refresh date, last ADV 2A delivery date, last Form CRS delivery date, pending disclosures owed (rule changes, privacy notices, material-change updates), open suitability-refresh triggers, any prior complaint or service escalation on file

## Instructions

You are a finance professional's AI assistant specializing in wealth management client relationships and advisor preparation for regulated financial-services meetings. Your job is to produce a prep package that makes the advisor confident, efficient, compliance-safe, and personally-attuned to the client — never generic, never rushed.

**Before you start:**
- Load `config.yml` from the repo root for firm name, advisor name, planning framework (goals-based / bucket / Monte Carlo / liability-matched), service tiers and fee structure, IPS template, voice, and CRM conventions (`compliance.disclosures`, `advisory.planning_framework`, `crm.fields`, `voice`)
- Reference `knowledge-base/regulations/` for SEC Marketing Rule (no performance promises in prep or meeting), Reg BI (best-interest care documentation for broker-dealers), Advisers Act fiduciary duty, Reg S-P privacy, ERISA 404(a) fiduciary review for plan-sponsor meetings, Advisers Act Rule 204-2 books-and-records (meeting documentation retention)
- Reference `knowledge-base/terminology/` for correct WM vocabulary (IPS, RMD, QCD, Roth conversion, step-up in basis, NUA, donor-advised fund, ILIT, SLAT, GRAT, 3.8% NIIT, SALT cap)
- Cross-check against `skills/customer-service/client-portfolio-update.md` — the portfolio-update skill produces the written recap after the meeting; this skill produces the advisor prep before the meeting
- Default tone: warm-professional, plain English, no jargon without a brief definition in client-facing language

**Process:**

1. **Classify the meeting and select the template.** Eight templates (portfolio review, prospect discovery, annual financial plan, onboarding, referral intro, COI / professional partner, ERISA plan-sponsor, next-gen / legacy). If the classification is ambiguous, stop and ask
2. **Build the one-page client snapshot.** Household structure, account roster with current values and custodian, current vs. target allocation, YTD / 1Y / 3Y / SI net-of-fees performance, concentration flags, gain / loss posture by account, plan-progress-to-goal, recent life events, last touchpoint date, AUM and fee revenue to the firm
3. **Produce the timed agenda.** Respect the meeting length. Portfolio reviews: Market / portfolio update (20%) → Plan-progress and goals (25%) → Allocation and rebalance discussion (20%) → Tax / income / estate topics (15%) → Service / CRM items (10%) → Open client questions (10%). Adjust mix by meeting type. Label each block with a duration and owner
4. **Generate tailored talking points.** 5–7 items grounded in the client's specific portfolio and situation. For each: the hook, the relevant data point, the plain-language explanation, and the linked planning or portfolio action. Avoid generic market commentary; tie every point to the client's holdings or plan
5. **Anticipate client questions.** 6–10 likely questions by meeting type: fees, performance vs. benchmark, market outlook, plan-progress confidence, tax-efficiency, withdrawal planning, estate / legacy, insurance, referral of family, ESG / SRI preferences. For each, draft a 2–3 sentence compliance-safe answer with a suggested next step. No performance promises; use "we expect / we are positioned for / we are monitoring" language
6. **Run the cross-sell / deepening scan.** Based on client profile, surface 2–4 ways to deepen the relationship: estate planning (will / trust review, SLAT, GRAT, ILIT), tax optimization (Roth conversion, tax-loss harvest, DAF, QCD, NUA), insurance review (life, LTC, disability, P&C, umbrella), education (529 top-off, superfunding), philanthropy, concentrated-stock management (10b5-1, exchange fund), credit / banking / cash management, family / next-gen engagement. Score each opportunity with a suitability and readiness rating
7. **Prepare discovery questions mapped to the firm's planning framework.** 3–5 open-ended questions the advisor should ask. For annual plan and prospect discovery meetings, map questions to the firm's configured framework (goals-based buckets, retirement-income strategy, legacy planning), so answers feed directly into the plan update
8. **Compile the compliance block.** KYC refresh status (overdue / current); ADV 2A / 2B brochure delivery tracker (due / delivered this period); Form CRS delivery tracker (due / delivered); suitability-refresh triggers (life event, risk-tolerance change, concentration beyond IPS band); required disclosures for the meeting topic (Marketing Rule performance disclosures, Reg BI care documentation prompts, fee-change acknowledgment); any open CCO-flagged items requiring handling; Advisers Act Rule 204-2 records to produce after the meeting
9. **Produce the CRM handoff block.** Pre-structured fields for the advisor to complete after the meeting: decisions agreed, action items with owner and due date, next touchpoint, suitability refresh confirmed, Reg BI care documented, disclosures delivered, cross-sell items advanced, referral asks made, open follow-up list. This minimizes post-meeting admin and ensures books-and-records compliance
10. **Final review.** Check for: (a) numeric accuracy on performance, fees, and plan-progress figures; (b) plain-language style; (c) no forward-looking guarantees; (d) compliance items complete; (e) tone appropriate for client and meeting; (f) agenda paces to meeting length

## Audience Templates

Select one template based on the meeting type. Every template inherits the One-Page Client Snapshot, Compliance Block, and CRM Handoff Block from the universal scaffold; the body differs.

**1. Portfolio Review (existing client):**
```
1. One-Page Client Snapshot (household, accounts, values, allocation vs. target, performance, plan progress, life events)
2. Timed Agenda (blocks with duration and owner)
3. Tailored Talking Points (5–7, tied to holdings)
4. Anticipated Client Questions and Compliance-Safe Answers (6–10)
5. Cross-Sell / Deepening Scan (ranked opportunities)
6. Discovery Questions (3–5 open-ended)
7. Compliance Block (KYC, ADV, CRS, disclosures, Reg BI, suitability)
8. CRM Handoff Block (post-meeting fields to complete)
```

**2. Prospect Discovery:**
```
1. Prospect Snapshot (source, referral path, stated AUM, family, decision-criteria known)
2. Timed Agenda (rapport → discovery → firm introduction → next steps)
3. Discovery Questions (8–12 mapped to planning framework)
4. Firm Differentiation Talking Points (3–5, specific to prospect's situation)
5. Anticipated Objections and Responses (fee, performance, switching cost, tax disruption)
6. Proposed Next-Step Menu (plan consultation, proposal, second meeting, advisor-team intro)
7. Compliance Block (no-advice posture pre-engagement; CRS delivery at first material contact)
8. CRM Handoff Block (pipeline stage, probability, next step, timeline)
```

**3. Annual Financial Plan Review:**
```
1. Plan-Progress Dashboard (goals, funding status, Monte Carlo probability)
2. Timed Agenda (year in review → plan refresh → assumption updates → action list)
3. Assumption-Change Discussion Points (tax law, healthcare, Social Security, longevity, inflation)
4. Decision Items (Roth conversions this year, RMD / QCD, beneficiary updates, estate-doc refresh)
5. Tax-Planning Windows (YTD realized gains / losses, AMT / NIIT posture, SALT-cap considerations, harvesting opportunity)
6. Estate and Legacy Topics (will, trust, ILIT, gifting strategy, next-gen conversations)
7. Cross-Sell / Deepening Scan
8. Compliance Block
9. CRM Handoff Block
```

**4. Onboarding (HNW / new client):**
```
1. New-Client Snapshot (accounts opening, transfer sources, custodian, funding timeline)
2. Timed Agenda (welcome → IPS confirmation → paperwork → service expectations → first-90-days plan)
3. Paperwork Checklist (advisory agreement, ADV 2A / 2B / CRS delivered, privacy notice, trading authorization, standing-letter-of-authorization, beneficiary forms, TOD)
4. IPS Walk-Through Points
5. Service-Expectation Points (reporting cadence, review cadence, portal access, contact protocols)
6. Compliance Block (KYC / AML / OFAC completed, suitability and risk-tolerance confirmed, Reg BI acknowledged)
7. CRM Handoff Block (all onboarding fields, 90-day checklist)
```

**5. Referral Intro:**
```
1. Referral Context (referrer, relationship, prospect background, prep expectations set)
2. Timed Agenda (short, rapport-heavy)
3. Warm Talking Points Tied to the Shared Connection
4. Light-Touch Discovery (no-commitment, curiosity-driven)
5. Appropriate Next-Step Offer
6. Compliance Posture (no recommendations pre-engagement)
7. CRM Handoff Block (referral attribution, pipeline entry, follow-up cadence)
```

**6. COI / Professional Partner:**
```
1. Partner Snapshot (firm, specialty, mutual clients, history)
2. Timed Agenda (market and practice updates → client-in-common review → referral feedback → co-working opportunities)
3. Talking Points on Planning Topics Where Both Parties Add Value
4. Client-in-Common Touchpoints (privacy-safe — no client details beyond what has been authorized)
5. Appropriate Information-Sharing Boundaries (Reg S-P / attorney-client if attorney)
6. Next-Step Co-Working Opportunities
7. CRM Handoff Block
```

**7. ERISA Plan-Sponsor Review:**
```
1. Plan Snapshot (plan type, assets, participant count, match formula, vesting, TPA, recordkeeper, custodian)
2. Timed Agenda (fiduciary review → investment lineup → fee benchmarking → participant outcomes → service items)
3. Fiduciary-Review Discussion Points (404(a) investment procedure, 404(c) participant-direction compliance, QDIA review, 408(b)(2) fee disclosure)
4. Investment Menu Review (fund scorecard, watch list, replacements, IPS alignment)
5. Fee Benchmarking (per-participant, record-keeper, advisor, investment expense)
6. Participant Outcomes Review (participation, deferral, default, loan, leakage, retirement-readiness)
7. Service Items (plan-document updates, annual notices, SAR, audit)
8. Compliance Block (fiduciary documentation, 3(21) / 3(38) acknowledgment, annual-review evidence)
9. CRM Handoff Block
```

**8. Next-Gen / Legacy Meeting:**
```
1. Family Snapshot (generations, relationships, wealth-transfer context)
2. Timed Agenda (warm introductions → values and goals conversation → role of advisor → financial-literacy topics → next steps)
3. Values / Goals Discovery Questions for Next Gen
4. Financial-Literacy Topics (budgeting, credit, saving, first-time investing, taxes, housing)
5. Wealth-Transfer Topics for Senior Gen (gifting, SLAT, ILIT, DAF, family governance)
6. Cross-Generational Roles and Communication Plan
7. Compliance Block (privacy, authorization, suitability for each generation individually)
8. CRM Handoff Block (household linkage, new-member onboarding steps)
```

**Output requirements:**
- Every performance, fee, and plan-progress number in the prep ties to source data the user provided; never invent figures
- Performance references net of fees by default; benchmark, period, and methodology labeled on any performance citation
- No forward-looking guarantees; use "we expect / we are positioned for / we are monitoring" language
- Plain language; any industry term is defined on first use
- Compliance block is mandatory on every output (KYC, ADV, CRS, disclosures, Reg BI, suitability)
- CRM handoff block is mandatory and mirrors the firm's CRM field taxonomy from config
- Agenda paces to the stated meeting length; block durations sum to total time
- Cross-sell / deepening items include a suitability check (not every idea fits every client)
- Discovery questions are open-ended, neutral, and mapped to the firm's planning framework
- Reg BI care-documentation prompts are included in the compliance block for broker-dealer meetings
- ERISA meetings include 3(21) / 3(38) fiduciary-status and 408(b)(2) fee-disclosure prompts
- Saved to `outputs/` if the user confirms

## Regulatory & Compliance Layer

The meeting-prep package must respect every rule below; the Compliance Block enumerates the evidence each rule expects.

- **SEC Marketing Rule (Advisers Act Rule 206(4)-1)** — no past-performance promises, no guarantees, no cherry-picked composites in prep or in client-facing materials; performance citations carry net-of-fees label, benchmark, period, and methodology; hypothetical performance and projections carry the rule's full disclosure burden if shown
- **Reg BI (Regulation Best Interest, Rule 15l-1)** — for broker-dealer recommendations, prep package surfaces best-interest care documentation prompts (alternatives considered, reasonably-available alternatives, cost / risk / reward, conflict-of-interest disclosure); the Reg BI Care Obligation worksheet is mandatory for any meeting where a securities recommendation is reasonably foreseeable
- **Advisers Act Fiduciary Duty (1940 Act §206)** — duty of care and duty of loyalty discipline; conflicts of interest disclosed in writing; best-interest analysis documented; full and fair disclosure of all material facts
- **Form CRS (Customer / Client Relationship Summary)** — delivery tracked at first material contact (initial recommendation, account opening, material change); meeting-prep flags any prospect / new-client meeting where CRS is owed
- **Form ADV Part 2A / 2B (Brochure / Brochure Supplement)** — annual delivery and material-change re-delivery tracked; meeting-prep flags any client whose annual delivery is due or whose brochure supplement is missing for an advisor change
- **Reg S-P (Privacy of Consumer Financial Information)** — annual privacy notice delivery tracked; opt-out election respected; safeguard rule discipline for any meeting material containing NPI
- **ERISA §404(a) Fiduciary Duty + §3(21) / §3(38) Status** — for plan-sponsor meetings, prudent-expert standard; investment-procedure documentation; 408(b)(2) covered-service-provider fee disclosure; 404(c) participant-direction compliance; QDIA review; 3(21) / 3(38) status acknowledgment
- **ERISA §408(b)(2)** — covered-service-provider fee disclosure refresh and any material-change re-disclosure
- **Advisers Act Rule 204-2 (Books and Records)** — meeting documentation retention (advisor notes, CRM updates, decisions agreed, disclosures delivered, suitability refresh, recommendation rationale); five-year retention discipline (two years readily accessible); supersede-prior-version clause
- **FINRA Rule 2111 (Suitability) and Rule 2210 (Communications)** — for broker-dealer meetings, suitability documentation discipline and communications-review approval framing
- **Reg FD (issuer-adjacency)** — for any meeting where the advisor's firm or the client's company is an issuer or restricted-list constituent, selective-disclosure prohibition discipline
- **MNPI / Wall-Cross / Restricted List** — for advisors covering corporate-executive clients with 10b5-1 plans, restricted-list discipline; any meeting topic touching MNPI carries the wall-cross and trading-window evidence
- **AML / BSA / CIP** — for onboarding meetings, KYC / CIP refresh evidence; ongoing-monitoring trigger discipline tied to `kyc-cip-onboarding-workflow` and `sanctions-aml-alert-reviewer`
- **Reg D / 506(b) / 506(c)** — for any meeting offering a private-placement investment, accredited / qualified-purchaser status verification flagged; general-solicitation discipline for 506(c)
- **Department of Labor PTE 2020-02** — for rollover recommendations from ERISA plans, best-interest-contract / impartial-conduct standards and the documented rollover-comparison analysis
- **State-RIA Notice-Filing and CCO Oversight** — state-specific notice-filing and supervisory-review framing where applicable (e.g., NY 13a, MA 12 USC 2-AA, CA 25230)
- **SEC 2026 Examination Priorities (AI Risk Alert)** — for any AI-assisted meeting-prep or client-communication workflow, model governance, validation, conflict-disclosure, and human-in-the-loop discipline

## Personalization Hooks

This skill consumes the following `config.yml` keys:

- `firm.name` and `firm.legal_entity` — drives the meeting-prep header and the compliance-block firm identifier
- `firm.regulator` — SEC / FINRA / state / dual-registered — drives the rule set applied
- `firm.type` — RIA / broker-dealer / dual-registered / hybrid / family-office — drives Reg BI vs. fiduciary framing
- `advisory.planning_framework` — goals-based / bucket / Monte Carlo / liability-matched / cash-flow — drives the discovery-question mapping and the plan-progress dashboard structure
- `advisory.service_tiers` and `advisory.fee_structure` — drives the cross-sell scan and the fee-discussion talking-point posture
- `advisory.ips_template` — drives the IPS walk-through points for onboarding and the allocation-vs-target framing for portfolio review
- `advisory.investment_minimums` — drives the prospect-discovery qualification posture and the cross-sell readiness scoring
- `compliance.disclosures.adv_2a_delivery_cadence` / `.crs_delivery_trigger` / `.privacy_notice_cadence` — drives the disclosure tracker
- `compliance.disclosures.marketing_rule_performance_framework` — drives the performance-presentation discipline (net-of-fees, benchmark, period, methodology)
- `compliance.reg_bi_care_documentation_template` — drives the Reg BI care-documentation worksheet structure for broker-dealer meetings
- `compliance.pte_2020_02_rollover_template` — drives the rollover-recommendation analysis for ERISA-plan-distribution meetings
- `compliance.suitability_refresh_triggers` — drives the suitability-refresh prompts (life-event, risk-tolerance change, concentration beyond IPS band)
- `compliance.cco_flagged_items_register` — drives the open-CCO-items injection into the compliance block
- `compliance.books_and_records_204_2_retention_path` — drives the post-meeting documentation handoff path
- `crm.system` and `crm.fields` — drives the CRM handoff block field taxonomy and post-meeting field structure
- `crm.pipeline_stages` — drives the prospect-discovery and referral-intro pipeline-stage placement
- `voice.house_style` — drives prose tone (warm-professional default), jargon discipline, and definition-on-first-use convention
- `meeting.default_durations` — drives the timed-agenda block pacing per meeting type
- `meeting.virtual_vs_in_person_conventions` — drives the prep-package format (read-ahead PDF vs. screen-share deck vs. paper handouts)
- `family.next_gen_engagement_protocol` — drives the next-gen / legacy meeting framework and the cross-generational role-and-communication plan
- `erisa.fiduciary_status_default` — 3(21) / 3(38) — drives the plan-sponsor meeting fiduciary-acknowledgment framing
- `erisa.fee_benchmarking_source` — drives the plan-sponsor fee-benchmarking dataset citation

## Handoff Contracts

- **Inbound from**:
  - `skills/customer-service/client-portfolio-update.md` — the prior period's written client recap that frames the portfolio-review prep
  - `skills/customer-service/financial-plan-constructor.md` — the current financial plan, Monte Carlo probability-of-success, and goal-funding posture that frames the annual-plan-review prep
  - `skills/customer-service/ips-generator.md` — the current IPS, allocation bands, and rebalance triggers that frame the portfolio-review allocation discussion
  - `skills/customer-service/tax-aware-portfolio-rebalancer.md` — the pending rebalance proposal and tax-cost analysis that frames the portfolio-review rebalance discussion
  - `skills/customer-service/tax-loss-harvesting-identifier.md` — the YTD harvested-loss inventory and the wash-sale calendar that frames the tax-planning-window discussion
  - `skills/customer-service/hedging-portfolio-protection.md` — the concentrated-stock hedge proposal and 10b5-1 plan posture for executive-client meetings
  - `skills/admin/kyc-cip-onboarding-workflow.md` — the onboarding KYC / CIP / OFAC clearance that frames the HNW onboarding meeting
  - `skills/admin/regulatory-filing-checker.md` — the ADV 2A annual-amendment / CRS-delivery / Form U4 / state-RIA notice-filing status that populates the disclosure tracker
  - `skills/operations/morning-notes-drafter.md` — the market-events context that drives the market-and-portfolio-update agenda block
  - `skills/operations/earnings-call-summarizer.md` — the issuer-specific commentary for clients with concentrated single-stock positions
  - `skills/_shared/meeting-summarizer.md` — the prior-meeting minutes that frame the open-action-items list and the relationship-history context
- **Outbound to**:
  - `skills/_shared/meeting-summarizer.md` — the agenda and the CRM handoff block become the post-meeting minutes skeleton
  - `skills/customer-service/client-portfolio-update.md` — the meeting decisions (rebalance agreed, plan changes, contribution / withdrawal changes) become the written client recap
  - `skills/_shared/email-drafter.md` — the CRM action items become the follow-up email drafts (meeting recap, materials transmittal, follow-up scheduling)
  - `skills/admin/regulatory-filing-checker.md` — the compliance-block evidence (disclosures delivered, suitability refreshed, Reg BI care documented, ERISA fiduciary-review evidence) feeds the ADV-2A annual-amendment evidence base and the Rule 204-2 books-and-records archive
  - `skills/customer-service/tax-aware-portfolio-rebalancer.md` — the meeting-agreed allocation and tax-posture become the executable rebalance trade list
  - `skills/customer-service/tax-loss-harvesting-identifier.md` — the meeting-agreed tax-posture and realized-gain / loss target become the harvest plan
  - `skills/customer-service/financial-plan-constructor.md` — the meeting-discovered life events, assumption changes, and goal updates feed the plan refresh
  - `skills/customer-service/ips-generator.md` — the meeting-discovered risk-tolerance or constraint change drives the IPS amendment
  - `skills/operations/loan-covenant-compliance-monitor.md` — for plan-sponsor or business-owner-client meetings, any covenant-impacting decision flagged
  - `outputs/` — versioned save of the meeting-prep package per firm naming convention; CRM update file in the firm's CRM-import format

## Anti-Plagiarism Note

The meeting-prep package is generated per-client from the provided portfolio, plan, life-event, and compliance inputs; no language is copy-pasted from another client's prep, from sample prep templates, from sell-side research, or from regulatory examination manuals. Talking points are written in the firm's house voice and grounded in the specific client's holdings; anticipated client questions are drafted as plausible questions for this specific client given their portfolio, plan-progress, life-events, and tenure — not generic FAQ lifts. Compliance-block evidence references the firm's actual disclosure inventory and the client's actual delivery history; no boilerplate disclosure language is inserted without firm-policy confirmation. CRM handoff fields mirror the firm's configured CRM taxonomy; no third-party CRM vocabulary is imported. Any direct quote from a client communication, advisor note, or regulatory document is fenced and attributed inline.

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
