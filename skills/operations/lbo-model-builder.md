---
name: "LBO Model Builder"
category: operations
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~2.5 hr/model"
version: 1.0
last_eval_score: null
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

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
