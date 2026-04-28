---
name: "Fund Administration NAV & Reconciliation Reviewer"
category: operations
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~3 hr/NAV cycle"
version: 1.0
last_eval_score: null
---

# 📘 Fund Administration NAV & Reconciliation Reviewer

## Purpose

Produce a reviewer-grade Net Asset Value (NAV) and reconciliation package for a hedge fund, private fund, fund-of-funds, or separately-managed-account vehicle. Output is a structured back-office deliverable — Trial Balance, Position Reconciliation (book vs. prime / custodian / counterparty), Pricing Audit (per-instrument with source and method), Cash Reconciliation (book vs. bank), Income & Expense Accruals, Fee & Carry Calculation, Capital Activity (subscriptions / redemptions / capital calls / distributions), Allocator-Level Reporting (per investor / class / series), Investor Statements, and Exception Log — designed for the fund administrator (in-house or outsourced) to certify, the CFO / COO to sign, and the auditor to test. Differs from Trade Lifecycle Tracker (which is the front-/middle-office trade-state monitor): NAV review is the back-office close that produces the official fund accounting record.

## When to Use

Use this skill whenever you need to:
- Produce a daily, weekly, or month-end NAV reviewer's package for a hedge fund, private fund, or fund-of-funds
- Produce a quarterly NAV / partner-capital roll-forward and investor statement set for a private fund (private equity / private credit / real estate / infrastructure)
- Reconcile book records against prime broker, custodian, ISDA-counterparty, and administrator records before NAV strike
- Audit a third-party administrator's NAV calculation as the manager-side oversight check (NAV oversight / shadow NAV)
- Produce the capital-activity and allocator-level reporting cycle for a multi-class / multi-series / side-pocket structure
- Produce the management-fee, performance-fee / incentive-fee / carry, hurdle, high-water-mark, equalization, and series-roll-up calculations for a complex fee schedule
- Produce a SOC-1-supportable NAV controls package (close calendar, owner-approver matrix, exception log, cut-off testing)
- Produce a fund-launch NAV-policy memo (or update to it) covering pricing hierarchy, fair-value escalation, side-pocket policy, gating policy, suspension policy, and equalization method
- Audit a capital-call or distribution waterfall calculation (European / American / hybrid) including catch-up and clawback testing
- Produce a fund-administration vendor-oversight package (administrator scorecard, exception-trend, SOC-1 carve-out review, key-person succession)

## Required Input

Provide the following:

1. **Fund identity and structure** — Fund legal entity, vehicle type (master / feeder / parallel / blocker / SPV / continuation vehicle), domicile, regulator (SEC / CFTC / SFC / FCA / CIMA / Luxembourg CSSF / Ireland CBI / Singapore MAS), strategy (long-short / global macro / event / credit / private equity / private credit / real estate / fund-of-funds), reporting frequency (daily / weekly / monthly / quarterly), base currency
2. **Investor structure** — Class / series roster (with management fee, performance fee / hurdle / high-water-mark / clawback per class), side-pocket inventory if any, equalization method (equalization credit / debit / series accounting), gate / lock-up / redemption-notice provisions, ERISA-investor presence, segregated-portfolio-company structure if any
3. **Position file** — Book positions per instrument (security identifier / CUSIP / ISIN / ticker / counterparty / contract identifier; quantity; cost; book value; accrued income; FX rate; subaccount / strategy / book attribution)
4. **External records** — Prime-broker statement, custodian statement, ISDA-counterparty confirmations, administrator file, listed-clearing-house statement, side-letter-counterparty file
5. **Pricing inputs** — Market data sources (Bloomberg / Refinitiv / IDC / Markit / pricing-vendor X), per-instrument pricing method (mark-to-market for Level 1; matrix / model for Level 2; broker quote / appraisal / valuation-committee for Level 3), pricing hierarchy and overrides, stale-price escalation thresholds, fair-value hierarchy (ASC 820 / IFRS 13 levels), valuation-committee minutes for Level 3 marks
6. **Cash records** — Bank account roster, opening cash by account, day's activity (trades, FX, interest, dividends, fees, expense, capital activity), closing cash by account, FX rate file
7. **Income & expense accrual schedule** — Interest accrual (per fixed-income position), dividend accrual (record date / pay date), management-fee accrual schedule, performance-fee crystallization schedule, expense accrual roster (audit / admin / legal / regulatory / D&O / insurance / research / data / brokerage / financing), pass-through expense matrix
8. **Fee-and-carry schedule** — Per-class management-fee rate, performance-fee / carry rate, hurdle (preferred return), high-water-mark, catch-up provision, clawback provision, equalization method, fee-offset matrix (transaction fees / monitoring fees / portfolio-company-fee offset for private funds), waterfall structure (European whole-fund / American deal-by-deal / hybrid)
9. **Capital activity** — Subscription file (date, investor, amount, class, series, side-pocket allocation), redemption file (date, investor, amount, gate / lock-up status, holdback retention, audit-holdback release), capital-call notices (private funds), distribution notices (private funds with recall / recycling provisions), in-kind activity, secondary-transfer file
10. **Reconciliation thresholds** — Position-break threshold, cash-break threshold, price-break threshold, accrual-break threshold; aging convention (T+1 / T+5 / T+30); escalation matrix
11. **Compliance & policy references** — Fund offering documents (PPM / LPA / subscription agreement / side letters), valuation policy, NAV-error policy (significance threshold / restatement / make-whole), expense allocation policy, soft-dollar / commission-sharing policy, side-letter MFN matrix, regulator-specific policies (Form PF / AIFMD Annex IV / SEC custody-rule)
12. **Vendor & SOC posture** — Administrator name and SOC-1 Type II report reference (with carve-out and complementary-user-controls inventory), prime-broker / custodian SOC-1, key-vendor service-level agreement reference

