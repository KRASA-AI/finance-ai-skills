---
name: "LBO Model Builder"
category: operations
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~2.5 hr/model"
version: 2.1
last_eval_score: 8.70
---

# 🏦 LBO Model Builder

## Purpose

Build a defensible leveraged buyout model from a target's financials, a sponsor's proposed deal terms, and a financing structure. Output includes a sources-and-uses table, tranche-by-tranche debt schedule with cash sweep, 5-year operating projection, returns waterfall (IRR and MOIC across hold periods), credit metrics trajectory, and a sensitivity grid on entry multiple, exit multiple, and leverage. This is the companion intrinsic-lens to the DCF skill for PE deal evaluation.

## When to Use

Use this skill whenever you need to:
- Screen a new PE target and generate a first-pass returns view
- Pressure-test a sponsor's proposed bid against financing and return thresholds
- Evaluate how different capital structures (leverage, tranche mix, PIK toggle) change equity returns
- Prepare an IC-ready returns exhibit with base / upside / downside cases
- Refresh an existing LBO after diligence updates EBITDA, capex, or working-capital assumptions
- Compare two competing bids or financing packages on a like-for-like basis

## Required Input

Provide the following:

1. **Target identifier** — Deal codename, company name, or ticker
2. **Historical financials** — 3 years of revenue, EBITDA, D&A, capex, change in NWC, tax rate, and LTM EBITDA as the purchase-price anchor
3. **Operating forecast** — 5-year revenue growth, EBITDA margin path, capex as % of revenue, NWC as % of revenue, and cash-tax rate
4. **Entry assumptions** — Purchase price (or entry EV/EBITDA multiple), transaction fees (advisory, financing, OID), management rollover, and closing date
5. **Financing structure** — For each tranche: size, rate (fixed or SOFR + spread), tenor, amortization schedule, prepayment / call features, PIK toggle, and any revolver size and commitment fee
6. **Cash sweep policy** — Mandatory excess-cash sweep percentage, order of priority across tranches, and any minimum-cash balance
7. **Exit assumptions** — Exit year (base: year 5), exit multiple methodology (EV/EBITDA, same as entry, or step-down), and transaction-cost assumption at exit
8. **Management incentive plan** (optional) — MIP size, vesting structure, ratchet thresholds
9. **Valuation date** — As-of date for the model (affects cash balance, debt outstanding, stub period)

## Instructions

You are a finance professional's AI assistant specializing in leveraged-buyout modeling and sponsor returns analysis. Your job is to build a transparent, auditable LBO with fully tied debt schedules and a clear path from operating assumptions to equity returns.

**Before you start:**
- Load `config.yml` from the repo root for fund conventions (default IRR thresholds, preferred return style, hurdle / carry waterfall if needed)
- Reference `knowledge-base/terminology/` for correct LBO terms (MOIC, DPI, cash-on-cash, leverage ratios)
- Reference `knowledge-base/best-practices/financial-cot-prompting.md` to structure reasoning across the three interconnected engines (operating, debt, returns)

**Process:**

1. Confirm all inputs; flag missing items and propose sponsor-grade defaults (e.g., 50–60% excess cash sweep, 1% commitment fee, SOFR + 475 bps on a first lien TLB) that the user can accept or override
2. Build the **Sources & Uses** table: equity cheque + each debt tranche = purchase price + refinanced debt + fees + min-cash-at-close. Verify sources = uses and show the equity cheque and implied leverage at close (net debt / LTM EBITDA)
3. Build the **5-year operating projection** to EBITDA, EBIT, unlevered FCF, and FCF available for debt service. Show revenue build, margin progression, D&A, capex, change in NWC, and cash taxes
4. Build the **debt schedule tranche by tranche**:
   - Beginning balance → mandatory amortization → optional prepayment from cash sweep → ending balance
   - Interest expense (average of beginning and ending balance for each period) on a cash vs. PIK basis
   - Revolver draws/repayments driven by the min-cash-balance constraint
   - Honor the prepayment priority order strictly (usually first lien before second lien before mezz/PIK)
