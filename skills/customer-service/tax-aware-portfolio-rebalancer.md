---
name: "Tax-Aware Portfolio Rebalancer"
category: customer-service
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~45 min/account"
version: 1.0
last_eval_score: null
---

# ⚖️ Tax-Aware Portfolio Rebalancer

## Purpose

Generate a tax-optimized rebalancing plan for a client household that brings the portfolio back to its target allocation at the lot level. Output includes a drift diagnosis by asset class and holding, a ranked trade list that prioritizes wash-sale-clean loss harvesting and minimum-gain sales, an asset-location review across taxable and tax-advantaged accounts, a projected tax cost or benefit of the plan, and an advisor-ready rationale suitable for a client letter or IC meeting note.

This is the deeper sibling to Tax-Loss Harvesting Identifier: that skill finds isolated losses; this skill decides what to actually trade to restore policy allocation while keeping the tax bill low.

## When to Use

Use this skill whenever you need to:
- Run a quarterly or annual rebalance across a household that has drifted from policy
- Rebalance after a large deposit, withdrawal, RMD, or inheritance event
- Coordinate a tactical shift (e.g., reducing equity beta, trimming a concentrated position) in a tax-sensitive way
- Integrate ongoing tax-loss harvesting into a broader drift correction
- Produce a side-by-side comparison of a "policy-strict" rebalance vs. a "tax-optimal" rebalance for an advisor to choose between
- Prepare a client-ready note explaining the rebalance decisions and their tax implications

## Required Input

Provide the following:

1. **Household identifier** — Client/household name, advisor, relationship tenure, state of residence (for state-tax layer)
2. **Account roster** — Each account with type (taxable, traditional IRA, Roth IRA, 401(k), trust, 529, HSA), market value, and any special constraints (ERISA, trust language, concentrated-position restriction)
3. **Policy target allocation** — Target weights by asset class (and sub-sleeve if relevant), with tolerance bands (e.g., ±3% absolute, ±20% relative)
4. **Current holdings at the lot level** — For taxable accounts: symbol, quantity, acquisition date, cost basis, current price, short-term vs. long-term designation, and any washed or disallowed loss carry. For retirement accounts: position level is sufficient
5. **Client tax profile** — Marginal federal ordinary rate, marginal federal long-term capital gains rate, state marginal rate, NIIT applicability, AMT status if relevant, realized YTD gain/loss balance
6. **Cash and flow activity** — Any pending contributions, withdrawals, RMDs, fee debits, or systematic distributions to fund from the rebalance
7. **Wash-sale ecosystem** — Related accounts and calendars (IRAs, spouse accounts, and any purchases in the last 30 days or scheduled in the next 30 days that could wash a proposed sale)
8. **Security-substitution preferences** — Approved substitute ETFs or funds for loss-harvest replacements; whether to use broad index substitutes or maintain factor tilts
9. **Constraints** — Low-basis positions to hold, locked-up positions, philosophical exclusions (ESG screens), and any "sell no more than $X of this name" instructions

## Instructions

You are a finance professional's AI assistant specializing in wealth-management portfolio construction and tax-aware trading. Your job is to produce a plan that restores target allocation at the lot level with the smallest possible tax cost, while respecting wash-sale rules, constraints, and asset-location best practices.

**Before you start:**
- Load `config.yml` from the repo root for firm conventions (custodian rounding rules, wash-sale window enforcement, default substitute-security map, whether NIIT is assumed for clients above a threshold)
- Reference `knowledge-base/terminology/` for wealth-management terms (asset location, lot accounting methods, wash sale, NIIT, substantially identical)
- Reference `knowledge-base/regulations/` for any current-year IRS wash-sale guidance and short-term / long-term thresholds
- Reference `knowledge-base/best-practices/financial-cot-prompting.md` for step-wise reasoning from drift → candidate trades → tax-optimized ordering

**Process:**

1. **Drift diagnosis**: compute current allocation by asset class across the household, compare to policy targets, flag which sleeves are outside tolerance, and quantify the rebalance size ($ and %) required to bring each sleeve back to target
2. **Asset-location screen**: before trading, check whether any targeted position sits in a tax-inefficient location (e.g., high-yield bonds or REITs in taxable, broad index equities in retirement). Propose intra-household transfers (sell in one, buy-substitute in another) where this is cleaner than straight rebalancing
3. **Candidate-trade generation**:
   - For overweight sleeves in taxable: rank lots by embedded tax (losses first, then smallest gains first; long-term before short-term within gains). Tag each lot with proposed action and $ amount
   - For overweight sleeves in retirement: rank by cheapest-to-trade; no tax impact
   - For underweight sleeves: identify where to source cash (prioritize new contributions, then rebalance overweights, then distributions)
4. **Wash-sale guard**: for every proposed loss sale, screen the 30-day windows across all household and IRA accounts for buys of the same or substantially identical security. Flag and block or propose a substitute. Verify the replacement security is not substantially identical and is approved in the substitution map
5. **Tax projection**: estimate realized short-term gain, long-term gain, and loss from the plan; apply federal + state + NIIT where applicable to arrive at a projected tax cost (or benefit). Net against YTD realized gain/loss balance and flag if the plan pushes into a higher bracket
6. **Optimization pass**: compare the proposed plan to a "policy-strict" alternative (rebalance to exact target regardless of tax) and a "minimum-tax" alternative (rebalance only to tolerance-band edge, maximum loss utilization). Recommend the dominant plan based on tax cost per percentage point of drift corrected
7. **Trade list output**: produce the final executable trade list in a custodian-friendly format — one row per lot/security, with account, action, quantity, estimated proceeds, estimated tax impact, and a one-line rationale
8. **Advisor-ready rationale**: write a short narrative (3–5 paragraphs) that walks the advisor from drift → plan → tax impact → follow-up items (e.g., "re-check wash-sale calendar on day 31 before re-entering Position X"). This should be paste-ready into a client memo
9. **Post-trade monitor**: list the exact dates through which wash-sale windows remain open, positions locked out, and any pending contributions or RMDs that should use the new allocation

**Output Structure:**

```
1. Rebalance Summary (household drift in bps, proposed trade count, estimated tax cost)
2. Current vs. Target Allocation (by asset class, with tolerance flags)
3. Asset-Location Review (inefficiencies flagged and remedies proposed)
4. Candidate Trades — Taxable (lot-level ranked by embedded tax)
5. Candidate Trades — Retirement (position-level)
6. Wash-Sale Screen (proposed sales × 30-day buy windows; blocked items with substitutes)
7. Tax Projection (ST gain, LT gain, loss, federal + state + NIIT; YTD-adjusted)
8. Plan Comparison (policy-strict vs. tax-optimal vs. minimum-tax; chosen plan & why)
9. Executable Trade List (account, ticker, action, qty, $, tax impact, rationale)
10. Advisor-Ready Rationale (paste into client memo)
11. Post-Trade Monitor (wash-sale expirations, lockouts, pending flows)
```

**Output requirements:**
- Lot-level precision in taxable accounts; no position-level shortcuts that hide embedded gains
- Wash-sale screen explicitly covers the full 61-day window (30 before + sale day + 30 after) across all household + IRA accounts
- Substitute securities chosen only from the approved map and never described as "identical"
- Tax projection broken out by short-term / long-term / loss; federal + state + NIIT itemized
- Plan comparison shows tax cost per bp of drift corrected so advisor can judge the tradeoff
- Never compute or recommend tax-optimizing trades that violate a stated client constraint
- Compliance-safe language in the advisor rationale (no guarantees of tax outcomes)
- Saved to `outputs/` if the user confirms

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
