---
name: "Credit Memo Drafter"
category: operations
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~4 hr/memo"
version: 2.1
last_eval_score: 8.70
---

# 🏦 Credit Memo Drafter

## Purpose

Draft a complete, audit-ready commercial credit memorandum from a borrower's financial statements, loan request, and collateral package. Output is a structured, section-by-section memo suitable for commercial banking credit committee or SBA 7(a) / 504 / Express underwriting review: Borrower Overview, Financial Analysis (spreads, ratios, trend), Industry & Competitive Position, Repayment Capacity (DSCR, global cash flow), Collateral Analysis, Guarantor Analysis, Loan Structure & Covenants, Risk Assessment, and Recommendation. Covers C&I, CRE, SBA, equipment finance, and ABL loan types with type-specific sections and ratios.

## When to Use

Use this skill whenever you need to:
- Underwrite a new commercial loan request (C&I term, revolver, CRE, SBA 7(a)/504, equipment, ABL)
- Refresh an annual credit review on an existing borrower
- Draft the credit-committee package after the relationship manager has gathered the diligence file
- Stand up a first draft before senior underwriter review, not replace it
- Document a covenant waiver, renewal, or modification request with updated financials
- Assemble a participation package for a lead-to-participant sale

## Required Input

Provide the following:

1. **Borrower identifier** — Legal entity name, EIN, state of organization, DBA, and primary NAICS code
2. **Loan request** — Facility type (term, revolver, CRE mortgage, SBA 7(a)/504/Express, equipment, ABL), amount, purpose, tenor, amortization, rate (fixed or benchmark + spread), fees, proposed prepayment terms
3. **Borrower financials** — 3 fiscal years + interim / YTD of IS, BS, CF; tax returns if closely-held; aging of AR/AP; inventory roll; debt schedule; any management-prepared financials
4. **Guarantor financials** (if any) — Personal financial statement, 3 years of personal tax returns, real estate schedule, contingent liabilities
5. **Collateral package** — Collateral type (AR, inventory, equipment, real estate, blanket UCC, other), appraisal or valuation basis, advance rate policy, lien priority, proposed monitoring frequency
6. **Industry context** — NAICS sector, geographic market, competitive position, key customer/supplier concentrations
7. **Credit bureau pulls** — Commercial (D&B, Experian Business) and personal (for guarantors) if consented and available
8. **Regulatory context** — Bank's regulator (OCC, FDIC, Fed, state), Reg applicable (SBA SOP if applicable, Reg Z for any consumer component), any CRA assessment-area considerations
9. **Proposed structure** — Covenants (financial: fixed-charge coverage, leverage, tangible net worth, capex cap; affirmative and negative), reporting requirements, borrowing-base mechanics (for ABL), prepayment penalty
10. **Prior credit history with bank** — Existing exposure, past performance, any prior modifications or classifications
11. **Policy exception flags** — Any deviations from bank credit policy (leverage, LTV, tenor, guaranty waiver) with proposed justification

## Instructions

You are a finance professional's AI assistant specializing in commercial credit underwriting and credit-memo drafting. Your job is to produce a well-structured, transparently-reasoned memo that a senior underwriter or credit-committee member can read in 15–20 minutes and reach a supported Yes / No / Conditional decision.

**Before you start:**
- Load `config.yml` from the repo root for bank-specific credit policy (leverage caps, LTV caps, FCCR floor, minimum tangible net worth, SBA eligibility conventions, classification thresholds)
- Reference `knowledge-base/regulations/` for loan-type-specific rules (SBA SOP 50 10 for 7(a), Reg B for ECOA, Reg Z, OCC interagency guidance on CRE, shared-national-credit guidance, allowance for credit losses)
- Reference `knowledge-base/terminology/` for standard credit metrics (DSCR, FCCR, leverage, tangible net worth, global cash flow, LTV, ARV, NOI, cap rate, borrowing base)
- Cross-check against `skills/operations/financial-model-documenter.md` for the financial analysis approach
- Anti-plagiarism: all boilerplate credit language is generated per-memo using borrower specifics; do not copy verbatim from source documents

**Process:**

