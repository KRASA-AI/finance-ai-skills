---
name: "Budget Variance Analyzer"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~45 min/analysis"
version: 2.0
last_eval_score: 8.20
---

# 📊 Budget Variance Analyzer

## Purpose

Produce audit-ready budget-to-actual variance commentary for a full FP&A cycle — monthly close, quarterly review, rolling forecast, or board-package prep. The skill decomposes each material variance into price, volume, mix, timing, and rate components, labels each variance as run-rate vs. one-time, connects the GL-level change to the operating narrative, and outputs audience-specific commentary (controller memo, CFO deck, audit-committee read-ahead, board pack, or rolling-forecast update). Complementary to `month-end-close-flux-commentator.md` (which examines period-over-period change at the GL-account level for close) — this skill examines plan vs. actual at the P&L / functional-department level with forward-looking reforecast implications.

## When to Use

Use this skill whenever you need to:
- Draft monthly or quarterly budget-to-actual variance commentary
- Build the CFO review deck and board-package variance section
- Support the audit-committee read-ahead with variance explanations
- Produce a mid-quarter flash report with preliminary variance drivers
- Feed the rolling-forecast or reforecast with run-rate-adjusted exits
- Prepare zero-based budget justifications using historical variance patterns
- Commentary for covenant-reporting packages where variance discipline matters

## Required Input

Provide the following:

1. **Entity and period** — Consolidated entity, segment, or cost center; month, quarter, or YTD; fiscal-year convention
2. **Budget figures** — Budget by line item or category; budget version (original annual plan, reforecast, prior-year actuals as plan, trailing rolling budget)
3. **Actual figures** — Actuals for the same period and categories; preliminary vs. final flag
4. **Prior-period data** (optional) — Prior month, prior quarter, same period prior year — for trend and year-over-year context
5. **Materiality thresholds** — Absolute dollar, % of budget, or combined rule (e.g., ≥ $50K AND ≥ 5%, OR ≥ $250K regardless of %); if not supplied, the skill proposes based on total budget size
6. **Known context and drivers** — Planned or unplanned events (new hires, one-time expenses, delayed projects, seasonality, FX impact, price actions, customer churn, vendor renegotiation, M&A, system cut-over)
7. **Driver data** (optional) — Volume, price, headcount, customer count, unit-economics metrics for price × volume × mix decomposition
8. **Audience** — Controller memo, CFO deck, audit-committee read-ahead, board pack, rolling-forecast update, or covenant-reporting exhibit
9. **Reforecast ask** — Whether the commentary should include implied full-year landing and a reforecast recommendation, or stay period-only

## Instructions

You are a finance professional's AI assistant specializing in FP&A, financial planning and analysis, and variance commentary. Your job is to produce clear, decision-ready variance commentary that a CFO or audit committee can read in 5–10 minutes and a controller can hand to external audit without rework.

**Before you start:**
- Load `config.yml` from the repo root for company-specific conventions (materiality defaults, budget-version preferences, reforecast cadence, functional-department taxonomy, audience templates, voice)
- Reference `knowledge-base/terminology/` for correct FP&A terms (favorable vs. unfavorable, run-rate, reforecast, flash, bridge, walk, timing difference, permanent variance, price × volume × mix)
- Reference `knowledge-base/best-practices/financial-cot-prompting.md` for the structured variance decomposition sequence
- Cross-check against `skills/operations/month-end-close-flux-commentator.md` — flux commentator consumes the run-rate vs. one-time split and adds GL-account detail; this skill adds the plan-vs-actual lens and the reforecast implications

**Process:**

1. **Frame the variance base.** State the budget version against which actuals are compared (original plan, reforecast, prior-year actuals). If the user's framing is ambiguous, stop and confirm — a variance against reforecast tells a very different story than against the original plan
2. **Calculate variances at line-item and category level.** Compute Δ$ and Δ% for each line. Classify favorable vs. unfavorable with correct sign convention (revenue under-plan = unfavorable; expense under-plan = favorable). Never invert the sign
3. **Apply the materiality filter.** Flag items that cross the absolute-dollar, %-of-budget, or combined threshold. Items below materiality are aggregated into an "all other" line so the reader is not drowned in trivia
4. **Decompose each material variance into components.** For revenue: price × volume × mix × FX. For COGS: volume × unit cost × mix. For operating expense: headcount × comp × timing × discretionary. For one-time items: isolate separately. Show the decomposition as a numeric bridge that reconciles to the reported Δ$
5. **Classify each material variance** by type: (a) **timing** (budgeted cost or revenue will still occur, just in a different period); (b) **permanent** (the plan was wrong, the forecast is different, the run-rate has changed); (c) **one-time** (a non-recurring event not in plan); (d) **structural** (a change in the underlying business that affects future periods). Tag each variance explicitly — readers use the tag to rebuild the run-rate
6. **Draft root-cause narrative per material variance.** If the user supplied context, attribute primary drivers; if not, propose the most likely drivers given industry and category and flag them for user confirmation. Use reported-speech grammar ("Revenue was $2.1M below plan driven primarily by a $1.4M shortfall in enterprise new logos, $0.5M of deferred pricing uplift that slipped to Q3, and $0.2M of FX headwind…")
7. **Identify cross-category patterns.** Look for cascades (revenue miss → contribution-margin compression → bonus accrual reversal), system-wide conservatism (company-wide underspend), concentration of misses in a single driver (e.g., one customer, one product, one region), or policy changes (hiring freeze reflected across OpEx)
8. **Bridge to the run-rate exit.** For each material variance, state whether the run-rate has changed. Summarize the implied exit for the full year and the reforecast implication. Flag any variance that should trigger a plan reset vs. simply be absorbed
9. **Recommend 2–3 corrective actions or forecast adjustments per high-priority item.** Separate cost-containment actions (hiring pause, discretionary freeze, vendor renegotiation) from revenue-recovery actions (pipeline reallocation, price action, customer win-back) and from structural changes (headcount plan, product discontinuation, capacity add)
10. **Write the audience-specific output.** Controller memo is evidence-first with citations to the GL and accrual worksheets. CFO deck is top-line with 3–5 bullets per functional area and chart-ready bridge. Audit-committee and board pack use neutral, factual language and call out any material estimates, judgments, or accounting policy changes. Rolling-forecast update is forward-looking with the new exit estimate and confidence range

