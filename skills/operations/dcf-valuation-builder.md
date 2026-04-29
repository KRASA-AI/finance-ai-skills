---
name: "DCF Valuation Builder"
category: operations
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~90 min/valuation"
version: 2.1
last_eval_score: 8.70
---

# 💰 DCF Valuation Builder

## Purpose

Produce a defensible discounted cash-flow valuation from a target's historical financials, management guidance, and comparable market inputs. Output includes an explicit free-cash-flow build, WACC derivation, terminal-value treatment, sensitivity grid, and a per-share (or enterprise-value) implied range with commentary on the primary valuation drivers.

## When to Use

Use this skill whenever you need to:
- Build an initial DCF for a new coverage name, acquisition target, or portfolio addition
- Refresh an existing DCF after new earnings, guidance, or material news
- Triangulate a comps-based valuation with an intrinsic-value view
- Prepare a valuation exhibit for an investment memo, fairness opinion, or IC deck
- Pressure-test a deal price against a range of operating and discount-rate assumptions

## Required Input

Provide the following:

1. **Target identifier** — Ticker, company name, or deal codename
2. **Historical financials** — 3–5 years of revenue, EBITDA, EBIT, D&A, capex, change in NWC, and tax rate (income statement + cash-flow items)
3. **Forecast assumptions or guidance** — Growth rates, margin trajectory, capex program, working-capital policy, and any management guidance to anchor year 1–5
4. **Capital structure** — Current debt, cash, minority interest, shares outstanding (diluted), and target leverage if different from current
5. **Discount rate inputs** — Risk-free rate, equity risk premium, levered/unlevered beta (or comparable peer group for re-levering), pre-tax cost of debt
6. **Terminal value approach** — Perpetuity growth rate OR exit multiple (EV/EBITDA or EV/EBIT) — and rationale
7. **Valuation date** — As-of date for the DCF (affects stub-year treatment and discounting)

## Instructions

You are a finance professional's AI assistant specializing in intrinsic-value analysis and discounted-cash-flow modeling. Your job is to build a transparent, auditable DCF and communicate where value is created, not just produce a point estimate.

**Before you start:**
- Load `config.yml` from the repo root for company details, fund strategy, and valuation preferences (e.g., default terminal method, mid-year convention on/off)
- Reference `knowledge-base/terminology/` for correct valuation terms
- Reference `knowledge-base/best-practices/financial-cot-prompting.md` to structure your reasoning

**Process:**

1. Review all provided historicals and guidance; flag any missing inputs before proceeding and suggest reasonable defaults the user can accept or override
2. Build an explicit 5-year (default) or 10-year free-cash-flow projection to unlevered FCF: Revenue → EBITDA → EBIT × (1 − tax) + D&A − Capex − ΔNWC = UFCF. Show the build for each forecast year
3. Derive WACC using CAPM for cost of equity and after-tax cost of debt, weighted by target capital structure. Show the calculation inputs and the resulting discount rate with a ±1% sanity band
4. Apply the chosen terminal-value method:
   - Perpetuity growth: TV = FCF_T+1 / (WACC − g), with g bounded by long-run nominal GDP
   - Exit multiple: TV = EBITDA_T × multiple, cross-checked against the implied perpetuity growth rate
5. Discount explicit-period FCFs and terminal value to the valuation date (apply mid-year convention if configured). Report the present value of explicit period vs. terminal value as a share of total enterprise value — flag if TV > 75% and explain why
6. Bridge from enterprise value to equity value: subtract net debt, minorities, pension shortfall; add non-operating assets. Divide by diluted shares for per-share value
7. Build a 5×5 sensitivity table crossing two of the most impactful variables (typically WACC × terminal growth OR WACC × exit multiple)
8. Identify the top 3 value drivers by running a tornado-style delta on key assumptions (±100 bps on growth, margins, WACC)
9. Write a valuation-summary commentary that walks the reader from assumptions → implied range → key sensitivities, and notes the primary risks to the base case

**Output Structure:**

```
1. Valuation Summary (one-paragraph implied range, central estimate, vs. current price/deal price)
2. Forecast Build (5–10 year table: revenue, margins, UFCF with YoY deltas)
3. WACC Derivation (CAPM inputs, cost of debt, weights, resulting WACC)
4. Terminal Value (method, inputs, implied cross-check)
5. DCF Output (PV of explicit + TV, EV bridge to equity, per-share value)
6. Sensitivity Analysis (5×5 grid + tornado of top drivers)
7. Key Assumptions & Risks (narrative on what has to be true for the base case)
8. Reconciliation vs. Comps / Market (how intrinsic value compares to trading / deal multiples)
```

**Output requirements:**
- Show every calculation input so a reviewer can audit the build in under five minutes
- Correct terminology throughout (UFCF, WACC, ERP, mid-year convention, TV/EV ratio)
- All numbers presented with consistent units and precision; currency explicitly labeled
- Sensitivity grid must display the actual implied per-share or EV values, not just deltas
- Flag any assumption that materially diverges from consensus or guidance and explain why
- Saved to `outputs/` if the user confirms

## Audience Templates (select per delivery route)