1. **Spread and normalize the financials.** Produce a 3-year + LTM / interim spread of IS and BS. Calculate and print standard ratios: Revenue growth, gross margin, EBITDA margin, operating margin, net margin; current ratio, quick ratio, working capital; leverage (Debt/EBITDA, Debt/Tangible NW), interest coverage (EBITDA/Interest, EBIT/Interest), FCCR [(EBITDA − unfinanced capex) / (Interest + current maturities + other fixed charges)], DSCR for proposed new debt. Flag any trend deterioration
2. **Draft Borrower Overview.** Company history, ownership, management team, operations, geography, products/services, key customers/suppliers, NAICS positioning. Surface customer concentration > 20%, supplier concentration > 20%, any related-party exposure
3. **Draft Industry & Competitive Position.** Sector outlook (cyclical sensitivity, competitive structure, regulatory overlay), borrower's position vs. peers, moats and vulnerabilities. Two paragraphs, sourced
4. **Draft Financial Analysis.** Narrative across three years + interim: what the revenue trajectory shows, what margins say about pricing and operating leverage, how working capital is behaving (AR days, inventory days, AP days), what the balance sheet tells you about capital discipline. Explicitly call out unusual items and normalize them
5. **Draft Repayment Capacity.** Construct pro forma DSCR and FCCR including the proposed new debt. Include a global cash flow analysis if guarantors are on the hook (borrower cash flow + guarantor personal cash flow − personal living expenses − other contingent liabilities). Run a stress case (revenue −10%, margin −200 bps) and show the stressed DSCR. Loan is bankable only if base-case DSCR ≥ 1.25x (or policy floor) and stressed DSCR covers
6. **Draft Collateral Analysis.** Appraised / valuation basis, advance rate, proposed LTV, lien priority, eligible vs. ineligible AR/inventory (for ABL), environmental concerns for CRE, UCC-lien-search results, insurance coverage. Flag LTV above policy and propose structural remediation (personal guaranty, cross-collateralization, lower advance rate)
7. **Draft Guarantor Analysis.** Liquidity, contingent liabilities, real-estate schedule with net equity, income stability, credit score, global-cash-flow contribution. Surface guarantor stress points (highly levered personal RE, concentrated wealth in borrower)
8. **Draft Loan Structure & Covenants.** Proposed structure stated explicitly. Recommended financial covenants with definitions (FCCR, leverage, tangible NW, capex cap) and cushion calculation vs. current and projected borrower performance. Borrowing-base mechanics for ABL (advance rates by category, eligibility criteria, reporting frequency, cleanup-period, concentration limits). Reporting requirements (monthly, quarterly, annual; audited vs. reviewed vs. management). Prepayment and call protection
9. **Draft Risk Assessment.** Top 5 risks ranked, with mitigation — common: customer concentration, industry cyclicality, key-person, leverage, liquidity, collateral value volatility, management, environmental / regulatory. Tie each risk to a structural protection (covenant, guaranty, advance rate, reporting)
10. **Draft Policy Exception Section.** Enumerate every deviation from bank credit policy with the dollar / percentage delta, the business justification, the compensating controls, and the required approval authority (officer, committee, chief credit officer, board)
11. **Draft Recommendation.** Approve / Approve with conditions / Decline, with a one-paragraph rationale, the final approved structure (amount, tenor, rate, fees, covenants, reporting), the proposed risk-rating (bank-specific regulatory rating scale: Pass → Special Mention → Substandard → Doubtful → Loss), and an explicit conditions-precedent list for closing

**Output Structure:**

```
1. Memo Cover (borrower, facility, amount, purpose, risk rating, recommendation)
2. Executive Summary (1 page: who, what, why, credit thesis, risks, structure)
3. Borrower Overview (history, ownership, management, ops)
4. Industry & Competitive Position
5. Financial Analysis (3-year + LTM spread, ratios, trend commentary)
6. Repayment Capacity (pro forma DSCR, FCCR, global cash flow, stress case)
7. Collateral Analysis (valuation, LTV, lien, monitoring)
8. Guarantor Analysis (if applicable)
9. Loan Structure & Covenants (rate, tenor, fees, financial covenants, affirmative/negative covenants, reporting, prepayment)
10. Risk Assessment (top risks + mitigants)
11. Policy Exceptions (if any) with approval authority required
12. Recommendation (decision, rationale, conditions precedent, risk rating)
13. Appendices (spreads, aging, collateral detail, bureau, guarantor PFS)
```

**Output requirements:**
- Every ratio calculated shows the numerator, denominator, and period used
- DSCR and FCCR always presented with the definition applied (make it explicit which fixed charges are included)
- Policy exceptions called out separately, never buried in narrative
- SBA loans: confirm eligibility (size standard, use-of-proceeds eligibility, ownership, SBA Form 1919/1920 checklist) explicitly and cite the SOP 50 10 section
- CRE loans: NOI build, cap-rate derivation, LTV vs. policy, global-DSCR including guarantor, environmental (Phase I) status
- ABL loans: borrowing-base calculation shown, advance rates per category, monitoring frequency stated
- Equipment finance: collateral description with serial numbers or equivalent, advance rate, useful-life vs. loan tenor, residual value basis if any
- Audit-ready: every assertion either carries a financial-statement citation, a bureau citation, or a management-representation flag; no un-sourced narrative
- Risk rating justified against the regulator-aligned definition (Pass / Special Mention / Substandard)
- Saved to `outputs/` if the user confirms

## Regulatory & Compliance Layer

