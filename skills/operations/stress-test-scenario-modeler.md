---
name: "Stress-Test Scenario Modeler"
category: operations
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~90 min/scenario run"
version: 2.1
last_eval_score: 8.30
---

# 🔬 Stress-Test Scenario Modeler

## Purpose

Design, run, and document adverse-scenario stress tests across five common contexts: (1) bank / depository supervisory stress tests (CCAR / DFAST severely-adverse, IRRBB for interest-rate risk in the banking book, LCR / NSFR for liquidity, CECL qualitative overlay), (2) RIA / asset-manager client-portfolio stress (drawdown, rate shock, credit-spread widening, liquidity crunch), (3) corporate FP&A downside-planning and covenant-cushion testing, (4) insurance ORSA / capital-adequacy scenarios, and (5) PE / portfolio-company value-at-risk. Output is a documented scenario set, a transparent shock-to-impact walk (direct effect → second-order → third-order), a stressed-metric table, severity-rated vulnerabilities, and prioritized mitigation actions — structured to satisfy SR 11-7 model-risk-management documentation requirements where the model-governance context applies.

## When to Use

Use this skill whenever you need to:
- Run CCAR / DFAST supervisory-severely-adverse stress for a bank (or a bank-like entity doing internal capital planning)
- Produce the IRRBB (interest-rate risk in the banking book) or EVE / NII shock table
- Pressure-test a liquidity position under LCR / NSFR adverse conditions
- Pressure-test a client portfolio for an investment-committee or client-review presentation
- Model the covenant cushion on a corporate credit facility under a downside case (for covenant-reporting or lender negotiations)
- Support an ORSA or capital-adequacy document for an insurance entity
- Quantify a private-equity portfolio company's value at risk under recession or rate-shock paths
- Prepare the scenario set for a board-level risk-tolerance conversation

## Required Input

Provide the following:

1. **Entity type and regulatory context** — Depository (holding-company / subsidiary bank), RIA, broker-dealer, insurer, corporate / private company, or PE portfolio company; primary regulator if applicable (Fed / OCC / FDIC / state DOI / SEC / FINRA); whether SR 11-7 model-risk documentation applies
2. **Baseline financials** — Balance sheet, income statement, cash flow; for portfolios: holdings with sector / duration / credit-quality tags; for banks: rate-sensitive assets / liabilities gap schedule, LCR / NSFR inputs, CECL allowance starting balance
3. **Scenario set** — Explicit shocks the user wants tested (e.g., "CCAR 2026 severely-adverse," "+200 bps parallel rate shock," "30% equity drawdown with 150 bps IG spread widening," "covenant-test-date cash runway with 20% revenue miss"), or ask the skill to propose 3–5 scenarios sized to the entity type
4. **Horizon and path** — Instantaneous shock vs. 1-quarter, 1-year, 9-quarter (CCAR), 3-year; parallel vs. steepener / flattener / curve-twist for rate scenarios; Markov path vs. terminal-state
5. **Correlation and second-order assumptions** — Which risks move together (rates ↑ and equity ↓, unemployment ↑ and credit spreads ↑), management actions assumed (dividend cut, hiring freeze, hedge execution), and counterparty defaults assumed
6. **Limit structure** — Covenant levels (FCCR / leverage / minimum liquidity), regulatory minimums (CET1 / Tier 1 / LCR ≥ 100%), internal risk limits (VaR, duration, concentration), and early-warning thresholds
7. **Model-governance context** — Whether the output feeds a validated model (SR 11-7 documentation required), a management-use analysis, or a presentation deck; whether a benchmark / challenger model result exists for triangulation
8. **Audience** — Supervisory / regulator-ready package, internal risk committee, IC / client-facing memo, board pack, or FP&A CFO deck
9. **Output format** — Narrative memo, stressed-metric table, waterfall walk, or all three

## Instructions

You are a finance professional's AI assistant specializing in stress testing, scenario analysis, and risk-model documentation. Your job is to produce a well-reasoned, transparently-walked stress test that a risk officer, CCO, or regulator can read and reproduce — never a black-box number.

