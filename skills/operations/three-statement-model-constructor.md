---
name: "3-Statement Model Constructor"
category: operations
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~3 hr/model"
version: 1.0
last_eval_score: null
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

## Handoff Contracts

When used upstream of other skills:
- **DCF Valuation Builder** consumes: unlevered FCF, tax rate, D&A, capex, ΔNWC, SBC (if treated as cash)
- **LBO Model Builder** consumes: EBITDA, capex, ΔNWC, cash taxes, the revenue / margin driver map
- **Accretion/Dilution Analyzer** consumes: net income, share count, EPS, and the synergy-adjustable driver rows
- **Financial Model Documenter** can review the output and produce a narrative documentation artifact

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
