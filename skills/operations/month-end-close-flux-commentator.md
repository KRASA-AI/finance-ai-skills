---
name: "Month-End Close Flux Commentator"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~2 hr/close"
version: 2.1
last_eval_score: 8.30
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

## Audience Templates

1. **Controller Memo** — Investigation-first; bullet per material account with Δ$, Δ%, top driver, confidence rating, evidence citation (sub-ledger / accrual worksheet / PO / invoice reference), and proposed follow-up; internal distribution to the controller and the close team; ties to the close-package workpaper trail
2. **MD&A Draft** — External-facing prose in "we" voice, SEC-filing tone, no forward-looking without safe-harbor framing, no un-aggregated detail that exposes competitive information; suited for direct hand-off to the MD&A drafter for the 10-Q / 10-K filing
3. **CFO Review Deck** — Top-3 drivers per material line; chart-ready Δ breakdown; one-sentence explanation per bar; run-rate vs. one-time split; suited for the daily / weekly CFO close stand-up during close week
4. **Audit-Support Binder** — Evidence-first; citation to sub-ledger / PO / invoice / accrual worksheet for every flagged item; tied to JE if posted; PCAOB AS 2305 substantive-analytical-procedure-ready; designed for external audit field-work review
5. **Compliance-Close Flux** — Anomaly-log-forward; emphasizes accounts where proposed drivers do not reconcile, where the direction of Δ is inconsistent with known business drivers, or where the sub-ledger detail does not tie to the GL; suited for the compliance / internal-audit close-attestation cycle
6. **Pre-Close Peek Flash** — Preliminary flux on T-2 / T-1 actuals; "watch list" of items likely to require accrual adjustment, cut-off entry, or intercompany true-up before the period is locked; suited for the in-close last-2-day controller cadence
7. **Multi-Entity Consolidation Flux** — Flux at the legal-entity level with intercompany elimination context; useful for multi-entity closes where consolidation eliminations may obscure entity-level drivers; routes to consolidation-pre-check before the consolidated TB is locked
8. **Segment / BU Flux** — Flux at the operating-segment level aligned to the CODM-reviewed measure (ASC 280 / IFRS 8); supports the segment-disclosure footnote and the segment-level operating review

## Regulatory & Compliance Layer

- **ASC 250 / IAS 8 (Accounting Changes and Error Corrections)** — Flux commentary that surfaces a prior-period adjustment must invoke the ASC 250 / IAS 8 framework — change in accounting principle vs. change in estimate vs. correction of error — with appropriate retrospective or prospective treatment; the commentary flags the candidate adjustment for controllership and external-auditor review before posting
- **SEC Reg S-K Item 303 (MD&A)** — The MD&A-draft template aligns to the Item 303 standard for period-over-period commentary: identify the known trends, demands, commitments, events, or uncertainties that have had or are reasonably likely to have a material effect; tie commentary to the financial statement line item being explained
- **Reg G (Non-GAAP Reconciliation)** — Where the flux commentary references non-GAAP measures (Adjusted EBIT, organic growth, constant-currency revenue), the most directly comparable GAAP measure and a reconciliation accompany the non-GAAP figure; never present non-GAAP without the reconciliation
- **SOX §302 / §906 (Management Certifications)** — Flux commentary is a process input to the quarterly / annual management certification; material flux items driving changes to ICFR-relevant judgments are flagged to the controller for control-attribute documentation; the flux process itself is a key ICFR control
- **PCAOB AS 2305 (Substantive Analytical Procedures)** — The audit-support binder format aligns to the auditor's analytical-procedure documentation requirements; the level of disaggregation, the precision of the expectation, and the explained-vs-unexplained Δ are documented
- **PCAOB AS 2201 (ICFR Assessment)** — Close-flux is a key control; controller preparer / reviewer sign-off, timeliness, and evidence are documented per AS 2201 control-attribute requirements
- **ASC 230 / IAS 7 (Cash Flow Statement)** — Flux on BS accounts that flow to the indirect-method CFS is reconciled to the CFS movement; non-cash items are isolated; CFS-impact flux items are flagged for the CFS preparer
- **ASC 740 / IAS 12 (Income Taxes)** — Flux on tax-related accounts (DTA / DTL, current tax payable, ETR drivers) is reconciled to the tax provision workpaper; valuation-allowance changes flagged for tax-department review
- **ASC 805 / IFRS 3 (Business Combinations)** — For closes following an acquisition, flux includes opening-balance-sheet measurement-period adjustments; the commentary distinguishes measurement-period adjustments from current-period operating drivers
- **ASC 350 / 360 / IAS 36 (Impairment)** — Flux that surfaces an impairment indicator (sustained margin decline, customer-concentration loss, sustained share-price decline below carrying value) is flagged for impairment-trigger evaluation
- **MNPI / Wall-Cross / Restricted-List Controls** — Flux commentary on a covered issuer is MNPI until publicly disseminated; the skill respects the firm's restricted-list, the wall-cross register, and the 10b5-1 plan posture for any covered issuer
- **Books-and-Records Retention** — Flux workpapers retained per SOX §802 / 18 U.S.C. §1519 (public companies, 7-year minimum), SEC Rule 17a-4 (broker-dealers), and SSAE 18 (service organizations)
- **SEC 2026 Examination Priorities** — Where AI / ML assists the driver-hypothesis generation or the materiality scoring, "AI cannot operate as a black box" — the methodology is documented, the inputs are auditable, the human controller's review of every material item is preserved, and the AI assistance is disclosed in the close-package narrative where required

