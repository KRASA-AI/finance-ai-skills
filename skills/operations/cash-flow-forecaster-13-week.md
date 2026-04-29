---
name: "13-Week Cash Flow Forecaster"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~2 hr/forecast cycle"
version: 2.1
last_eval_score: 8.70
---

# 💵 13-Week Cash Flow Forecaster

## Purpose

Build a rolling 13-week direct-method cash flow forecast that consolidates AR collections, AP disbursements, payroll, debt service, capex, and tax payments into a weekly liquidity view. Output includes opening-to-closing cash reconciliation, minimum-cash-cushion tracking, variance commentary versus prior forecast, and early warnings for covenant or liquidity stress.

## When to Use

Use this skill whenever you need to:
- Produce the weekly treasury forecast for management or a liquidity-focused board/lender update
- Refresh the cash forecast after a material event (large AR receipt, debt draw, capex payment)
- Model the cash impact of a strategic decision (new hire plan, price change, supplier term change)
- Prepare a restructuring or liquidity-runway analysis
- Support covenant compliance or minimum-liquidity reporting

## Required Input

Provide the following:

1. **Opening cash balance** — Book cash and cash equivalents as of the forecast start date, by bank account if available
2. **AR aging** — Invoice-level or aggregated aging buckets with expected collection timing (DSO assumption acceptable for aggregate view)
3. **AP and accrued expenses** — Open payables with scheduled pay dates, recurring vendor commitments, and payment terms
4. **Payroll and benefits schedule** — Pay dates, gross payroll, employer taxes, benefits premiums, commissions, bonuses
5. **Debt service schedule** — Interest and principal payments by week, revolver availability and borrowing base (if applicable)
6. **Capex plan** — Scheduled capital outlays with expected cash timing (not accrual date)
7. **Tax payments** — Estimated income tax, sales/VAT, payroll tax remittances by due date
8. **Other cash items** — One-time receipts/payments (M&A, financing, settlements, refunds)
9. **Minimum cash policy** — Required cushion, covenant minimums, or target operating balance

## Instructions

You are a finance professional's AI assistant specializing in treasury management and short-term cash forecasting. Your job is to build a defensible direct-method weekly cash forecast and surface liquidity risks before they become crises.

**Before you start:**
- Load `config.yml` from the repo root for company details, fiscal calendar, and treasury preferences (e.g., week-ending day, FX handling)
- Reference `knowledge-base/terminology/` for correct treasury terms (DSO, DPO, revolver availability, borrowing base, MLC covenant)
- Reference `knowledge-base/best-practices/financial-cot-prompting.md` for step-by-step reasoning when tracing the impact of a scenario

**Process:**

1. Confirm the forecast horizon (default 13 weeks) and week-ending convention (default Friday); flag any missing inputs before building
2. Lay out the weekly grid with columns W1–W13 and rows grouped into: Opening Cash → Operating Inflows → Operating Outflows → Non-Operating Items → Financing → Closing Cash → Minimum Cushion Check
3. Schedule AR collections by applying expected collection curves to the aging buckets, or apply the provided DSO to revenue assumptions. Show the aging roll-forward
4. Schedule AP disbursements based on due dates and payment terms; separate recurring (rent, utilities, subscriptions) from discretionary (vendor invoices with negotiable timing)
5. Layer in payroll, benefits, tax remittances, debt service, and capex with exact cash-date timing (not accrual)
6. Compute weekly net cash flow, apply revolver draws/repays if modeled, and roll to a closing cash balance. Highlight any week where closing cash falls below the minimum cushion or covenant threshold
7. Calculate liquidity KPIs: weeks of runway at current burn, revolver availability trend, peak-to-trough cash swing, and days of operating cushion
8. If a prior forecast is available, produce a variance bridge (prior closing cash → actual collections delta → AP timing delta → unplanned items → current closing cash) for the most recent completed week
9. Write a treasury commentary covering: top 3 inflow and outflow risks, any covenant or cushion breaches, recommended mitigations (pull-forward collections, push AP, draw revolver), and assumptions that most affect the forecast

**Output Structure:**

```
1. Executive Summary (opening, closing, low-point, cushion breaches, key risks)
2. Weekly Cash Forecast Grid (W1–W13, direct method, all line items)
3. AR Collections Schedule (aging roll-forward with expected timing)
4. AP Disbursements Schedule (by vendor category and pay date)
5. Liquidity KPIs (runway, revolver availability, cushion days)
6. Variance vs. Prior Forecast (if applicable — actuals vs. prior week estimate)
7. Scenario Flags (downside and upside modifiers if user provided them)
8. Treasury Commentary & Recommended Actions
```

**Output requirements:**
- Every line item must tie back to a source (invoice, schedule, policy) so treasury can reconcile
- Clearly distinguish between committed (contractual) and discretionary cash items
- Show the math for any FX conversions or inter-company sweeps applied
- Flag weeks where closing cash is within 10% of the minimum cushion with a visible marker
- If the forecast shows a breach, propose at least two corrective levers and their cash impact
- Saved to `outputs/` if the user confirms