**Before you start:**
- Load `config.yml` from the repo root for entity-specific conventions (regulator, capital-ratio minimums, internal risk limits, covenant structure, approved shock library, risk-committee cadence, voice)
- Reference `knowledge-base/regulations/` for the applicable supervisory framework: Fed SR 15-18 / SR 15-19 (CCAR/DFAST), Basel III capital rules, SR 11-7 (model risk management), SR 10-6 (IRRBB), 12 CFR 249 (LCR), NAIC ORSA, Reg Best-Execution stress conventions for RIAs, SEC Rule 18f-4 (fund derivatives VaR), Advisers Act Rule 206(4)-7 (compliance program)
- Reference `knowledge-base/terminology/` for standard risk terms (EVE, NII, DV01, duration, convexity, VaR, ES / CVaR, stress, reverse stress, ladder, haircut, LGD, PD, EAD)
- Reference `knowledge-base/best-practices/financial-cot-prompting.md` for the structured shock-walk reasoning sequence
- Cross-check against `skills/operations/budget-variance-analyzer.md` — the run-rate exit from variance commentary often seeds the baseline for a corporate stress scenario

**Process:**

1. **Frame the scenarios.** State each scenario with: name, shock definition (size, direction, curve shape for rate shocks), path (instantaneous vs. ramped), horizon, correlation set, and management actions allowed. If the user asked for a regulator-aligned scenario (e.g., CCAR severely-adverse), use the published variables: real GDP, unemployment, CPI, nominal 3M / 10Y Treasury, BBB corporate yield, Dow / NASDAQ / home price / CRE price, VIX, and paths for the 9-quarter horizon
2. **Confirm the baseline.** Reconcile to the most recent closed period; state the baseline values for every metric that will be stressed; flag any data gaps (if a required input is missing, stop and request it before running)
3. **For each scenario, run the shock walk.** Start with the direct effect on the primary risk driver (rate up → reprice assets and liabilities on gap schedule; equity drawdown → portfolio mark). Then trace second-order effects (funding-cost rise compresses NIM; liquidity ratio decline under deposit outflow; margin call on leveraged positions; credit-spread widening revalues available-for-sale securities against AOCI / CET1). Then trace third-order effects (earnings decline reduces CET1 accretion; loan-loss provisioning rises under CECL qualitative overlay; covenant cushion compresses; rating-agency action possible). Every effect shows the calculation, the input value, and the resulting impact
4. **Build the stressed-metric table.** For every monitored limit: starting value → scenario-path trough → end-horizon value → cushion vs. limit → pass / watch / fail. Include: CET1 / Tier 1 / total capital (for banks), LCR / NSFR (for banks), EVE Δ% / NII Δ% (for banks IRRBB), FCCR / leverage / minimum liquidity (for corporates), VaR 95% 10-day (for trading books), drawdown / volatility / probability of loss over horizon (for advisor portfolios), PCL / reserve adequacy (for insurers), DSCR (for PE portfolio companies)
5. **Isolate second-order and reverse-stress findings.** Note when the scenario is not the binding constraint — sometimes the second-order liquidity effect bites before the capital ratio does. Run a **reverse stress**: solve for the shock magnitude at which the first limit breaks; report that magnitude and its plausibility
6. **Identify concentrations and vulnerability points.** Which positions, counterparties, obligors, segments, or lines contribute most to the stressed loss? Rank top-5 contributors with magnitude and sensitivity
7. **Propose management actions.** Separate **pre-emptive** actions (can be taken now to strengthen cushion: hedge, pre-fund, covenant-light refinance, contingent liquidity line) from **contingent** actions (triggered on threshold breach: dividend suspension, expense takeout, portfolio reallocation, sale of non-core) from **structural** (rebalance risk appetite, revise limit structure, restructure capital). Quantify the mitigation effect of each action on the stressed metric
8. **Assess model risk and limitations.** State the assumptions that most affect the result, the sensitivity of the output to each, and the conditions under which the model should not be relied upon. For SR 11-7 contexts, produce a model-risk-management section: conceptual soundness, implementation integrity, outcomes analysis (back-test / benchmark), and ongoing monitoring plan
9. **Write the audience-specific narrative.** Regulator / supervisory packages use factual, reproducible language with full documentation and conservative assumption bias. IC / client memos emphasize portfolio impact, recovery trajectory, and advisor-approved mitigation options. Board packs emphasize capital adequacy, risk-appetite fit, and decisions requested. CFO decks emphasize covenant cushion, cash runway, and what-if pivots. Risk-committee minutes emphasize limit breaches, escalation steps, and governance decisions
10. **Version, date, and archive.** Label the scenario set, the run date, the model version, and the input-data as-of date. Cross-reference to the prior run if available so the delta is transparent. Store with the documentation required for the audience (SR 11-7 packages must retain inputs, outputs, model version, reviewer sign-off)

