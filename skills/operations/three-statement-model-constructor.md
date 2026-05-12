---
name: "3-Statement Model Constructor"
category: operations
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~3 hr/model"
version: 2.1
last_eval_score: 8.70
---

# 📐 3-Statement Model Constructor

## Purpose

Build a fully integrated, tied-out 3-statement financial model (Income Statement → Cash Flow Statement → Balance Sheet) from a target's historical filings and a forward assumption set. Output includes a clean historical restate, a 5-year forecast with a consistent driver logic, a working-capital and PP&E roll-forward, a debt and interest schedule, and a balance-sheet check that ties to zero in every period. This skill is the upstream companion to the DCF Valuation Builder, LBO Model Builder, and Accretion/Dilution Analyzer — all of which assume a 3-statement foundation exists.

## When to Use

Use this skill whenever you need to:
- Stand up a 3-statement model for a new coverage name, deal, or diligence workstream
- Refresh an existing model after a 10-Q / 10-K / earnings release with new actuals
- Validate an analyst-built model for balance-sheet tie-out, circularity, or driver consistency
- Produce a scenario-ready base case before handing off to a valuation, LBO, or M&A workflow
- Rebuild a model from a PDF-only CIM or sell-side teaser with no source spreadsheet
- Document the driver map before a senior banker or IC review

## Required Input

Provide the following:

1. **Target identifier** — Company name, ticker, or deal codename
2. **Historical financials** — 3 full fiscal years of IS, CF, and BS line items; LTM if available. Either paste, attach filings (10-K, 10-Q, 20-F), or reference an existing spreadsheet
3. **Reporting convention** — Fiscal year end, reporting currency, unit (thousands vs. millions), GAAP vs. IFRS
4. **Segment structure** (optional) — Revenue by segment / geography / product line if the forecast is built bottom-up
5. **Forecast horizon** — Default 5 years; longer for long-cycle industries (energy, infra, biotech)
6. **Revenue driver logic** — Volume × price, segment growth rates, unit economics, contracted backlog roll, or top-down % growth
7. **Cost structure drivers** — COGS as % of revenue, fixed vs. variable split, D&A schedule, SG&A growth or operating leverage, stock-based comp policy
8. **Capital intensity** — Capex as % of revenue or explicit capex plan, maintenance vs. growth split, useful life / depreciation convention
9. **Working capital** — DSO, DIO, DPO, deferred revenue as % of forward revenue, or explicit net working capital % of revenue
10. **Capital structure** — Existing debt stack (tranche, rate, maturity, amortization), revolver availability, target leverage policy, planned financings
11. **Tax convention** — Statutory rate, effective-rate adjustments (R&D credits, foreign mix, GILTI/BEAT), NOL balance and usage rules
12. **Dividend / buyback policy** — Payout ratio, announced buyback programs, preferred dividends
13. **Known one-timers** — Restructuring, impairments, litigation settlements, pending accounting-standard changes to segregate from underlying run-rate

## Instructions

You are a finance professional's AI assistant specializing in 3-statement financial modeling, historical normalization, and forecast construction. Your job is to produce a fully-tied, auditable model where every number traces to a clearly labeled driver or a stated historical source, and the balance sheet checks to zero every period.

**Before you start:**
- Load `config.yml` from the repo root for reporting conventions (base currency, unit convention, fiscal calendar, tax rate defaults)
- Reference `knowledge-base/terminology/` for standard IS/CF/BS line-item definitions (EBITDA, unlevered FCF, working capital, NWC, etc.)
- Reference `knowledge-base/best-practices/financial-cot-prompting.md` for the structured reasoning pattern across IS → CF → BS integration
- Cross-check against `skills/operations/financial-model-documenter.md` — this skill BUILDS the model; Documenter reviews an existing one
- Cross-check against `skills/operations/dcf-valuation-builder.md` — DCF consumes the UFCF output of this skill

**Process:**