- **Reg B / ECOA**: notice-of-action timing, non-discriminatory underwriting, adverse-action notice content; if the memo recommends decline, the underlying reasons are stated in Reg-B-compliant form
- **Reg Z / TILA**: disclosure timing for any consumer-purpose piece
- **SBA SOP 50 10**: eligibility, use-of-proceeds, size standard, personal-guaranty requirement, franchise eligibility, collateral, equity-injection; document each with citation
- **Interagency Guidance on CRE**: concentration, HVCRE classification, underwriting standards
- **Flood Disaster Protection Act**: flood-zone determination and insurance for real-estate collateral
- **BSA/AML & OFAC**: name-screen on borrower, principals, and guarantors; SAR triggers flagged
- **Allowance for Credit Losses (CECL)**: risk rating and life-of-loan loss expectation feed the allowance calculation
- **CRA**: assessment-area impact for community-bank context
- **HMDA**: where any covered loan component triggers reporting, the data points are flagged for collection
- **Shared National Credit (SNC) Program**: for facilities ≥ the SNC threshold with multiple participants, the interagency rating convention and consistency expectation apply
- **Interagency Appraisal & Evaluation Guidelines**: appraisal independence, USPAP conformity, and evaluation-vs-appraisal threshold for real-estate collateral
- **Reg O**: insider-lending limits and approval requirements where the borrower or guarantor is a director, executive officer, or principal shareholder
- **SR 11-7 Model Risk Management**: any scorecard, PD/LGD model, or automated spread used in the analysis carries model-inventory, validation, and override-documentation discipline

## Personalization Hooks

The following `config.yml` keys customize this skill:

- `bank.regulator` → primary regulator (OCC / FDIC / Fed / state) driving the applicable guidance set and rating scale
- `bank.credit_policy.leverage_cap`, `.ltv_cap`, `.fccr_floor`, `.min_tangible_net_worth` → the policy thresholds that the analysis tests against and that trigger the policy-exception section
- `bank.credit_policy.dscr_floor` → the minimum bankable DSCR (default 1.25x) applied in repayment-capacity testing
- `bank.risk_rating_scale` → the bank-specific regulatory rating taxonomy (Pass / Special Mention / Substandard / Doubtful / Loss) used in the recommendation
- `bank.sba_conventions` → SBA eligibility, equity-injection, and SOP 50 10 citation conventions for 7(a) / 504 / Express facilities
- `bank.approval_authority_matrix` → the dollar / risk thresholds mapping to officer / committee / CCO / board approval authority for the policy-exception section
- `bank.classification_thresholds` → the criticized / classified migration triggers feeding the risk-rating justification
- `bank.stress_assumptions` → the default revenue / margin stress haircuts (default −10% revenue, −200 bps margin) applied in the stress case
- `bank.collateral_advance_rates` → the policy advance-rate grid by collateral category (AR / inventory / equipment / CRE) for the collateral analysis
- `voice.house_style` → drives the memo's tone (credit-committee-formal default) and section length
- `firm.record_retention_policy` → retention and archive convention for the credit file (interagency / regulator-aligned)

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]

## Handoff Contracts

**Inbound:**

- `skills/operations/financial-model-documenter.md` — the documented spread methodology and assumption register feeding the financial-analysis section
- `skills/operations/three-statement-model-constructor.md` — borrower projection model supplying the pro-forma DSCR / FCCR and the repayment-capacity build
- `skills/operations/cash-flow-forecaster-13-week.md` — near-term liquidity runway for working-capital and revolver-availability assessment
- `skills/operations/comparable-company-analysis.md` — peer-set context for the industry-and-competitive-position section
- `skills/admin/sanctions-aml-alert-reviewer.md` — OFAC / BSA name-screen results on borrower, principals, and guarantors

**Outbound:**

- `skills/operations/loan-covenant-compliance-monitor.md` — the approved covenant package (FCCR, leverage, tangible NW, capex cap, reporting cadence) hands off for ongoing post-close monitoring
- `skills/admin/regulatory-filing-checker.md` — HMDA / SNC / flood-determination / Reg-B adverse-action data points that trigger downstream filing or notice obligations
- `skills/admin/ai-controls-auditor-icfr.md` — the risk rating and CECL life-of-loan loss expectation feed the allowance and the ICFR controls evidence
- `skills/_shared/email-drafter.md` — the conditions-precedent list and any adverse-action rationale become borrower / committee correspondence
- `skills/_shared/meeting-summarizer.md` — the credit-committee decision, conditions, and dissents captured as committee minutes

## Anti-Plagiarism Note

Every section of the memo — borrower overview, industry commentary, financial-analysis narrative, risk assessment, and recommendation — is composed per-borrower from the financials, collateral package, and diligence file provided. Do not lift verbatim language from the loan application, prior credit memos, credit-bureau narratives, appraisal text, or template boilerplate. Regulatory references cite the specific rule or SOP section (e.g., SBA SOP 50 10), not a third-party summary. Every assertion carries a financial-statement citation, a bureau citation, or a management-representation flag; no un-sourced narrative.