5. Project the **credit metrics** each year: net leverage (Net Debt / EBITDA), total leverage, interest coverage (EBITDA / Interest), FCCR (EBITDA − Capex / Interest + Mandatory Amort), and fixed-charge coverage. Flag any covenant breach against user-provided or standard (e.g., 6.0x maintenance net leverage) thresholds with the year and cushion percentage
6. Build the **returns engine**: exit EV at year 5 = EBITDA_exit × exit multiple → less net debt at exit → equity proceeds → apply MIP dilution → sponsor equity proceeds. Compute IRR and MOIC for years 3, 4, 5, 6, and 7 to show hold-period sensitivity
7. Build a **sensitivity grid** (typical 5×5): exit multiple × entry multiple, or exit multiple × EBITDA growth, reporting both IRR and MOIC. Also run a tornado of the top five drivers (EBITDA growth, exit multiple, entry multiple, leverage, cost of debt)
8. Layer an **upside / base / downside** case summary (e.g., ±200 bps revenue growth, ±150 bps margin) with IRR and MOIC at base hold year
9. Write an **IC-ready commentary**: deal thesis in two sentences, how returns are driven (earnings growth vs. multiple vs. deleveraging, each as a % of MOIC), key diligence risks, and three break-the-deal sensitivities the reader should test

**Output Structure:**

```
1. Deal Summary (entry EV, purchase multiple, leverage at close, equity cheque, base-case IRR / MOIC)
2. Sources & Uses (line-item table; verify sources = uses)
3. Operating Model (5-year P&L, FCF available for debt service)
4. Debt Schedule (per tranche: BoP, draws/amort/sweep, EoP, interest expense)
5. Credit Metrics (leverage, coverage, covenant cushion by year)
6. Returns Waterfall (EV bridge, equity proceeds, MIP dilution, IRR / MOIC by hold year)
7. Sensitivity Analysis (5×5 IRR grid, 5×5 MOIC grid, tornado of top drivers)
8. Case Comparison (upside / base / downside; IRR, MOIC, year-5 leverage)
9. Returns Attribution (% of MOIC from EBITDA growth, multiple change, deleveraging)
10. Key Risks & Diligence Flags
```

**Output requirements:**
- Show every calculation input so a reviewer can audit the build in under ten minutes
- Separate cash and PIK interest; never bury PIK in operating cash flow
- Keep debt schedule fully tied: beginning balance + movements = ending balance, every year, every tranche
- Returns attribution must reconcile: contribution from growth + multiple + deleveraging ≈ MOIC change vs. 1.0x
- All multiples, rates, and percentages explicitly labeled; currency consistent
- Flag any covenant breach, minimum-cash violation, or negative ending-cash in red and explain
- Saved to `outputs/` if the user confirms

## Audience Templates (select per deal context)

1. **Club-Deal / Sole-Sponsor Screening** — First-pass screening model; simplified sources-and-uses; 5×5 IRR / MOIC sensitivity grid at entry × exit multiple; IC-ready headline metrics (IRR, MOIC, year-5 leverage); go / no-go recommendation with three break-the-deal sensitivities
2. **Take-Private LBO** — Public-company acquisition; include public-to-private premium analysis; stub-period handling; Rule 13e-3 going-private transaction context; minority-shareholder consideration; financing timeline relative to HSR / FTC filing milestones
3. **Continuation Vehicle / NAV-Financing** — End-of-fund-life re-underwriting; LP liquidity options (rollover vs. tender); NAV-based facility sizing; continuation-period return distribution; GP alignment structure; coordinates with `fund-administration-nav-reviewer`
4. **Carve-Out / Corporate-Division LBO** — Standalone-cost adjustments (stranded-overhead, TSA cost-burden, IT/HR/ERP stand-up); carve-out working-capital normalization; parent-company guarantees and stranded liabilities; standalone revenue adjustments
5. **Dividend Recap / Re-Leveraging** — Post-acquisition recap; incremental debt sizing relative to current leverage; FCCR / leverage covenant headroom check post-recap; dividend waterfall through the existing structure; IRC §1371 / §305 for S-corp / preferred implications if applicable
6. **Minority-Equity / Growth Equity with Ratchet** — Minority stake; no-control premium; liquidity drag (lack-of-marketability); co-investor rights (tag-along, drag-along, ROFR, ROFO); ratchet step-up at IRR hurdle thresholds; coordinates with `investment-memo-drafter`
7. **Model Refresh / Diligence-Update** — Narrow refresh after new diligence findings; actuals-vs.-model bridge on EBITDA, capex, NWC; updated credit-metrics trajectory; re-run sensitivity grids; delta commentary for the IC refresh memo
8. **Banker Staple-Financing Pack** — Sell-side lender-staple financing proposal; market-clearing leverage and pricing for each tranche; projected DSCR / FCCR headroom; forms the lender-side companion to the buy-side model for deal-pricing calibration