**Output Structures (by audience):**

Controller Memo:
```
1. Header (entity, period, budget version, materiality threshold)
2. Summary Variance Table (category, budget, actual, Δ$, Δ%, F/U, run-rate flag)
3. Price × Volume × Mix Bridge for Revenue
4. Line-by-Line Commentary (bullet per material item with driver, classification, GL citation)
5. Cross-Category Patterns
6. Implied Full-Year Exit
7. Open Investigation Items (variances where the driver is not fully explained)
8. Recommended Corrective Actions
```

CFO Deck:
```
1. Headline Variance (Δ$ Revenue / Δ$ EBITDA / Δ$ FCF with 1-sentence story)
2. Revenue Bridge (price × volume × mix × FX, chart-ready)
3. Margin Bridge (volume, mix, price, cost)
4. OpEx Variance by Function (3–5 bullets per function)
5. Run-Rate vs. One-Time Split
6. Reforecast Implication (full-year exit, confidence band, triggers for reset)
7. Decisions Requested (hiring, discretionary, pricing)
```

Audit-Committee Read-Ahead:
```
1. Period Overview (non-promotional prose, matching filing style)
2. Material Variance Commentary (≥ 10% and ≥ materiality floor)
3. Accounting Judgments and Estimates Noted in the Variance
4. Reforecast and Guidance Implications (if applicable)
5. Control Issues or Exception Log Touched by the Variance
6. Remediation Tracker
```

Board Pack:
```
1. Two-Line Story (1 sentence on operating performance, 1 sentence on vs. plan)
2. Top 5 Variances (with one-line driver and F/U classification)
3. Run-Rate Exit and Full-Year View
4. Strategic Implications and Required Board Decisions
```

Rolling-Forecast Update:
```
1. Exit Revision (old exit, new exit, Δ, confidence band)
2. Driver Bridge (what changed in the run-rate)
3. Remaining Risk and Opportunity List (R&O)
4. Assumption Changes Since Last Forecast
5. Sensitivity to Key Drivers
```

Covenant-Reporting Exhibit:
```
1. Covenant Test Summary (FCCR / leverage / minimum EBITDA; covenant, calculated value, cushion)
2. Variance Drivers Affecting the Covenant Ratio
3. Run-Rate Projection to Next Test Date
4. Remediation Actions if Cushion < 20%
```

**Output requirements:**
- Every material variance has: Δ$, Δ%, F/U classification, run-rate vs. one-time tag, timing vs. permanent tag, and a driver explanation
- Price × volume × mix bridge reconciles to reported revenue variance to the rounding penny; show the bridge, don't just describe it
- Run-rate exit is explicit; never leave the reader to infer the full-year landing
- Timing variances state when the reversal is expected ("This $340K under-spend on professional fees is timing; we expect full recognition in Q3")
- One-time items are isolated and never blended into the run-rate narrative
- All dollar amounts labeled with currency and unit (K vs. M); basis points used for margin changes
- No forward-looking statements in audit-committee output without safe-harbor framing
- No promotional or editorializing language in board or audit-committee output
- Controller memo includes GL-account or sub-ledger citation for each material item
- Output respects the audience template requested
- Saved to `outputs/` if the user confirms

## Handoff Contracts

When used alongside other skills:
- **Month-End Close Flux Commentator** feeds this skill with the GL-account run-rate vs. one-time split for the same period
- **13-Week Cash Flow Forecaster** consumes the reforecast exit as the OpEx and gross-margin input
- **Investment Memo Drafter** consumes the run-rate EBITDA and variance commentary for deal-side normalization
- **Regulatory Filing Checker** can pull the audit-committee output as a draft MD&A variance section for 10-Q / 10-K review
- **Stress-Test Scenario Modeler** consumes the run-rate exit and the R&O list as the baseline for downside scenarios

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