1. **Collect and normalize historicals.** Restate 3 years (+ LTM if available) to a clean template. Strip one-timers into a separate adjustments schedule (restructuring, impairments, gain/loss on sale, legal). Reconcile any mid-year segment re-reporting. Produce a "historical summary" block that the forecaster will extend
2. **Design the driver map.** Explicitly state, for every forecasted line, whether it is driven by: (a) a growth rate, (b) a margin / % of revenue, (c) a unit-economics build, (d) a schedule (debt, PP&E, leases, SBC), or (e) a hard-coded forecast. Flag any line that cannot be cleanly placed so the user can confirm treatment
3. **Build the Income Statement forecast.** Revenue → Gross Profit → EBITDA → EBIT → Pretax Income → Net Income. Show SBC separately; do not bury it in "Other." Confirm tax expense = (Pretax Income − permanent differences) × effective tax rate, with NOL usage applied if a balance is provided
4. **Build the supporting schedules** in the order that resolves circularity cleanly:
   - **PP&E roll-forward**: Beginning PP&E + Capex − D&A − Disposals = Ending PP&E. Depreciation tied to the useful-life convention stated in inputs
   - **Working capital schedule**: AR (DSO × revenue / 365), Inventory (DIO × COGS / 365), AP (DPO × COGS / 365), Deferred revenue as a % of NTM revenue. Compute change in NWC for the cash flow
   - **Debt and interest schedule**: Tranche-by-tranche beginning balance → scheduled amortization → optional prepayment → ending balance; interest on average balance, cash vs. PIK segregated
   - **Equity / share count roll-forward**: Beginning shares + issuance − buybacks = ending shares; weighted-average for EPS
   - **Deferred tax and NOL schedule** if material
5. **Build the Cash Flow Statement** (indirect method). Start with Net Income → add back non-cash items (D&A, SBC, deferred tax, impairments) → subtract ΔNWC → CFO. Capex + acquisitions + divestitures = CFI. Debt drawdowns − repayments − interest paid − dividends − buybacks = CFF. Sum to Δcash; reconcile to the balance-sheet cash line
6. **Build the Balance Sheet.** Roll each line forward using the schedules and the CFS drivers. Assets = Cash + AR + Inventory + PP&E + Other. Liabilities = AP + Debt + Deferred Revenue + Other. Equity = Beginning Equity + Net Income + SBC − Buybacks − Dividends. **Verify A = L + E for every historical and forecast period and print the difference in a check row; if non-zero, identify the missing link before presenting the model**
7. **Layer in a plug and circularity handling.** Decide the plug mechanism (typically: excess cash accumulates or revolver draws to meet a minimum cash balance). Document the circularity (interest on average debt depends on ending debt which depends on interest); resolve through iteration or a short-term debt average balance proxy, and state the convention used
8. **Produce a driver dashboard.** One page with: revenue CAGR, EBITDA margin path, capex intensity, NWC as % of revenue, leverage trajectory, tax rate, FCF conversion. This is what the senior reviewer scans first
9. **Write a review commentary.** Three sentences on model thesis, three key forecast assumptions the user should pressure-test, and an explicit "what would break this forecast" list
10. **Run a self-audit before delivery.** Confirm: (a) A = L + E every period, (b) Cash on BS = prior cash + Δcash from CFS, (c) Retained earnings roll ties, (d) Debt BS balance = ending balance on debt schedule, (e) Depreciation on CFS = D&A on IS = change implied by PP&E roll. Any failed check blocks delivery until fixed

**Output Structure:**

```
1. Model Summary (thesis, horizon, reporting convention, base-case EBITDA / FCF / leverage trajectory)
2. Driver Map (table: line item → driver type → assumption source)
3. Historical Normalization (3-year restated IS / CF / BS with one-timers stripped)
4. Income Statement Forecast (5-year)
5. Supporting Schedules (PP&E, Working Capital, Debt & Interest, Equity, Tax/NOL)
6. Cash Flow Statement Forecast (5-year, indirect method)
7. Balance Sheet Forecast (5-year) + A = L + E check row
8. Driver Dashboard (1-page KPI trajectory)
9. Review Commentary (thesis, three assumptions to pressure-test, break-the-forecast list)
10. Self-Audit Log (each check pass/fail with the numerical delta)
```

**Output requirements:**
- Every forecast line must be traceable to a driver row; no orphan numbers
- Balance sheet must tie (A − L − E = 0) to the rounding penny in every historical and every forecast period; surface and explain any violation before delivery
- Cash flow must reconcile to the change in balance-sheet cash to the rounding penny
- Segregate cash vs. PIK interest; never combine stock-based comp into SG&A in the delivered version
- All schedules print the beginning balance, movements, and ending balance — never just ending values
- Forecasts are labeled as "Actual" vs. "Forecast"; the first forecast year label is explicit
- A model that fails the self-audit is NOT delivered — it is returned to the user with a remediation list
- Saved to `outputs/` if the user confirms