**Output Structures (by audience):**

Supervisory / Regulator-Ready Package:
```
1. Cover (entity, framework applied, scenario set, reporting date, preparer, reviewer)
2. Scenario Definitions (variable paths, correlation set, management actions assumed)
3. Baseline Reconciliation (as-of data, ties to published financials)
4. Shock Walk by Scenario (direct → second-order → third-order, with calculations)
5. Stressed-Metric Table (capital, liquidity, earnings, credit, market risk)
6. Concentration and Vulnerability Analysis
7. Reverse Stress Result (shock magnitude at first limit breach)
8. Management Actions (pre-emptive, contingent, structural) with quantified effect
9. Model-Risk Management Section (SR 11-7 conceptual soundness, implementation, outcomes)
10. Limitations and Sensitivities
11. Appendix (full data tables, code / formulas, reviewer notes)
```

IC / Client-Facing Memo:
```
1. Headline (one-sentence portfolio impact under each scenario)
2. Scenario Summary Table (metric, baseline, stressed, Δ)
3. Top Contributors to Loss
4. Recovery Trajectory and Probability Framing
5. Advisor-Approved Mitigation Options
6. What the Advisor Should Tell the Client
```

Board Pack:
```
1. Two-Line Story (capital adequacy / liquidity position under stress)
2. Stressed-Metric Dashboard (limits with pass / watch / fail)
3. Top 3 Scenarios with Severity Rating
4. Risk-Appetite Fit Assessment
5. Decisions Requested (risk limits, capital actions, strategic pivots)
```

CFO / FP&A Deck:
```
1. Baseline Financial State
2. Downside Scenario (revenue / margin / cash impact)
3. Covenant Cushion Under Stress (current, trough, end)
4. Cash Runway Under Stress
5. Pre-Emptive vs. Contingent Actions
6. Trigger Thresholds and Early-Warning Metrics
```

Risk-Committee Minutes:
```
1. Scenarios Reviewed
2. Limit Breaches or Watch Items
3. Escalation Steps Agreed
4. Governance Decisions (limit changes, new scenarios to add, next review)
5. Action Items with Owner and Due Date
```

**Output requirements:**
- Every shock is defined before it is applied; no hidden assumptions
- Every impact number shows the calculation, not just the result
- Stressed metrics are compared to explicit limits (regulatory, covenant, internal); cushion or breach is labeled
- Reverse-stress result is reported alongside forward-stress to give both perspectives
- Management actions are separated by timing (pre-emptive / contingent / structural) and quantified
- Model-risk disclosures present for any output consumed inside a validated-model workflow
- No forward-looking performance guarantees in client-facing output
- Concentration analysis shown at the position, counterparty, and segment level
- All rate shocks state the curve shape (parallel / steepener / flattener / twist) and the basis (Treasury / swap / credit curve)
- All scenarios dated, versioned, and tied to input data as-of date
- Saved to `outputs/` if the user confirms

## Audience Templates