## Personalization Hooks

Configure via `config.yml` in the repo root:

```yaml
month_end_flux_commentator:
  erp_system: "SAP S/4HANA"                  # ERP or GL system (SAP, Oracle Fusion, NetSuite, Workday, etc.)
  reporting_currency: "USD"
  base_unit: "thousands"                     # thousands | millions
  fiscal_calendar: "4-4-5"                   # calendar | 4-4-5 | 4-5-4 | 5-4-4 | 13-period
  close_calendar:
    soft_close_day: 3                        # business day on which preliminary actuals lock
    hard_close_day: 5                        # business day on which books lock
    consolidation_complete_day: 7            # business day on which consolidated TB is final
  materiality_thresholds:
    absolute_dollar: 25000                   # $ threshold for inclusion in flux
    percent_change: 0.10                     # % threshold for inclusion in flux
    combined_rule: "AND"                     # AND | OR — combined-threshold logic
    bs_account_floor: 100000                 # higher floor for BS accounts
    is_account_floor: 25000                  # IS account threshold
  recurring_item_list:                       # known recurring drivers carried forward in commentary
    - "monthly rent — escalator clause"
    - "quarterly bonus accrual"
    - "Q4 audit-fee accrual ramp"
    - "annual insurance prepayment amortization"
  commentary_style_default: "controller-memo" # controller-memo | mda-draft | cfo-deck | audit-support | compliance-close | flash | multi-entity | segment
  external_filing_alignment: true            # MD&A-draft template aligns to issuer's MD&A style
  segment_reporting_alignment: "operating-segments"  # operating-segments | reportable-segments | none
  fx_treatment: "ASC 830 current-rate"       # ASC 830 current-rate | ASC 830 remeasurement | IAS 21
  prior_period_carryforward: true            # include carry-forward statement on recurring items
  proposed_je_threshold: 5000                # $ threshold above which the AI proposes a JE rather than just commenting
  voice: "controller-neutral"                # voice register for narrative
  output_format: "controller-memo"           # controller-memo | mda-draft | cfo-deck | audit-support | compliance-close | flash | multi-entity | segment
```

## Handoff Contracts

**Inbound (this skill consumes from):**
- **General Ledger Reconciler** supplies the reconciled trial balance as the authoritative source; unreconciled balances are flagged for resolution before flux commentary begins
- **AI Controls Auditor (ICFR)** supplies the SOX / ICFR control-context that frames the close-flux as a key control with documented preparer / reviewer / timeliness attributes
- **Three-Statement Model Constructor** supplies the budget / forecast comparison base where flux is run against forecast rather than against prior period

**Outbound (this skill feeds into):**
- **Budget Variance Analyzer** receives the run-rate vs. one-time split to isolate plan-vs-actual drivers (the flux commentator and the BVA together produce the close-cycle period-over-period + plan-vs-actual narrative)
- **Three-Statement Model Constructor** receives the run-rate adjustments to refresh the rolling forward model assumptions
- **Earnings Call Summarizer** receives the top-3 flux drivers as "how to talk about the quarter" inputs for the earnings-call prep
- **Regulatory Filing Checker** receives the MD&A-draft output as the candidate MD&A section for 10-Q / 10-K review; close-flux commentary that crystallizes after period-end but before filing is flagged for subsequent-event evaluation
- **Investment Memo Drafter** receives the run-rate EBIT for deal-side normalization
- **AI Controls Auditor (ICFR)** receives the flux-control attribute log (preparer, reviewer, timeliness, evidence, anomalies flagged) for ICFR control-testing workpaper population
- **Meeting Summarizer** receives the CFO-deck-style summary as the operating-review-meeting input
- **Email Drafter** receives the close-summary headline as the input for the controller's close-completion email to the CFO
- **Outputs/** archive for retention per the firm's records-retention policy (SOX §802, SEC 17a-4, SSAE 18)

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]

## Anti-Plagiarism Note

Every flux explanation, every driver hypothesis, every proposed JE, every audience-template narrative is generated per-input from the user-supplied trial balance and expected-driver list and the KRASA v2.1 skill idiom. Concepts of materiality-filtered flux commentary, run-rate vs. one-time tagging, driver hypothesis with confidence scoring, anomaly flagging, and MD&A-aligned commentary are drawn from public CPA / CFA / FP&A practitioner literature, AICPA / IIA / PCAOB guidance, and SEC Reg S-K / Reg G standards (concepts only, no verbatim content). Vendor ERP / sub-ledger references in the personalization hooks are configuration values only; no vendor documentation has been copied. No prior-issuer MD&A, earnings-release commentary, or close-package workpaper text is reproduced.