## Audience Templates (select per delivery context)

1. **GAAP Issuer / Public Company** — SEC-reporting entity; GAAP basis; segment-revenue disaggregation per ASC 280; non-GAAP EBITDA reconciliation per Reg G; Reg S-K Item 303 MD&A-ready revenue and margin narrative; EPS tie-out (basic and diluted); SOX §302 / §906 management-certification support; coordinates with `regulatory-filing-checker`
2. **IFRS Issuer / Foreign Private Issuer** — IFRS basis (IFRS 15 revenue, IFRS 16 leases, IAS 12 taxes, IAS 7 cash flow); 20-F filing format; segment per IFRS 8; reconciliation of IFRS → non-IFRS adjusted measures; functional-currency and presentation-currency distinction per IAS 21; coordinates with `regulatory-filing-checker`
3. **Private Company / Sponsor-Backed** — Non-reporting entity; EBITDA and unlevered FCF focused; management-adjusted EBITDA walk (add-backs, one-timers, run-rate adjustments); no EPS requirement; LBO-model-ready output structure (EBITDA, capex, ΔNWC, cash taxes as the primary handoff columns); coordinates with `lbo-model-builder` and `dcf-valuation-builder`
4. **Non-Profit / Tax-Exempt Entity** — ASC 958 basis; net-asset classification (without donor restrictions / with donor restrictions); functional-expense allocation (program vs. management vs. fundraising); UBIT revenue flag; 990-schedule compatibility; endowment-draw policy; coordinates with `ips-generator` (endowment / foundation IPS template)
5. **DCF / Valuation Feed** — 3-statement as a UFCF factory for the DCF; explicit UFCF definition (EBIT × (1 − t) + D&A − capex − ΔNWC); mid-year vs. end-of-year convention stated; terminal-year normalization (capex = maintenance, NWC at steady-state, D&A ≈ capex); SBC treatment stated (deducted vs. excluded with stated impact); coordinates with `dcf-valuation-builder`
6. **LBO Feed** — 3-statement as the operating-engine input to the LBO model; EBITDA, capex, ΔNWC, and cash taxes as primary output columns; management-adjusted EBITDA walk including PPA step-up amortization and one-timers; revolver / cash-balance plug mechanism explicitly labeled; coordinates with `lbo-model-builder`
7. **Fund Portfolio-Company** — NAV-impact format; EBITDA and leverage trajectory for sponsor reporting; quarterly actuals vs. budget bridge; IRR-bridge compatibility (equity value at cost, at current, at exit); management-fee and carry waterfall not in scope here but EBITDA and net income feed `fund-administration-nav-reviewer`
8. **Reg S-K Item 303 MD&A Feed** — SEC issuer quarterly refresh; revenue and margin drivers mapped to MD&A "known trends" and "material changes" language; comparison periods (current quarter, YTD, same periods prior year); safe-harbor framing for forward-looking statements; coordinates with `regulatory-filing-checker` and `month-end-close-flux-commentator`
9. **Restructuring / Plan of Reorganization** — Debtor-side pro-forma; fresh-start accounting under ASC 852 (reorganization value, goodwill elimination, retained-earnings reset to zero on emergence); DIP-to-exit-financing conversion; claims and recovery sensitivity by class; emergence-balance-sheet tie-out; coordinates with `cash-flow-forecaster-13-week` (DIP budget) and `loan-covenant-compliance-monitor` (emergence covenants)

## Regulatory & Compliance Layer