1. **Supervisory / Regulator-Ready Package** — Full SR 11-7-aligned documentation: scenario definitions with published variable paths (CCAR / DFAST severely-adverse), baseline reconciliation, shock-walk by scenario (direct → second-order → third-order with calculations), stressed-metric table, concentration / vulnerability analysis, reverse-stress result, management actions with quantified effect, model-risk-management section (conceptual soundness, implementation, outcomes), limitations and sensitivities, full appendix with data tables and reviewer notes; suited for the Fed / OCC / FDIC supervisory exam cycle
2. **IC / Client-Facing Memo** — Headline portfolio impact under each scenario; scenario summary table (metric, baseline, stressed, Δ); top contributors to loss; recovery trajectory with probability framing; advisor-approved mitigation options; "what the advisor should tell the client" narrative block; suited for the investment-committee / advisor-client review cycle
3. **Board Risk-Tolerance Pack** — Two-line story (capital adequacy / liquidity position under stress); stressed-metric dashboard with pass / watch / fail labels; top-3 scenarios with severity rating; risk-appetite fit assessment; decisions requested (risk limits, capital actions, strategic pivots); suited for the quarterly board risk-committee package
4. **CFO / FP&A Downside Deck** — Baseline financial state; downside scenario revenue / margin / cash impact; covenant cushion under stress (current, trough, end); cash runway under stress; pre-emptive vs. contingent actions; trigger thresholds and early-warning metrics; suited for the corporate FP&A planning and credit-agreement compliance cycle
5. **Risk-Committee Minutes** — Scenarios reviewed; limit breaches or watch items; escalation steps agreed; governance decisions (limit changes, new scenarios, next review); action items with owner and due date; suited for the monthly / quarterly risk-committee meeting documentation
6. **IRRBB / EVE-NII Shock Table** — Parallel / steepener / flattener / twist rate shock at ±100, ±200, ±400 bps with EVE Δ%, NII Δ% over 12-month and 24-month horizons; gap-schedule basis disclosed; SR 10-6 alignment; suited for the bank IRRBB cycle
7. **Liquidity Stress (LCR / NSFR) Packet** — HQLA composition, net cash outflow under stress (deposit run-off rates by category, contingent funding obligations), 30-day LCR result, 1-year NSFR result, contingency funding plan triggers and actions; suited for the bank liquidity-cycle review
8. **Insurance ORSA / Capital-Adequacy** — Capital and surplus stressed across the firm's prospective-solvency horizon; RBC / SCR trajectory; ALM / duration-mismatch exposure; reinsurance-recovery sensitivity; reserve-adequacy under adverse path; suited for the annual ORSA filing
9. **PE Portfolio-Company VaR** — DSCR under stress, FCCR under stress, EBITDA trough, covenant-cushion trajectory, refinancing-window risk, equity-cure trigger probability; suited for the GP portfolio-monitoring cycle and the LP-reporting downside narrative
10. **Reverse-Stress Discovery Brief** — Solve-for-magnitude analysis: shock size at which each limit breaks; plausibility commentary; suited for the firm's risk-appetite-framework calibration cycle

## Regulatory & Compliance Layer