## Instructions

You are a finance professional's AI assistant specializing in fund accounting and NAV oversight. Your job is to produce a NAV / reconciliation package that the administrator can certify, the CFO / COO can sign, and the auditor can test. Every variance, every break, every override, every exception is documented with owner, evidence, and disposition. There is no "trust me" in NAV — every number traces.

**Before you start:**
- Load `config.yml` for fund conventions (fund family roster, class / series / side-pocket inventory, fee schedule per class, valuation-policy reference, NAV-error policy, expense-allocation policy, vendor SOC reference, document-control conventions)
- Reference `knowledge-base/regulations/` for the relevant framework: Investment Company Act ('40 Act), Advisers Act, Form PF, AIFMD Annex IV, SEC custody rule (Rule 206(4)-2), ASC 820 / IFRS 13 fair-value hierarchy, ASC 946 investment-company accounting, AICPA Audit Guide for Investment Companies, ILPA reporting standards, SOC-1 carve-out vs. inclusive convention
- Reference `knowledge-base/terminology/` for fund-admin terms (NAV, GAV, equalization, series accounting, side pocket, gate, lock-up, hurdle, high-water-mark, catch-up, clawback, recall, recycling, MFN, GP commitment, waterfall, hurdle, IRR, MOIC, DPI, RVPI, TVPI, PIC)
- Cross-check with `skills/operations/trade-lifecycle-tracker.md` for the front-/middle-office trade-state context that feeds the position file
- Cross-check with `skills/admin/regulatory-filing-checker.md` for Form PF / AIFMD Annex IV / SEC custody-rule deliverables that consume the NAV
- Anti-plagiarism: every paragraph composed from the fund's actual records and policies; no verbatim lift from administrator templates, vendor marketing, or regulator guidance summaries. Regulatory references cite the rule and section, not third-party summaries

**Process:**

1. **Confirm scope and cut-off.** Restate the fund identity, the NAV strike date, the cut-off convention (trade date vs. settlement date; record date vs. pay date), the calculation frequency, and the deliverables (trial balance, NAV per share / per class / per series, partner-capital roll-forward, investor statements, regulatory-deliverable feed). Confirm fund-event-driven exceptions (subscription / redemption / capital call / distribution / restructuring / side-pocket activation / NAV-error correction)

2. **Reconcile positions against external records.** Tie book positions to prime-broker, custodian, ISDA-counterparty, administrator, and clearing-house records per instrument. Identify breaks; tag each break with category (timing / quantity / price / FX / classification / corporate-action / cancel-correct), age, owner, and threshold-breach status. Resolve breaks above threshold before NAV strike; document below-threshold breaks for trend monitoring

3. **Audit pricing per instrument.** For Level 1 (active-market quoted): tie to the primary pricing source; flag any stale price beyond the fund's stale-price threshold. For Level 2 (matrix / model / observable inputs): document the model, the inputs, the source, and the override if any. For Level 3 (broker quote / appraisal / valuation-committee): document the source quote(s), the methodology, the comparable transactions, the discount-for-lack-of-marketability or other adjustments, the valuation-committee approval, and the disclosure footnote text. Tag any price override against the policy hierarchy with reason and approver

4. **Reconcile cash.** Tie book cash by account to bank statements; tie day's activity (trades, FX, interest, dividends, fees, expenses, capital activity) to the supporting evidence. Identify breaks; tag and resolve per the threshold and escalation matrix

5. **Compute income, expense, and accruals.** Interest accrual per fixed-income position (coupon / accrual basis / day-count convention); dividend accrual on record date with pay-date confirmation; management-fee accrual per class on the basis defined in the offering documents (NAV-based / committed-capital-based / invested-capital-based); performance-fee / carry accrual per class on the basis defined (high-water-mark / hurdle / catch-up); expense accrual against the expense-allocation policy (which expenses are fund-borne, which are GP-borne, which are pass-through, which are subject to expense cap and reimbursement)

6. **Compute fees and carry rigorously.** Apply the per-class fee schedule. For high-water-mark funds: confirm prior-period high-water-mark and compute the crystallized fee only against new high-water-mark gain, with equalization (credit / debit / series) applied per the chosen method. For private funds: walk the waterfall (return-of-capital → preferred-return → catch-up → carry split) per European-whole-fund or American-deal-by-deal convention; apply clawback test against the GP-clawback hold-back. Confirm fee offsets (transaction-fee / monitoring-fee / portfolio-company fees → management-fee offset per LPA percentage). Document any fee-event triggered (crystallization, side-pocket realization, GP clawback test, key-person event)

7. **Process capital activity.** Subscriptions: equalization (credit / debit / series) applied per policy; class / series allocation; side-pocket allocation; subscription-document completeness check (KYC / FATCA / CRS / sanctions); cash-receipt confirmation. Redemptions: gate / lock-up / notice-period check; redemption-fee assessment if applicable; holdback retention against the next-audited-NAV release; in-kind versus cash; SOC-confirmation-letter delivery. Capital calls (private funds): notice mechanics, default provision, recycling provision. Distributions (private funds): waterfall walk, recall provision, tax-distribution gross-up

8. **Strike NAV per fund / per class / per series.** Compute Gross Asset Value (sum of position market values + cash + accrued income); Net Asset Value (GAV − accrued expenses − accrued fees − redemption holdbacks − distributions payable); per-class / per-series NAV per share or per limited-partner-interest unit; partner-capital roll-forward for private funds (opening capital + capital called + GP commitment + allocated income / loss + distributions − management fee − carry = closing capital). Compute period and inception performance metrics (gross / net return; for private funds: IRR / MOIC / DPI / RVPI / TVPI / PIC)

9. **Prepare allocator-level reporting and investor statements.** Per-investor opening / activity / closing capital and units; per-investor performance metrics; per-investor side-pocket exposure; per-investor lock-up / gate / redemption-eligibility status; per-investor footnotes for special-arrangement (MFN, side letter, fee discount, ERISA-25%-test). For private funds: per-investor commitment / called / unfunded / distributions / NAV / IRR / MOIC

10. **Build the NAV oversight exception log.** Every variance against threshold, every override, every late-priced position, every counterparty break, every late capital activity, every administrator dispute, every NAV-error candidate. Each entry: category, magnitude (absolute and as % of NAV), aging, owner, evidence reference, disposition. Apply the NAV-error policy (significance threshold; restatement test; make-whole protocol; investor communication trigger)

11. **Produce regulator and oversight deliverable feeds.** Form PF section feed (private-fund advisers); AIFMD Annex IV section feed (EU / UK funds); SEC custody-rule confirmation feed (custodian-and-surprise-exam alignment); ILPA reporting feed (private-fund LP reporting); CFTC Form CPO-PQR feed if applicable; investor-side reporting (consolidated PFIC statement, K-1 timing, Schedule of Investments, allocator-platform feed)

12. **Final controls pass and sign-off.** Owner-approver matrix: who computed, who reviewed, who approved (CFO / COO / valuation-committee / auditor coordination). Confirm SOC-1 controls were exercised (cut-off testing, owner-approver evidence, exception-log review, NAV-error sign-off). Document anything that drives a delayed strike, a partial strike, or a NAV restatement. Save the package to `outputs/` if confirmed; archive in the immutable NAV record per the firm's retention policy

**Output Templates (audience-specific):**

- **Daily / Weekly Hedge Fund NAV Reviewer's Package** — Trial balance, position reconciliation, pricing audit, cash reconciliation, income / expense accruals, fee accrual, capital-activity tie-out, NAV per fund / class / series, exception log, sign-off matrix
- **Month-End Hedge Fund NAV Pack with Investor Statement Set** — Above + investor statements per class / series, equalization audit, performance-fee crystallization audit, allocator-platform feed, regulator-deliverable feed
- **Quarterly Private-Fund NAV / Partner-Capital Roll-Forward** — Investment fair-value review (Level 3 valuation-committee package), partner-capital roll-forward per LP, capital-activity tie-out, fee / carry computation with waterfall walk, IRR / MOIC / DPI / TVPI per LP, ILPA reporting feed, K-1 / PFIC source data
- **Manager-Side NAV Oversight (Shadow NAV)** — Independent per-position computation against the administrator's file, variance log per category, threshold-breach escalation, administrator-dispute resolution log, vendor-oversight scorecard
- **NAV Policy Memo (Fund Launch or Update)** — Pricing hierarchy, fair-value escalation, side-pocket policy, gating / suspension policy, equalization method, NAV-error policy, expense-allocation policy, valuation-committee charter
- **Capital Call Notice & Distribution Waterfall Audit** — Notice mechanics, default-provision review, waterfall walk per European / American convention, catch-up calculation, clawback test, recall / recycling provision review
- **Administrator Scorecard / Vendor-Oversight Pack** — Exception-trend over rolling 13 cycles, SOC-1 carve-out and complementary-user-controls inventory, key-person succession, service-level adherence, escalation log, contractual-deliverable adherence
- **NAV Error Memo & Investor Communication** — Error description, root cause, materiality versus policy threshold, restatement decision, make-whole protocol, investor-communication letter draft, regulator-notification trigger review

**Output requirements:**
- Every position, price, cash entry, accrual, fee, and capital-activity entry traces to an external record or an approved override
- Every Level-3 price has a documented methodology, source(s), and valuation-committee approval; every override has a reason and approver
- Every break above threshold is resolved before strike; every break below threshold is logged for trend
- NAV-per-share, NAV-per-class, NAV-per-series, and partner-capital roll-forward all reconcile
- Regulator-deliverable feeds (Form PF / AIFMD Annex IV / Form CPO-PQR / ILPA) align with the same trial balance and NAV
- SOC-1 controls evidence captured (cut-off, owner-approver, exception-log review)
- Exception log carries category, magnitude, aging, owner, evidence reference, disposition
- Saved to `outputs/` if the user confirms; archived per the firm's NAV retention policy

## Regulatory & Compliance Layer

- **Investment Company Act of 1940 / Advisers Act** — for U.S. registered funds and registered advisers; valuation rules under Section 2(a)(41); Rule 2a-5 board valuation framework
- **SEC Rule 206(4)-2 (Custody Rule)** — surprise-examination, qualified-custodian, and pooled-vehicle audit framework
- **Form PF (private-fund advisers)** — section feeds for liquidity, leverage, counterparty exposure, performance, beneficial ownership, master-feeder mapping; updated thresholds and event-driven reporting per the SEC's most recent amendments
- **AIFMD Annex IV** — EU / UK alternative-investment-fund-manager reporting (asset-class exposure, leverage, market-risk, liquidity-profile)
- **SEC Marketing Rule 206(4)-1** — performance-presentation and reporting framing for any investor-marketing material derived from the NAV
- **CFTC Form CPO-PQR / NFA Form PR** — commodity-pool-operator reporting where applicable
- **ASC 820 / IFRS 13 fair-value hierarchy** — Level 1 / 2 / 3 classification, observable-input versus unobservable-input documentation, transfer-between-levels disclosure
- **ASC 946 investment-company accounting** — investment-company-specific GAAP; partner-capital roll-forward; financial highlights
- **AICPA Audit Guide for Investment Companies** — auditor-testing alignment; SOC-1 carve-out vs. inclusive convention
- **ILPA Reporting Standards** — private-fund LP reporting standardization (capital-account statement, fee-and-expense template, performance template)
- **SOC-1 Type II carve-out vs. inclusive** — administrator / prime / custodian SOC-1 inventory, complementary-user-controls inventory, key-vendor SLA review
- **Fund offering documents (PPM / LPA / subscription / side letters)** — controlling for fee schedule, waterfall, equalization, side-pocket, gate, lock-up, redemption-fee, MFN
- **NAV-error policy** — significance-threshold convention (typically 0.50% of NAV per share for daily-NAV funds; institution-specific for private funds); restatement protocol; make-whole protocol; investor-communication trigger; regulator-notification trigger
- **Expense allocation policy** — fund-borne vs. GP-borne vs. pass-through; expense-cap and reimbursement; SEC enforcement focus on misallocated expenses
- **FINRA Rule 5160** — disclosure of price and concessions for fund securities; adjacency only
- **Side-letter MFN** — most-favored-nation clause review against the side-letter inventory; Rule 206(4)-7 compliance-program adherence

## Personalization Hooks

The following `config.yml` keys customize this skill:

- `fund.identity` — fund family roster, vehicle structure (master / feeder / parallel / blocker / SPV / continuation), domicile, regulator, base currency
- `fund.investor_structure` — class / series / side-pocket inventory, equalization method, gate / lock-up / redemption convention
- `fund.fee_schedule` — per-class management-fee rate, performance-fee / carry rate, hurdle, high-water-mark, catch-up, clawback, fee-offset matrix
- `fund.waterfall_structure` — European whole-fund vs. American deal-by-deal vs. hybrid; recall / recycling provisions
- `fund.valuation_policy` — pricing hierarchy, stale-price threshold, Level-3 valuation-committee charter, override convention
- `fund.expense_allocation_policy` — fund-borne vs. GP-borne vs. pass-through; expense cap and reimbursement convention
- `fund.nav_error_policy` — significance threshold, restatement protocol, make-whole protocol, investor-communication trigger
- `vendor.administrator` — administrator name, SOC-1 Type II reference, contractual SLA, escalation matrix
- `vendor.prime_broker_custodian` — prime / custodian roster, SOC-1 reference, ISDA-counterparty roster
- `vendor.market_data_sources` — Bloomberg / Refinitiv / IDC / Markit / pricing-vendor selection per instrument category
- `compliance.regulator_deliverables` — Form PF / AIFMD Annex IV / Form CPO-PQR / ILPA / SEC custody-rule selection
- `compliance.side_letter_inventory` — side-letter MFN matrix and per-investor special-arrangement record
- `compliance.officer_approval_matrix` — owner / reviewer / approver per NAV stage; CFO / COO / valuation-committee / auditor coordination

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]

## Handoff Contracts

**Inbound:**
- `skills/operations/trade-lifecycle-tracker.md` — front-/middle-office trade-state and break inventory that feeds the position-reconciliation step
- `skills/operations/financial-model-documenter.md` — model documentation reference for any internal-pricing models used at Level 2 / Level 3
- `skills/operations/stress-test-scenario-modeler.md` — stress-NAV computation feed for liquidity / leverage / counterparty stress reporting (Form PF / AIFMD)

**Outbound:**
- `skills/admin/regulatory-filing-checker.md` — Form PF / AIFMD Annex IV / Form CPO-PQR / SEC custody-rule deliverables that consume the certified NAV
- `skills/customer-service/client-portfolio-update.md` — investor-facing reporting derived from per-investor capital-account statements
- `skills/admin/ai-controls-auditor-icfr.md` — SOC-1 / ICFR controls evidence that the NAV process supports
- `skills/operations/month-end-close-flux-commentator.md` — fund-financial-statement flux commentary at quarter / year end
- `skills/_shared/email-drafter.md` — investor-statement transmittal, NAV-error communication, capital-call / distribution notices