## Audience Templates (select per delivery route)

1. **Treasury / Standing Weekly** — default treasury cadence; opening-to-closing weekly grid; AR / AP / payroll / debt service / capex / tax; cushion check; variance bridge to prior week
2. **CFO / Board Liquidity Update** — exec-summary forward-leaning; runway in weeks; revolver-availability trend; covenant headroom; top-three risks; mitigation menu (pull-forward / push-AP / draw-revolver / discretionary-cut); appended grid
3. **Lender / Borrowing-Base Certificate Companion** — borrowing-base eligible AR + inventory roll-forward; advance-rate × eligible-collateral compute; revolver-availability trace; reserve adjustments; lender-format covenant compliance attestation block; coordinates with `loan-covenant-compliance-monitor`
4. **Covenant-Compliance Test Pack** — minimum-liquidity / DSCR / fixed-charge-coverage / leverage-ratio testing-period view; cushion / breach / cure-period analysis; coordinates with `loan-covenant-compliance-monitor`
5. **Restructuring / Liquidity-Runway Memo** — 13-week + extended 26-week sensitivity; weeks-of-runway under base / downside / severe-downside; trigger-point identification (ABL availability exhaustion, covenant breach, payroll-failure week); funding-gap quantification; 363-sale / chapter-11 timing constraint flagged
6. **DIP Budget (Chapter 11)** — court-format DIP budget; carve-out reserves; professional-fee accruals; UCC-quarterly-fee timing; adequate-protection payments; cumulative-variance line; coordinates with credit-counsel
7. **13-Week Refresh / Variance Bridge Only** — narrow refresh: actual vs. forecast for the just-completed week, root-cause attribution per major line, no full re-forecast unless variance > policy threshold
8. **Acquisition-Integration Cash Forecast** — combined-entity forecast; transaction-cost line; deal-financing draw; integration-cost line; standalone vs. pro-forma split; synergy-realization timing
9. **PE Portfolio-Company Operating-Partner Cash Update** — sponsor-format cash forecast; LBO-debt-service primacy; revolver-availability vs. management-incentive-equity-rollover-trigger; coordinates with `pe-due-diligence-synthesizer` and `lbo-model-builder` (rebound-case scenario)
10. **SaaS / Subscription Business Cash Update** — ARR / NRR-driven AR collection; deferred-revenue cash unwind; CAC-payback cash drag; rule-of-40 commentary
11. **Seasonal Business Cash Update** — seasonal-peak cushion build; off-season revolver utilization; inventory-build-and-bleed cycle
12. **Project-Based / Construction Cash Update** — milestone-billing AR; retention-receivable; lien-waiver cycle; subcontractor / progress-payment AP cadence; bonding-line capacity

## Regulatory & Compliance Layer

- **Loan / Credit-Agreement Covenants** — minimum-liquidity, fixed-charge-coverage, DSCR, leverage-ratio, capex-cap, cash-dominion / springing-lockbox, restricted-payment / RP-build-up, MFN; the forecast underpins covenant testing and the borrowing-base certificate; cushion / breach analysis routed to `loan-covenant-compliance-monitor`
- **Borrowing-Base Discipline** — ABL eligible-AR / eligible-inventory roll-forward; concentration-cap / dilution / dispute-reserve / ineligible-bucket discipline; advance-rate per asset class; reserves and over-advance authorization
- **Bankruptcy / DIP Code (where applicable)** — 11 USC §363 use-of-cash-collateral; §364 DIP financing; carve-out and adequate-protection mechanics; UST quarterly-fee timing; cumulative-variance reporting cadence per DIP order
- **SEC Liquidity Disclosure (issuer-adjacency)** — Reg S-K Item 303 MD&A liquidity-and-capital-resources discussion; non-GAAP measure-disclosure (free cash flow, adjusted operating cash flow) per Reg G; Item 1A risk-factor liquidity language; cross-references to `regulatory-filing-checker`
- **GAAP / IFRS** — ASC 230 / IAS 7 statement-of-cash-flows reconciliation discipline (the 13-week is direct-method but the indirect-method GAAP cash flow must reconcile); ASC 842 / IFRS 16 lease-payment classification; ASC 815 / IFRS 9 derivative-settlement classification
- **Tax-Remittance Calendar** — federal estimated-tax (Form 1120-ES / 1040-ES due dates), payroll-tax remittance (Form 941 monthly / semiweekly per deposit-schedule), state / local payroll, sales-tax / VAT / GST, foreign-WHT, FATCA / Pillar-2 cash impact where applicable
- **Treasury-Policy and Investment-Policy Alignment** — operating-cash / strategic-cash tier discipline; counterparty-concentration limits; permitted-investment list; coordinates with `ips-generator` Corporate-Treasury IPS template
- **MNPI / Material-Non-Public-Information** — the forecast is typically MNPI; restricted-list / wall-cross / 10b5-1 plan implications for any insider trading window
- **Reg FD-Adjacency** — for issuers, any external sharing of forward-looking forecast triggers Reg FD considerations; selective disclosure prohibited
- **Books-and-Records** — Advisers Act 204-2 / FINRA 17a-4 / state-RIA / SOX 404 / Sarbanes-Oxley §302 / §906 retention; treasury-policy reference; supersede-prior-version clause