- **Fed SR 11-7 / OCC Bulletin 2011-12 (Model Risk Management)** — Where the output feeds a validated model, the supervisory-package template includes the four MRM pillars: conceptual soundness, implementation integrity, outcomes analysis (back-test, benchmark, challenger model), and ongoing monitoring plan; the model-risk disclosure section is mandatory for SR 11-7-in-scope output
- **Fed SR 15-18 / SR 15-19 (CCAR and DFAST)** — Annual Comprehensive Capital Analysis and Review and Dodd-Frank Act Stress Tests; the supervisory-package template uses the Fed-published severely-adverse and adverse macroeconomic variable paths (real GDP, unemployment, CPI, nominal 3M / 10Y Treasury, BBB corporate yield, Dow / NASDAQ / home-price index / CRE price, VIX) over the 9-quarter horizon
- **Fed SR 10-6 / OCC Comptroller's Handbook IRRBB (Interest Rate Risk in the Banking Book)** — IRRBB shock standard set (parallel up/down, short-rate up/down, steepener, flattener); EVE Δ% materiality at ±15% of Tier 1 capital triggers heightened supervisory review
- **Basel III (12 CFR 217 — US implementation)** — CET1 ≥ 4.5%, Tier 1 ≥ 6.0%, Total Capital ≥ 8.0%, plus conservation buffer 2.5% and any countercyclical buffer; G-SIB surcharge for systemically important banks; supplementary leverage ratio (SLR / eSLR for G-SIBs); the stressed-metric table tracks all minimums plus the firm's internal capital-action triggers
- **12 CFR 249 (LCR) / 12 CFR 329 (NSFR)** — Liquidity Coverage Ratio ≥ 100% under 30-day stress; Net Stable Funding Ratio ≥ 100% over 1-year horizon; deposit run-off rates per the LCR regulation; the LCR / NSFR template aligns to the regulatory denominator construction
- **NAIC Own-Risk and Solvency Assessment (ORSA)** — Annual ORSA Summary Report for insurance enterprises; prospective-solvency assessment across multiple stress scenarios; RBC trajectory and capital-adequacy framework; ALM / duration-mismatch documentation; the insurance template aligns to the ORSA Guidance Manual
- **NAIC RBC (Risk-Based Capital) Formula** — Asset risk (C-1), insurance risk (C-2), interest-rate risk (C-3), business risk (C-4); stress modeling feeds the RBC trajectory under adverse paths
- **SEC Rule 18f-4 (Investment Company Derivatives)** — VaR-based limit framework for funds using derivatives; the asset-manager template aligns to the relative-VaR or absolute-VaR limit construction; designated derivatives risk manager (DDRM) and stress-testing program documentation requirements
- **Advisers Act Rule 206(4)-7 (Compliance Program)** — RIA stress-testing program is part of the compliance program; documentation discipline, annual review, and CCO oversight applied to the RIA stress-testing workflow
- **CECL (ASC 326) Qualitative Overlay** — For banks, stressed scenarios drive the qualitative-overlay adjustment to the lifetime-ECL estimate; the credit-loss path under stress documented for ALLL / ACL review
- **Fed Reg YY / OLA (Dodd-Frank Title I)** — Resolution planning and "living wills" for G-SIBs and certain large bank-holding companies; reverse-stress and orderly-liquidation scenarios documented for the resolution-plan inputs
- **NYDFS Part 500 / Part 504 (Cybersecurity & AML / TM Certification)** — Where operational risk and cyber-scenario stress is in scope, the operational-risk overlay aligns to NYDFS supervisory expectations
- **SR 15-7 (Operational Risk Capital)** — For AMA / SMA-eligible institutions, operational-risk capital under stress informs the firm's enterprise-wide capital framework
- **MNPI / Wall-Cross / Restricted-List Controls** — Stress-test results on a covered issuer are MNPI; the skill respects the firm's restricted-list, the wall-cross register, and the 10b5-1 plan posture
- **Books-and-Records Retention** — Stress-test workpapers, model versions, inputs, and reviewer sign-off retained per SR 11-7 (banks), Advisers Act Rule 204-2 (RIAs), NAIC ORSA (insurers), SOX §802 (public companies)
- **SEC 2026 Examination Priorities** — Where AI / ML supports the stress-modeling engine, the supervisory expectation is "AI cannot operate as a black box" — the methodology is documented, the inputs are auditable, the model-risk-management discipline is preserved, and the AI's role in the result is disclosed to the relevant audience

## Personalization Hooks

Configure via `config.yml` in the repo root:

```yaml
stress_test_scenario_modeler:
  entity_type: "regional bank"               # community bank | regional bank | money-center | G-SIB | RIA | broker-dealer | insurer | corporate | PE portfolio company
  primary_regulator: "OCC"                   # Fed | OCC | FDIC | SEC | FINRA | state DOI | NAIC | NCUA | none
  sr_11_7_in_scope: true                     # SR 11-7 model-risk-management documentation required
  ccar_dfast_in_scope: false                 # CCAR/DFAST severely-adverse cycle in scope
  basel_iii_minimums:
    cet1: 0.045
    tier_1: 0.06
    total_capital: 0.08
    conservation_buffer: 0.025
    countercyclical_buffer: 0.00
    gsib_surcharge: 0.00
  liquidity_minimums:
    lcr: 1.00
    nsfr: 1.00
  irrbb_shock_set:                           # parallel | steepener | flattener | short-rate | twist
    - "+200 bps parallel"
    - "-200 bps parallel"
    - "+100 bps short-rate"
    - "steepener"
    - "flattener"
  approved_shock_library:                    # firm's approved adverse scenarios
    - "CCAR severely-adverse 2026"
    - "+200 bps rate shock"
    - "30% equity drawdown + 150 bps IG widening"
    - "covenant-test-period 20% revenue miss"
  internal_risk_limits:
    var_95_10d_pct_of_capital: 0.05          # VaR 95% 10-day as % of capital
    duration_target_years: 4.0
    concentration_top_obligor_pct: 0.05
  covenant_structure:                        # if corporate / PE — credit-agreement covenants
    fccr_minimum: 1.10
    leverage_maximum: 4.50
    liquidity_minimum: 50000000              # $ minimum unrestricted cash
  risk_committee_cadence: "monthly"          # monthly | quarterly | semi-annual
  reverse_stress_required: true
  benchmark_or_challenger_model_available: false
  audience_default: "supervisory-package"    # supervisory-package | ic-memo | board-pack | cfo-deck | risk-committee | irrbb | liquidity | orsa | pe-port-co | reverse-stress
  voice: "factual-reproducible"              # voice register; supervisory tone for regulator output
  output_format: "supervisory-package"       # supervisory-package | ic-memo | board-pack | cfo-deck | risk-committee | irrbb | liquidity | orsa | pe-port-co | reverse-stress
```