- **ASC 230 / IAS 7 (Cash Flow Statement)** — Indirect-method CFS construction discipline: beginning with net income, adding back non-cash items (D&A, SBC, deferred tax, impairment), subtracting ΔNWC, and producing CFO / CFI / CFF that reconciles to the change in balance-sheet cash. Direct-to-indirect reconciliation available on request. Classification of interest paid, dividends received, and taxes paid follows the entity's stated policy (ASC 230 vs. IAS 7 differences in classification of interest / dividends)
- **ASC 280 / IFRS 8 (Segment Reporting)** — Revenue disaggregation per reportable segment consistent with what management presents to the chief operating decision-maker (CODM); if the model is for a multi-segment issuer, segment revenue must tie to the total revenue line and footnote disclosures
- **ASC 606 / IFRS 15 (Revenue Recognition)** — Deferred-revenue roll-forward (contract liabilities) treated as a working-capital item; contract-asset roll-forward (unbilled receivables) treated as AR; revenue recognized only when / as performance obligations are satisfied; no "pull-forward" revenue recognition in the forecast
- **ASC 740 / IAS 12 (Income Taxes)** — Deferred-tax asset / liability schedule (temporary differences, NOL carryforwards, valuation allowance); effective-rate vs. statutory-rate reconciliation; IRC §382 NOL limitation applied where a change-of-ownership event has occurred or is modeled; GILTI / BEAT / FDII impact for multinational entities
- **ASC 842 / IFRS 16 (Leases)** — Operating leases: ROU asset and lease-liability roll-forward; operating-lease cost on IS (not rent expense post-ASC 842); finance leases: interest expense on IS, principal repayment on CFS financing activities; lease-liability beginning balance + additions − payments = ending balance; coordinates with `general-ledger-reconciler` (ROU asset / lease-liability sub-ledger tie-out)
- **ASC 805 / IFRS 3 (Business Combinations — Purchase Accounting)** — If the model is a post-acquisition pro-forma, PPA step-ups to inventory, PP&E, and intangibles flow through the IS via higher COGS / D&A; deferred-revenue haircut reduces near-term revenue; contingent-consideration liability on the balance sheet; goodwill impairment tested annually (ASC 350); coordinates with `accretion-dilution-analyzer`
- **ASC 350 / 360 / IAS 36 (Impairment)** — Goodwill and indefinite-lived intangibles tested annually (or on triggering event) using reporting-unit analysis; long-lived assets tested for recoverability (undiscounted CFs > carrying value); impairment charge flows through IS; coordinates with `dcf-valuation-builder` (ASC 350 impairment template)
- **Reg S-K Item 303 (MD&A — Forward-Looking Statements)** — For public-company models, forward-looking revenue, margin, and liquidity commentary must be framed with safe-harbor language (Securities Act §27A, Exchange Act §21E); no guarantees; material assumptions disclosed; coordinates with `regulatory-filing-checker`
- **Reg G (Non-GAAP Reconciliation)** — Any EBITDA, adjusted EBITDA, adjusted EPS, or free-cash-flow presentation must be reconciled to the most directly comparable GAAP measure; non-GAAP label must not be more prominent than the GAAP measure in client-facing or issuer output
- **SOX §302 / §906 (Management Certifications)** — For SEC issuers, the 3-statement model is an input to the MD&A and financial statements that are certified by the CEO and CFO under SOX; material errors in the model propagate to certification exposure; the balance-sheet tie-out and self-audit log are specifically designed to prevent certification risk
- **MNPI / Wall-Cross / Restricted-List / 10b5-1** — Forward-looking models for public-company subjects are typically MNPI; restrict distribution per the firm's wall-cross and restricted-list policy; 10b5-1 plan implications for any insider
- **Books-and-Records Retention** — Model files are books-and-records subject to Advisers Act Rule 204-2 / FINRA 17a-4 / SOX §802 retention requirements; version with date-stamp; retain assumption inputs and sensitivity runs per firm policy

## Personalization Hooks (consume from `config.yml`)