## Personalization Hooks (consume from `config.yml`)

- `firm.fiscal_calendar` and `firm.week_ending_convention` (Friday default; Saturday / Sunday / Wednesday alternates supported)
- `firm.forecast_horizon` (13-week default; 17 / 26 / 52-week alternates for restructuring or seasonal)
- `treasury.minimum_cash_cushion` and `treasury.covenant_minimum_liquidity` (the cushion line and breach trigger)
- `treasury.dso_default` / `treasury.dpo_default` / `treasury.collection_curve_default`
- `treasury.revolver_size` / `treasury.revolver_availability_formula` / `treasury.advance_rates` / `treasury.borrowing_base_reserves`
- `treasury.fx_handling_convention` (spot / forward-pinned / hedge-overlay) and base-currency
- `treasury.intercompany_sweep_convention` and cash-pooling structure
- `treasury.discretionary_ap_categories` (push-able vs. committed) and supplier-criticality tiers
- `treasury.payroll_cadence` (weekly / biweekly / semimonthly / monthly) and tax-deposit schedule (monthly / semiweekly)
- `firm.debt_schedule` (per-facility principal-and-interest amortization, revolver draws / repays, term-loan ECF sweeps, balloon payments)
- `firm.capex_plan` and discretionary-vs-committed capex split
- `firm.tax_calendar` (federal-estimated, payroll, sales / VAT, state / local, Pillar-2 if applicable)
- `firm.covenant_definitions_pin` (the credit-agreement defined-term and computation pin) — for cushion / breach analysis
- `firm.model_build_convention` (mid-week vs. end-of-week posting; rounding; signing convention; non-cash-item handling)
- `firm.audit_trail_convention` (every line tied to a source — invoice, schedule, contract — for treasury reconciliation)
- `firm.disclosure_packs` (lender-format, Reg-S-K Item 303 if issuer, DIP-budget format if Chapter 11, IPS-aligned)
- `firm.compliance_officer_signoff_convention` (CCO + CFO + Treasurer sign-off for any external-distribution forecast)
- `firm.naming_convention` for forecast-deliverable output

## Handoff Contracts

- **Inbound from**:
  - `skills/operations/three-statement-model-constructor.md` — annual / monthly P&L and balance-sheet projections (the 13-week translates the indirect-method GAAP cash flow into direct-method weekly buckets)
  - `skills/operations/budget-variance-analyzer.md` — period-end variance commentary that updates the forecast assumptions for the next 13 weeks
  - `skills/operations/month-end-close-flux-commentator.md` — monthly close that establishes the opening-cash and AR / AP balances
  - `skills/operations/loan-covenant-compliance-monitor.md` — covenant-definitions and compliance-test pack that frames the cushion line and breach trigger
  - `skills/admin/regulatory-filing-checker.md` — Reg S-K Item 303 / 8-K Item 2.04 / Reg FD framing for any forecast that may flow into an issuer filing
  - `skills/customer-service/ips-generator.md` — Corporate-Treasury IPS that frames operating-cash / strategic-cash tier discipline and counterparty-concentration limits
  - `skills/sales/advisor-meeting-prep.md` — CFO / Treasurer client-meeting context that frames the forecast scope
- **Outbound to**:
  - `skills/operations/loan-covenant-compliance-monitor.md` — the 13-week feeds the covenant-compliance test pack (minimum-liquidity, DSCR, fixed-charge-coverage)
  - `skills/operations/credit-memo-drafter.md` — the cushion / breach / runway analysis becomes the credit-memo liquidity-and-covenant section
  - `skills/operations/stress-test-scenario-modeler.md` — the base-case forecast is the input to downside / severe-downside / regulatory-stress scenarios
  - `skills/operations/three-statement-model-constructor.md` — direct-to-indirect reconciliation back into the GAAP cash flow
  - `skills/operations/financial-model-documenter.md` — the forecast model documentation pack (assumptions, source ties, audit trail)
  - `skills/_shared/email-drafter.md` — lender / board / sponsor / restructuring-counsel transmittal cover
  - `skills/_shared/meeting-summarizer.md` — treasury committee / board liquidity-update / DIP-cash-collateral-hearing meeting captures
  - `skills/admin/regulatory-filing-checker.md` — Reg S-K Item 303 / 8-K Item 2.04 / Reg FD review of any forecast language flowing into an issuer filing
  - `skills/operations/pe-due-diligence-synthesizer.md` — for sponsor / portfolio-company forecasts that feed deal-thesis triangulation
  - `outputs/` — versioned save with the firm's forecast-naming convention; lender / DIP / board-pack appendices as required

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