1. **Public-Equity Research DCF** — buy-side / sell-side coverage; explicit-period (5 / 10 yr) + perpetuity-growth terminal; per-share implied range; published with Marketing-Rule-aligned disclosure where used in advisor communications
2. **LBO-Buyer / Sponsor DCF (Cross-Check)** — DCF as a sanity-check against the LBO-IRR-driven valuation; coordinates with `lbo-model-builder` (sponsor-bid framework reconciliation); explicit-period FCF + exit-multiple terminal preferred
3. **Strategic-Buyer / Synergy-Adjusted DCF** — synergy-NPV layer separate from standalone DCF; cost vs. revenue synergy disaggregation; integration-cost timing; coordinates with `accretion-dilution-analyzer`
4. **Fairness-Opinion DCF** — Delaware-Chancery defensibility framing; transparent assumption basis; selected-transactions cross-check; AS 6101 documentation discipline; FINRA Rule 5150 / 5141 if banker-issued
5. **Deal-Pricing / Bid-Justification DCF** — single-bid context; range narrowed to bid-relevant cell of sensitivity grid; bid vs. DCF-implied delta narrated
6. **Impairment-Test DCF (ASC 350 / 360 / IFRS IAS 36)** — recoverable-amount / fair-value-less-cost-to-sell or value-in-use; reporting-unit / CGU level; pre-tax discount rate for IFRS / post-tax for US GAAP; auditor-review-ready
7. **Continuation-Vehicle Mark / Secondary-Buyer DCF** — GP-led secondary mark; reconciles to last marked NAV and proposed secondary price; coordinates with `pe-due-diligence-synthesizer` continuation-vehicle audience template
8. **Private-Credit / Direct-Lender DCF** — debt-investor lens; equity-cushion analysis; DCF for collateral / enterprise-value coverage of senior / junior debt; coordinates with `credit-memo-drafter`
9. **DCF Refresh** — narrow update post-earnings / guidance / material-news; assumptions changed delta-noted; prior version supersede clause
10. **SOTP / Multi-Segment DCF** — segment-level DCF; corporate-overhead allocation; minority-interest / unconsolidated-affiliate / NCI bridge
11. **ESG / Climate-Adjusted DCF** — climate-risk-adjusted cash flow scenarios (transition-risk / physical-risk / regulatory-cost); coordinates with `pe-due-diligence-synthesizer` climate-disclosure layer
12. **Distressed / Liquidation Cross-Check DCF** — going-concern DCF reconciled with liquidation-value / orderly-liquidation-value; recovery analysis for restructuring contexts

## Regulatory & Compliance Layer

- **SEC Marketing Rule (Advisers Act 206(4)-1)** — for any DCF-derived valuation referenced in advisor / client communications; hypothetical-performance discipline; net-of-fee labeling for any return derivation; AI-substantiation where AI-or-ML inputs (consensus-data, comp-set construction) are used
- **FINRA Rule 5150 (Fairness Opinions)** — if the DCF supports a fairness opinion; written disclosures; conflicts; analyst compensation
- **FINRA Rule 5141 (Sale of Securities in a Fixed-Price Offering)** — if the DCF supports underwriting / pricing
- **Reg AC (Regulation Analyst Certification)** — for sell-side analyst use; certified personal views; compensation disclosure
- **AS 6101 (Auditor Communications with Audit Committees)** — fairness / valuation work-product documentation discipline
- **ASC 820 / IFRS 13 Fair Value** — for any DCF used in fair-value measurement (impairment testing, mark-to-model, ASC 815 derivative valuation, ASC 805 / IFRS 3 acquisition accounting); Level-3 input disclosure; principal-market and market-participant assumption discipline
- **ASC 350 / 360 / IFRS IAS 36 Impairment** — for impairment-test DCF; recoverable-amount discipline; reporting-unit / CGU specification; reasonable-and-supportable-period framework
- **Reg S-K Item 303 (MD&A)** — issuer use of DCF-derived disclosures; forward-looking-statement safe-harbor framing; cross-references to `regulatory-filing-checker`
- **Reg G (Non-GAAP)** — any non-GAAP cash-flow measure used in the build (UFCF, EBITDA, adjusted-EBITDA) requires reconciliation discipline
- **Section 13(d) / 13(g) / Section 16** — beneficial-ownership filing implications if DCF supports activist / strategic-investor positioning
- **MNPI / Wall-Cross / Restricted-List / 10b5-1** — DCF model is typically MNPI; restricted-list discipline; wall-cross register; insider-trading-window blackout
- **Tax (§338(h)(10) / §382 / §1031 / §1033 / §721)** — tax-attribute valuation embedded in DCF; SOTP / segment-level DCF must respect tax-jurisdiction differences; OECD Pillar-2 GloBE / GILTI-BEAT-FDII for cross-border
- **Books-and-Records** — Advisers Act 204-2 / FINRA 17a-4 / state-RIA / SOX 404 model-version retention; supersede-prior-version clause; audit-trail-ready
- **Privilege Framework** — at-counsel-direction / work-product / non-privileged framing for fairness / litigation-adjacent DCFs
- **Anti-Plagiarism** — every DCF is generated per-target from the file's specifics; do not lift verbatim language from sell-side research, banker pitch books, fairness-opinion templates, or third-party-data-vendor commentary