- `firm.reporting_convention` (`GAAP` / `IFRS` / `private-company` / `nonprofit-ASC-958`) — determines which accounting-standard rules apply throughout the build
- `firm.base_currency` and `firm.unit_convention` (`thousands` / `millions`) — auto-applied to all output tables and labels
- `firm.fiscal_year_end` (e.g., `December` / `June` / `September`) — drives period labeling and partial-year stub handling
- `model.forecast_horizon_default` (`5` years; `3` / `7` / `10` year alternates for specific industries or mandates)
- `model.revenue_driver_convention` (`top-down-pct` / `volume-x-price` / `unit-economics` / `contracted-backlog-roll`) — default logic for the driver map
- `model.depreciation_convention` (`straight-line` default; `double-declining-balance` / `MACRS` alternates; `useful_life_policy` table)
- `model.working_capital_convention` (`dso-dio-dpo-driver` default / `nwc-as-pct-of-revenue` simplified)
- `model.interest_convention` (`average-of-beginning-and-ending-balance` default; `beginning-balance` alternate for simplicity)
- `model.plug_mechanism` (`excess-cash-accumulates` / `revolver-draws-to-minimum-cash`) — the mechanism that resolves the BS circular reference
- `model.tax_rate_default` — blended cash-tax rate; deferred-tax schedule complexity (`full` / `simplified-effective-rate`)
- `model.nol_policy` — Section 382 annual-limitation amount if an ownership-change event has occurred; NOL-usage cap per period
- `model.sbc_treatment_convention` (`deducted-in-fcf` / `excluded-with-add-back`) — must be consistent with the DCF and LBO handoff expectations
- `firm.model_build_convention` (mid-year convention yes/no; signing convention; rounding policy; stub-period handling; non-cash-item label convention)
- `firm.restricted_list` / `firm.mnpi_policy` — governs which company names can appear in model output shared externally
- `firm.audit_trail_convention` — every forecast line traceable to a driver row; source-tied for historical lines; version-stamp requirements
- `firm.naming_convention` — model-file name format (company, date, version)

## Handoff Contracts

- **Inbound from**:
  - `skills/operations/general-ledger-reconciler.md` — Reconciled BS ending balances and IS actuals as the authoritative historical input for the rolling model update; coordinates with `general-ledger-reconciler` to ensure no unreconciled account flows into the model as a "settled" balance
  - `skills/operations/month-end-close-flux-commentator.md` — Period-over-period flux narrative confirms the IS actuals and surfaces anomalies (one-timers, reclassifications) that the model must strip or normalize in the historical restate
  - `skills/operations/budget-variance-analyzer.md` — Reforecast-exit assumptions (updated revenue growth, margin path) feed the model's next-period driver rows
  - `skills/operations/earnings-call-summarizer.md` — Public-company actuals and management guidance (revenue guidance range, margin commentary) update the revenue and margin drivers for the forecast period
  - `skills/operations/market-research-brief.md` — Sector revenue-growth benchmarks and margin comparables anchor the driver map's long-run assumptions
  - `skills/operations/pe-due-diligence-synthesizer.md` — Diligence findings (revenue quality, cost structure, capex intensity, NWC normalization, key-person risk) update target's operating assumptions
  - `skills/admin/regulatory-filing-checker.md` — Restated GAAP financials from 10-K / 10-Q / 20-F / S-1 used as the historical input; ensures the model's historical block is consistent with the filed financials
- **Outbound to**:
  - `skills/operations/dcf-valuation-builder.md` — UFCF (EBIT × (1 − t) + D&A − capex − ΔNWC), D&A, capex, ΔNWC, SBC treatment, and tax rate consumed as DCF primary inputs
  - `skills/operations/lbo-model-builder.md` — EBITDA, capex, ΔNWC, cash taxes, and the revenue / margin driver map consumed as LBO operating engine inputs
  - `skills/operations/accretion-dilution-analyzer.md` — Net income, share count, EPS, and the synergy-adjustable driver rows consumed for EPS accretion / dilution analysis
  - `skills/operations/cash-flow-forecaster-13-week.md` — Annual / monthly IS and BS projections as the source for the direct-method 13-week translation (AR / AP balances, monthly EBITDA run-rate)
  - `skills/operations/loan-covenant-compliance-monitor.md` — Leverage ratio, FCCR, DSCR, and interest-coverage trajectory from the debt schedule consumed for covenant-compliance testing
  - `skills/operations/stress-test-scenario-modeler.md` — Base-case IS / CF / BS serves as the stress-test starting point for downside / severe-downside scenario construction
  - `skills/operations/financial-model-documenter.md` — Model documentation pack: driver map, assumption sources, balance-sheet tie-out log, self-audit results; the documenter reviews and narrativizes what this skill builds
  - `skills/operations/investment-memo-drafter.md` — Revenue, EBITDA, FCF, and leverage trajectory narrative consumed as the IC memo's financial-summary section
  - `skills/admin/regulatory-filing-checker.md` — Restated financial schedules and forecasted financial statements for 10-K / 10-Q / 20-F / S-1 filing review; any material accounting judgment in the model flags for regulatory review
  - `outputs/` — Versioned save with company, date-stamp, version number, and reporting-convention label per `firm.naming_convention`

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