## Regulatory & Compliance Layer

- **SEC Marketing Rule (206(4)-1) — AI substantiation** — Any LBO return attribution used in fund marketing or LP reporting must comply with the Marketing Rule's substantiation requirements; AI-generated IRR / MOIC outputs must be clearly labeled as model-based estimates with stated assumptions; no presentation of LBO results as guaranteed outcomes; coordinates with `regulatory-filing-checker`
- **FINRA Rule 5150 (Fairness Opinions) / Rule 5141 (Fixed-Price Offerings)** — Where the LBO model underpins a fairness opinion or a registered securities offering (e.g., take-private, leveraged recapitalization), the quantitative analysis must satisfy FINRA 5150 / 5141 disclosures and procedural requirements
- **Section 13(d) / (g) / Section 16 Ownership Thresholds** — Post-close ownership of >5% or >10% of a public-company target triggers Section 13(d) / (g) filing and Section 16 short-swing-profit obligations for insider sponsors; coordinate timing with `regulatory-filing-checker`
- **MNPI / Wall-Cross / Restricted-List / 10b5-1** — The LBO model is typically produced under an NDA and constitutes MNPI; restricted-list implications for any listed debt or equity securities of the target; wall-cross protocols before the model is shared across trading desks; 10b5-1 plan implications for any insider trading window
- **Tax Structuring — IRC Key Sections** — §338(h)(10) / §336(e) deemed-asset-sale elections (change in tax basis of target's assets; affects D&A step-up and UFCF); §382 NOL carryforward limitations (change-of-ownership trigger; limits NOL usage against future income); §1374 built-in-gains tax (S-corp conversions); §197 intangible-asset amortization (15-year for goodwill and customer lists in an asset or deemed-asset deal); IRC §163(j) business-interest-expense deduction limitation (relevant to highly-levered LBOs); OECD Pillar-2 / GILTI-BEAT-FDII for cross-border structures; state-and-local apportionment implications for multi-state targets
- **Reg D / Rule 144A / Rule 145** — Financing-side securities offered to QIBs via Rule 144A require offering memoranda; any equity rollover by management under a sponsored deal may implicate Rule 145 registration requirements; coordinate with `regulatory-filing-checker`
- **ASC 805 / IFRS 3 Purchase Accounting** — Post-close GAAP financials must reflect purchase-price allocation (PPA): fair-value step-ups to inventory, PP&E, intangibles (customer lists, trade names, technology); deferred-revenue haircut; contingent-consideration liability; the amortization of PPA step-ups flows through the post-close P&L and must be stripped from Adjusted EBITDA in the model
- **Books-and-Records Retention** — Model files and IC materials are books-and-records subject to retention requirements per Advisers Act Rule 204-2 (RIA) / FINRA 17a-4 (BD); version the model with date and deal-name stamp; retain source assumptions and sensitivity grids per firm policy
- **Privilege Framework** — LBO models and IC materials prepared with or for counsel may be attorney-client privileged or attorney-work-product protected; label appropriately; do not distribute externally without counsel review
- **Anti-Plagiarism** — All model structure, output templates, and IC commentary generated from inputs and the KRASA v2.1 idiom; do not lift verbatim from third-party sell-side models, lender-staple financing memos, or GP deal documents

## Personalization Hooks (consume from `config.yml`)

- `fund.sponsor_name`, `fund.fund_name`, `fund.vintage_year` — populates the IC memo and deal-code headers
- `fund.return_hurdle_irr` (sponsor's minimum-acceptable IRR; used to flag base-case scenarios below threshold)
- `fund.return_hurdle_moic` (minimum-acceptable MOIC across hold years)
- `fund.preferred_return_style` (simple vs. compounded preferred return for carry / waterfall)
- `lbo.capital_structure_convention` (default tranche stack: 1L TLB / 2L TL / unitranche / mezz / PIK / preferred equity; sponsor's typical market position for each)
- `lbo.excess_cash_sweep_default` (default sweep %; 50% or 75% TLB sweep is common)
- `lbo.minimum_cash_at_close` (minimum cash balance at closing; affects equity cheque sizing)
- `lbo.management_equity_rollover_convention` (% of management equity rolled; MIP size as % of equity; ratchet-threshold default)
- `lbo.dividend_recap_convention` (minimum leverage cushion before a recap is modeled; financing-cost assumption for incremental debt)
- `lbo.sponsor_return_hurdle` (IRR floor for IC approval; used to auto-flag base-case scenarios below threshold)
- `lbo.leverage_policy` (maximum closing net leverage × EBITDA per fund mandate)
- `lbo.exit_multiple_methodology` (same-as-entry / step-down / market-calibrated)
- `lbo.mip_size_default` (MIP as % of equity at par; default ratchet thresholds)
- `firm.cost_of_debt_convention` (SOFR-based spread + OID structure; default spread by tranche tier)
- `firm.tax_rate_default` (blended cash-tax rate applied to UFCF; state-and-local add-on)
- `firm.model_build_convention` (mid-year convention yes/no; signing convention; rounding; non-cash-item handling)
- `firm.restricted_list` / `firm.mnpi_policy` / `firm.wall_cross_register` — governs which deal names can be modeled and shared
- `firm.audit_trail_convention` (source-tied assumption log; version-stamp requirements)
- `firm.naming_convention` (deal-name, date-stamp, version-number format for model output files)
- `firm.disclosure_packs` (LP-reporting template for fund-level IRR attribution)

## Handoff Contracts

- **Inbound from**:
  - `skills/operations/three-statement-model-constructor.md` — EBITDA, capex, ΔNWC, cash taxes, and the revenue / margin driver map are the primary operating inputs to the LBO model
  - `skills/operations/dcf-valuation-builder.md` — DCF-derived intrinsic value provides the entry-multiple sanity check; WACC used as a proxy for minimum-return hurdle; SOTP as a segment-entry multiple anchor
  - `skills/operations/comparable-company-analysis.md` — Entry and exit multiple calibration from the trading- and transactions-comps set
  - `skills/operations/pe-due-diligence-synthesizer.md` — Diligence findings (revenue quality, churn, cost structure, capex intensity, NWC normalization, key-person risk) update the operating assumptions in the LBO model
  - `skills/operations/market-research-brief.md` — Macro / sector revenue-growth and margin-benchmark assumptions that anchor the model's operating forecast
  - `skills/operations/earnings-call-summarizer.md` — Public-company comps' post-quarterly EBITDA trend for exit-multiple and comparable-company benchmarking
  - `skills/operations/credit-memo-drafter.md` — Lender-side debt sizing, pricing, and covenant terms that inform the debt schedule
  - `skills/admin/regulatory-filing-checker.md` — MNPI / wall-cross / restricted-list status; Reg D / Rule 144A / Rule 145 considerations for any registered securities component
- **Outbound to**:
  - `skills/operations/dcf-valuation-builder.md` — IRR / MOIC triangulation: LBO equity returns vs. DCF-derived equity value provides the buy / sell price discovery range
  - `skills/operations/accretion-dilution-analyzer.md` — If the LBO buyer is a public strategic acquirer, the deal structure feeds the EPS accretion / dilution analysis
  - `skills/operations/investment-memo-drafter.md` — Deal summary, returns bridge, credit-metrics trajectory, and key risks consumed as the IC memo's quantitative section
  - `skills/operations/cim-drafter.md` — For sell-side assignments, the lender-model outputs feed the financing section of the CIM
  - `skills/operations/pitch-book-generator.md` — Sponsor pitchbook returns-bridge slide and deal-thesis summary
  - `skills/operations/financial-model-documenter.md` — Model documentation pack: assumptions, source ties, sensitivity-grid interpretation, audit trail
  - `skills/operations/stress-test-scenario-modeler.md` — LBO base-case operating projection consumed as the starting point for downside / severe-downside stress scenarios
  - `skills/operations/credit-memo-drafter.md` — Debt schedule and credit-metrics trajectory consumed as the lender credit-memo's quantitative inputs (leverage, coverage, covenant cushion)
  - `skills/admin/regulatory-filing-checker.md` — Ownership-threshold / Section 13(d)(g)(16) review; Marketing Rule substantiation for any fund-marketing use of LBO return attribution
  - `skills/_shared/email-drafter.md` — IC memo transmittal; banker / lender communication with deal-summary attachment
  - `skills/_shared/meeting-summarizer.md` — IC meeting minutes capturing deal approval, vote, dissents, and action items
  - `outputs/` — Versioned save with deal codename, date-stamp, and version number per `firm.naming_convention`

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