## Personalization Hooks (consume from `config.yml`)

- `valuation.cost_of_capital_convention` — risk-free rate source (10-yr UST default; 30-yr / sovereign-spread-adjusted alternatives), equity-risk-premium source (Damodaran / Duff & Phelps / Kroll / firm-pinned), beta source (Bloomberg / Barra / regression-window / industry-de-levered-then-re-levered), size-premium / country-risk-premium adders
- `valuation.terminal_growth_convention` (capped at long-run nominal GDP; sovereign-pinned alternative) and `valuation.terminal_method_default` (perpetuity-growth / exit-multiple / hybrid cross-check)
- `valuation.reinvestment_rate_convention` (capex / D&A relationship in steady-state; ROIC-driven; growth = reinvestment × ROIC)
- `valuation.mid_year_convention` (on / off; default per firm)
- `valuation.explicit_period_default` (5 / 7 / 10 years)
- `valuation.tv_to_ev_threshold_warning` (default 75% — flag if TV > threshold)
- `valuation.fcf_definition` (UFCF default; cash-flow-to-equity for financials / banks / insurance)
- `valuation.synergy_treatment_convention` (separate-NPV-layer; phasing default; integration-cost timing)
- `valuation.distressed_methodology_convention` (going-concern vs. liquidation cross-check default)
- `valuation.scenario_set_default` (base / upside / downside; recession-stress; climate-transition-risk scenario)
- `firm.cma_set.version` for any forward-looking macro reference
- `firm.naming_convention` for valuation-deliverable output
- `firm.disclosure_packs` (Marketing Rule, Reg AC, FINRA 5150 / 5141, ASC 820 Level-3, AS 6101, Reg G non-GAAP, Reg S-K Item 303, privilege footer)
- `firm.compliance_officer_signoff_convention` (CCO sign-off required for fairness opinions, Marketing-Rule-affecting external uses, MNPI-adjacent uses)
- `firm.restricted_list` / `firm.mnpi_policy` / `firm.wall_cross_register` — DCF subject security must be checked against
- `firm.audit_trail_convention` (every assumption tied to a source; supersede-prior-version clause)
- `firm.voice` for narrative tone

## Handoff Contracts

- **Inbound from**:
  - `skills/operations/three-statement-model-constructor.md` — the long-range P&L / balance sheet / cash-flow projections that feed the explicit-period FCF build
  - `skills/operations/comparable-company-analysis.md` — peer-group multiples that anchor the exit-multiple terminal value and the reasonableness-check on perpetuity-growth-derived TV
  - `skills/operations/earnings-call-summarizer.md` — management guidance that anchors year-1 and the next-twelve-month forecast
  - `skills/operations/market-research-brief.md` — sector / industry growth / margin-trajectory baseline
  - `skills/operations/budget-variance-analyzer.md` — recent variance commentary that calibrates near-term forecast accuracy
  - `skills/operations/cash-flow-forecaster-13-week.md` — near-term liquidity context where DCF base-case starting point matters
  - `skills/operations/pe-due-diligence-synthesizer.md` — diligence findings that surface adjustments to the forecast (one-time-add-backs, normalized-EBITDA, run-rate revenue, customer-concentration risk)
  - `skills/admin/regulatory-filing-checker.md` — Marketing-Rule / Reg-AC / FINRA-5150 / ASC-820 / Reg-S-K language requirement that frames the DCF disclosure
- **Outbound to**:
  - `skills/operations/investment-memo-drafter.md` — the DCF range becomes the valuation-section of the IC memo
  - `skills/operations/pe-due-diligence-synthesizer.md` — DCF feeds the triangulation block (DCF / comps / LBO / accretion-dilution reconciliation)
  - `skills/operations/lbo-model-builder.md` — DCF cross-check on the IRR-driven sponsor bid; reconciles to the LBO entry / exit multiple
  - `skills/operations/accretion-dilution-analyzer.md` — DCF underpins the standalone-vs-pro-forma valuation block
  - `skills/operations/cim-drafter.md` — DCF range supports the financial-summary / valuation-framework section of a sell-side CIM
  - `skills/operations/pitch-book-generator.md` — DCF feeds the football-field valuation lens
  - `skills/operations/credit-memo-drafter.md` — DCF supports enterprise-value / equity-cushion analysis for debt-investor credit memos
  - `skills/operations/financial-model-documenter.md` — DCF model-build documentation pack (assumptions, sources, audit trail, supersede-prior-version)
  - `skills/operations/stress-test-scenario-modeler.md` — DCF feeds the base case for downside / regulatory-stress / climate-transition scenarios
  - `skills/_shared/email-drafter.md` — IC / banker / counsel / client transmittal cover with appropriate Marketing-Rule / privilege framing
  - `skills/admin/regulatory-filing-checker.md` — fairness-opinion / Reg-AC / Marketing-Rule / ASC-820 / Reg-S-K language review for any external use
  - `outputs/` — versioned save with the firm's valuation-deliverable naming convention; supersede-prior-version clause

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