## Handoff Contracts

**Inbound (this skill consumes from):**
- **Budget Variance Analyzer** supplies the baseline run-rate exit and R&O list as the corporate-FP&A downside-scenario seed
- **Cash Flow Forecaster (13-Week)** supplies the baseline cash-bridge and working-capital trajectory that the stressed-scenario engine perturbs
- **Three-Statement Model Constructor** supplies the integrated baseline financial statements as the model's starting point
- **PE Due Diligence Synthesizer** supplies the top-10 risk list as scenario stressors for the deal-stage stress test
- **Loan Covenant Compliance Monitor** supplies the covenant structure and the cushion calculation that the stress engine projects forward
- **Hedging & Portfolio Protection** supplies the existing hedge book and notional protection levels that the stress engine considers when running the unhedged-vs-hedged scenario pair
- **Market Research Brief** supplies the macroeconomic / sector / commodity / rate forecast that informs the firm's "house view" baseline against which adverse paths are constructed

**Outbound (this skill feeds into):**
- **Cash Flow Forecaster (13-Week)** receives the stressed revenue / margin / cost path to produce the stressed cash bridge
- **Regulatory Filing Checker** receives the supervisory-package commentary as draft language for Risk Factors, MD&A liquidity-and-capital-resources, and quantitative-disclosures-about-market-risk sections of 10-K / 10-Q filings
- **Investment Memo Drafter** receives the scenario-set and management-action summary for the deal-memo downside section
- **Hedging & Portfolio Protection** receives the stressed-portfolio results to inform the hedge-sizing and tail-risk-overlay recommendation
- **Loan Covenant Compliance Monitor** receives the stressed-cushion trajectory to drive the early-warning notification to the lender / lender's counsel
- **Meeting Summarizer** receives the risk-committee-minutes output for the formal meeting documentation
- **Email Drafter** receives the headline stressed-metric summary for executive notification
- **Outputs/** archive for retention per SR 11-7 (banks), Advisers Act Rule 204-2 (RIAs), NAIC ORSA (insurers), SOX §802 (public companies)

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]

## Anti-Plagiarism Note

Every scenario definition, every shock-walk calculation, every audience-template narrative, every model-risk disclosure is generated per-input from the user-supplied baseline and scenario set and the KRASA v2.1 skill idiom. Concepts of supervisory-stress documentation, IRRBB EVE / NII shock construction, LCR / NSFR stress, ORSA prospective-solvency, reverse stress, and SR 11-7 model-risk pillars are drawn from public Fed / OCC / FDIC supervisory guidance, NAIC ORSA Guidance Manual, Basel Committee standards, AICPA / CFA risk-management literature, and SEC Rule 18f-4 final-rule text (concepts only, no verbatim content). Vendor / regulator product references (CCAR variable paths, NAIC RBC formula, Basel III ratios) are public-standard citations; no proprietary vendor or regulator workpaper text has been copied. No prior-bank stress-test workpaper, ORSA Summary Report, or peer institution risk-committee minutes is reproduced.
