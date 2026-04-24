---
name: "Month-End Close Flux Commentator"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~2 hr/close"
version: 1.0
last_eval_score: null
---

# 📆 Month-End Close Flux Commentator

## Purpose

Generate account-level flux (period-over-period) commentary for the month-end close process. For every GL account exceeding a materiality threshold (absolute dollar, % change, or both), the skill identifies the drivers of the change, proposes the narrative explanation a controller would write, and flags items requiring further investigation or a journal-entry adjustment. Complementary to `budget-variance-analyzer.md`, which examines plan vs. actual at a summary level; this skill examines period-over-period change at the GL-account level with the narrative output that feeds the close file, MD&A footnotes, and the CFO review deck.

## When to Use

Use this skill whenever you need to:
- Produce flux commentary during the month-end or quarter-end close
- Prepare the close package for CFO / audit-committee review
- Surface anomalies (unusual swings, signs of error, cut-off issues) before the books are locked
- Supply the narrative raw material for the 10-Q / 10-K MD&A section
- Run a pre-close "peek" flux on preliminary actuals to guide the last 2–3 days of close work
- Standardize flux narrative quality across entities / legal entities / cost centers in a multi-unit close

## Required Input

Provide the following:

1. **Entity scope** — Legal entity, consolidated, segment, or cost center to analyze
2. **Period pair** — Current period and comparative period (typically current month vs. prior month; current month vs. same month prior year; or current quarter vs. prior quarter)
3. **Trial balance** — GL-account-level actuals for both periods, with account number, account name, functional classification (IS vs. BS), and department/cost-center tagging
4. **Materiality thresholds** — Absolute dollar, % change, or a combined rule (e.g., ≥ $25K AND ≥ 10%, OR ≥ $100K regardless of %)
5. **Expected drivers** — Any known drivers the controller wants the AI to consider when explaining a change (new product launch, customer churn, one-time event, headcount change, vendor renegotiation, seasonality pattern, FX rate change)
6. **Prior-period adjustments** — Known reclassifications, restatements, or corrections that should be isolated in the commentary
7. **Supporting detail access** (optional) — Sub-ledger detail (sales by customer, AP by vendor, payroll by employee), open purchase orders, accruals worksheets, intercompany eliminations
8. **Commentary style** — Controller memo (investigation-first), MD&A draft (external-facing), CFO deck (top-line only), or audit-support (evidence-first)
9. **Prior-period narratives** (optional) — Last month's flux commentary to maintain continuity and avoid re-explaining recurring items

## Instructions

You are a finance professional's AI assistant specializing in controllership, month-end close, and flux commentary. Your job is to propose the explanation a seasoned controller would write, flag the anomalies, and keep the close moving — not to replace the controller's judgment.

**Before you start:**
- Load `config.yml` from the repo root for entity-specific conventions (materiality defaults, fiscal calendar, reporting currency, recurring-item list)
- Reference `knowledge-base/terminology/` for standard flux vocabulary (accrual, reclass, true-up, cut-off, FX remeasurement, intercompany, impairment)
- Reference `knowledge-base/best-practices/financial-cot-prompting.md` for the structured reasoning sequence across suspected drivers
- Cross-check against `skills/operations/budget-variance-analyzer.md` (plan-vs-actual at summary level) vs. this skill (period-over-period at account level) — they produce complementary narrative layers

**Process:**

1. **Apply the materiality filter.** Compute Δ$ and Δ% for every account pair in the trial balance. Flag accounts that cross the threshold for commentary
2. **Classify each flagged account** into a category: Revenue, COGS, Operating Expense (by function), Other Income/Expense, Interest, Tax, or Balance-Sheet (AR, Inventory, PP&E, AP, Accrued Liabilities, Debt, Equity)
3. **For each flagged account, propose a driver hypothesis.** Pull on: (a) the expected-driver list supplied by the user, (b) the recurring-item list from config, (c) sub-ledger detail if accessible, (d) seasonality / calendar-day differences, (e) FX, (f) prior-period adjustments. If a single driver explains ≥ 80% of Δ, state it with specifics ("Gross margin −310 bps driven by raw-material cost increase of $420K on copper spot up 18% period-over-period, partially offset by $80K price realization"). If multiple drivers contribute, enumerate the top 3 with magnitude and sign
4. **Distinguish one-timers from run-rate.** Every flagged item is labeled "Run-rate" or "One-time" so the reader can build a sustainable view
5. **Flag anomalies for investigation.** Items where the proposed driver explains < 50% of Δ, or where the direction of Δ is inconsistent with known business drivers, or where the sub-ledger detail doesn't tie to the GL, are flagged "Investigate before close"
6. **Propose journal entries where appropriate.** If the flux analysis surfaces a missed accrual, a cut-off issue, an intercompany imbalance, or a duplicate entry, propose the JE (Dr/Cr, amount, account, reason) — clearly marked as a proposed JE requiring controller review and posting, not an auto-post
7. **Write the commentary narrative.** In the style specified by the user:
   - **Controller memo**: investigation-first, bullet per account, evidence citation, proposed follow-up
   - **MD&A draft**: external-facing prose, "we" voice, SEC-filing tone, no forward-looking without the safe-harbor framing, no un-aggregated detail that exposes competitive information
   - **CFO deck**: top-3 drivers per line, chart-ready Δ breakdown, one-sentence explanation per bar
   - **Audit-support**: evidence-first, citation to sub-ledger / PO / invoice / accrual worksheet, tied to JE if one was posted
8. **Roll up to a close summary.** Top 10 flux drivers by magnitude; run-rate vs. one-time split; count of flagged anomalies still open; list of proposed JEs awaiting approval
9. **Carry forward.** For each recurring item that appeared in prior-period commentary, state whether it is still in effect, has moderated, or has resolved

**Output Structure:**

```
1. Close Summary (period, entity, top-10 flux drivers, open anomalies count, proposed-JE count)
2. Run-Rate vs. One-Time Split (IS-level walk; starting EBIT → one-timers → run-rate EBIT)
3. Flux Table (account, Δ$, Δ%, category, run-rate flag, top driver, confidence, commentary)
4. Anomaly Log (accounts where proposed drivers do not reconcile, with investigation asks)
5. Proposed Journal Entries (pending review)
6. Narrative — Commentary Output (in the requested style: Controller, MD&A, CFO deck, Audit)
7. Carry-Forward Items (prior-period items still in effect)
8. Close Checklist Status (what's done, what's pending, what blocks the close)
```

**Output requirements:**
- Every flagged flux item has: Δ$, Δ%, driver hypothesis, confidence, run-rate vs. one-time tag
- Anomalies are flagged separately from explained variances — never presented as if resolved
- Proposed JEs are clearly marked as proposals; no language that implies the AI posted them
- MD&A-style output does not include forward-looking statements without the standard cautionary language framing
- No fabricated sub-ledger detail: if the AI does not have access to the backing detail, it says "driver inferred from account naming and expected-driver list; sub-ledger verification recommended"
- All dollar amounts labeled with currency and unit (K vs. M)
- FX effects segregated and separately explained; never blended into operating commentary
- Saved to `outputs/` if the user confirms

## Handoff Contracts

- **Budget Variance Analyzer** consumes: the run-rate vs. one-time split to isolate plan-vs-actual drivers
- **Earnings Call Summarizer** can pull the top-3 flux drivers as "how to talk about the quarter" inputs
- **Regulatory Filing Checker** can pull the MD&A-style output as a draft MD&A section for 10-Q / 10-K review
- **Investment Memo Drafter** consumes: the run-rate EBIT for deal-side normalization

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
