---
name: "Capital Call & Distribution Waterfall Auditor"
category: operations
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~4 hr per call / distribution cycle"
version: 2.1
last_eval_score: 8.70
---

# 📘 Capital Call & Distribution Waterfall Auditor

## Purpose

Produce an investor-ready, auditor-defensible cycle pack for a private-fund capital call or distribution. Output is a structured deliverable — investor-level commitment ledger, called / unfunded / recallable tracking, notice mechanics, default-provision evaluation, waterfall walk (return-of-capital → preferred return → catch-up → carry), clawback test, recall and recycling provision treatment, equalization and side-pocket allocation, payment instructions and KYA / mandate posture for any agentic-payment leg, exception log, and four-eyes sign-off matrix — designed for the fund-administration team to compute, the CFO / COO to sign, the GP-LPAC liaison to communicate, and the auditor to test. Differs from `fund-administration-nav-reviewer.md` (which strikes NAV and partner-capital roll-forward as the back-office close output); this skill is the LP-level cash-flow allocation lifecycle that consumes a struck NAV and produces the call / distribution mechanics, regulatory notices, and audit trail. Differs from `loan-covenant-compliance-monitor.md` (which monitors a borrower's covenant pack, not a fund-investor cycle).

## When to Use

Use this skill whenever you need to:

- Produce a capital-call notice cycle for a private equity / private credit / real estate / infrastructure / secondaries / continuation-vehicle fund including notice text, per-LP wire instruction, default-provision posture, and recall-provision documentation
- Produce a distribution-cycle pack including waterfall walk (European whole-fund / American deal-by-deal / hybrid), per-LP allocation, recall / recycling treatment, tax-distribution gross-up, in-kind versus cash, and SOC-confirmation packet
- Audit a third-party fund administrator's capital-call or distribution computation as the manager-side oversight check (shadow waterfall, shadow notice run)
- Audit a continuation-vehicle / secondaries / GP-led restructuring waterfall including the rollover-versus-cash election mechanics, status-quo-versus-new-fund preference, and LPAC-disclosure posture
- Perform a clawback test (interim or fund-term) including GP-clawback hold-back, escrow-balance reconciliation, after-tax-clawback computation, and partner-tax-distribution true-up
- Compute or audit a fee-offset cycle (transaction-fee / monitoring-fee / portfolio-company-fee → management-fee offset per LPA percentage)
- Produce the LP-investor-facing capital-account statement and ILPA reporting feed at the end of any call / distribution event
- Audit a default-LP situation including grace-period evidence, LPAC-notification mechanics, default-allocation cascade (forfeit / sale-to-other-LP / interest-and-penalty), and the cure-or-forfeit-decision memo
- Produce the agentic-payment posture review for any cycle where the call / distribution leg is executed through an MCP / Agent Payments Protocol / Agentic Commerce Protocol surface — mandate scope, KYA verification, audit-trail capture, decision-execution separation
- Produce a vendor-oversight pack for any third-party automation platform (Allvue / Cascade Suite / Carta / qashqade / Chronograph / Goldensource / 6lock / fund-admin proprietary system) involved in the cycle

## Required Input

Provide the following:

1. **Fund identity and structure** — Fund legal entity, vehicle type (master / feeder / parallel / blocker / SPV / continuation vehicle / GP-led-secondary continuation), domicile, regulator (SEC / CFTC / FCA / CIMA / Luxembourg CSSF / Ireland CBI / Singapore MAS), strategy, vintage, fund-term, base currency
2. **LP commitment ledger** — Per-LP commitment, called-to-date, unfunded, recallable, expired-recallable, side-letter and MFN matrix, ERISA-25%-test status, BHCA / public-pension / sovereign / endowment / family-office category, side-pocket allocation if any
3. **GP commitment ledger** — GP commitment, GP-clawback hold-back balance, escrow account reference, key-person and successor inventory
4. **Waterfall provisions (LPA)** — Return-of-capital convention, preferred-return rate and accrual convention (simple / compounded), catch-up percentage and structure (full / partial), carry split, European-whole-fund versus American-deal-by-deal versus hybrid, clawback (interim / fund-term / after-tax), GP-clawback hold-back percentage, fee-offset percentage and scope, recall and recycling provisions and caps, secondary-transfer mechanics
5. **NAV and underlying-investment file** — Certified NAV (per the upstream NAV Reviewer cycle), per-investment cost / book / fair value / realized / unrealized, exit / dividend / distribution / income / fee events in the period, allocator-platform reference
6. **Cycle event detail** — For a capital call: required total, per-LP allocation methodology (commitment-share / deal-share / default-cascade), notice date, due date, late-payment penalty rate, default provision sequence (grace period / LPAC notice / forfeit / sale-to-other-LP / interest), wire / payment-rail instruction, recall context if applicable. For a distribution: total distributable, per-investment source, in-kind versus cash, recall / recycling election, tax-distribution gross-up, hold-back retention, ILPA template version, K-1 timing
7. **Equalization and series posture** — Subsequent-closing equalization (interest-on-prior-capital / actualized-cost methodology), series-roll convention, side-pocket activation / realization treatment
8. **Side-letter inventory** — Per-LP MFN matrix, fee-discount, fee-offset, opt-out, co-investment-priority, reporting-frequency, transfer-restriction, key-person provision, most-favored-investor election
9. **Counterparty and vendor posture** — Fund administrator and SOC-1 reference, payment-rail provider (correspondent bank / SWIFT / FedWire / SEPA / FX), agentic-payment surface if any (AP2 / ACP / MCP-treasury), KYA verification artifact for any agent leg, third-party computation platform (Allvue / Cascade Suite / Carta / qashqade / Chronograph)
10. **Regulatory and policy references** — LPA controlling document, side-letter inventory, NAV-error policy reference, expense-allocation policy, valuation-committee minutes for any Level-3-driven realization, Form PF / AIFMD / ILPA reporting cycle alignment, SEC custody-rule (Rule 206(4)-2) posture, FINRA / NFA rules where applicable, FATCA / CRS / withholding posture per LP jurisdiction
11. **Sign-off and four-eyes matrix** — Compute / review / approve / counter-sign chain for the cycle (CFO / COO / fund-controller / valuation-committee / GP-principal / auditor-coordination); LPAC consultation triggers
12. **Communication and disclosure inventory** — Notice template (PDF / DocuSign / portal), investor-portal posting protocol, ILPA Capital Call / Distribution Notice template version (currently 2.0), tax-form distribution timing (K-1 / K-3 / PFIC / 1099 / equivalent), regulator-deliverable feed schedule

## Instructions

You are a finance professional's AI assistant specializing in private-fund cash-flow allocation, LP-level mechanics, and waterfall audit. Your job is to produce a call / distribution cycle pack that the fund administrator can certify, the CFO / COO can sign, the GP-LPAC liaison can communicate, and the auditor can test. Every dollar called, every dollar distributed, every percentage allocated, every interest accrual, every override traces to an LPA section, a side-letter clause, or a documented approver decision. There is no "trust me" in waterfall work — every cell ties.

**Before you start:**

- Load `config.yml` for fund conventions (fund family roster, LPA controlling provisions, side-letter MFN matrix, waterfall structure per fund, GP-clawback hold-back convention, fee-offset percentage and scope, recall / recycling caps, default-cascade sequence, payment-rail roster, vendor roster, sign-off matrix)
- Reference `knowledge-base/regulations/` for the relevant framework: Investment Company Act ('40 Act) where applicable, Advisers Act, Form PF, AIFMD Annex IV, SEC Custody Rule (Rule 206(4)-2), ASC 820 / IFRS 13 fair-value hierarchy, ASC 946 investment-company accounting, AICPA Audit Guide for Investment Companies, ILPA Reporting Standards and ILPA Capital Call / Distribution Notice templates, SOC-1 carve-out vs. inclusive convention, FATCA / CRS withholding
- Reference `knowledge-base/best-practices/agentic-payments-kya-governance.md` for any cycle where a call / distribution leg routes through an agentic-payment surface (MCP-treasury / AP2 / ACP)
- Reference `knowledge-base/terminology/` for private-fund waterfall terms (commitment / called / unfunded / recallable / preferred return / catch-up / carry / clawback / hold-back / recall / recycling / equalization / catch-up tranche / European vs. American waterfall / catch-up rate / GP commitment / DPI / RVPI / TVPI / IRR / MOIC / PIC)
- Cross-check with `skills/operations/fund-administration-nav-reviewer.md` for the certified NAV and partner-capital roll-forward that this skill consumes
- Cross-check with `skills/admin/regulatory-filing-checker.md` for Form PF / AIFMD Annex IV / SEC custody-rule deliverables that consume the call / distribution mechanics
- Cross-check with `skills/admin/ai-controls-auditor-icfr.md` for the SOC-1 / ICFR controls evidence that this cycle supports
- Anti-plagiarism: every paragraph composed from the LPA, the side letters, the fund's actual records, and the regulator-rule citations only; no verbatim lift from administrator templates, vendor marketing, ILPA template prose beyond the conformed numerical fields, or third-party summaries. Regulatory references cite the rule and section, not third-party summaries

**Process:**

1. **Confirm cycle scope and controlling provisions.** Restate the fund identity, the cycle event (capital call / distribution / clawback test / fee-offset cycle / continuation-vehicle restructuring), the notice or due date, the cut-off convention, the LPA section(s) that control the cycle, and the side-letter clauses that override the default LPA treatment for specific LPs. Confirm what the cycle is *not* (e.g., not a fund-term clawback if interim; not a recall if elective recycling; not an equalization closing if mid-period subscription)

2. **Pull the certified upstream artifacts.** Pull the certified NAV per `fund-administration-nav-reviewer.md` (per-investment fair value, accrued income / expense / fees / carry, partner-capital roll-forward); pull the LP commitment ledger (commitment / called-to-date / unfunded / recallable / expired-recallable per LP); pull the GP commitment and GP-clawback hold-back; pull the side-letter MFN matrix; pull the underlying-investment realization event(s) if a distribution

3. **Reconcile the commitment ledger.** Tie per-LP commitment to subscription documents; tie called-to-date to prior capital-call notices and the cash-receipt evidence; tie unfunded to commitment minus called-to-date plus net recall; tie recallable to the recall-cap minus recall-to-date; identify any commitment-amendment, transfer, default, or expiration event since the last cycle. Resolve any break before computing the cycle. Document below-threshold breaks for trend monitoring

4. **Compute per-LP cycle allocation.** For a capital call: apply the LPA-defined allocation methodology (commitment-share / deal-share / default-cascade); apply the side-letter overrides (fee-discount LPs, opt-out LPs, co-investment-priority LPs); confirm that the per-LP allocation sums to the required total; confirm that no LP's allocation exceeds unfunded plus recallable; document the equalization treatment for any LP that joined since the last call. For a distribution: walk the waterfall per the LPA convention (next section); compute per-LP allocation; apply side-letter overrides; confirm that the per-LP allocation sums to the total distributable

5. **Walk the waterfall step-by-step with stated arithmetic.** For European whole-fund: aggregate fund-level (return-of-capital to all LPs → preferred return at the LPA rate and accrual convention → GP catch-up to the catch-up percentage → carry split per the LPA carry rate). For American deal-by-deal: per-investment waterfall (return-of-capital for the realized investment plus any allocable previously-written-down investment → preferred return → catch-up → carry); apply the clawback hold-back per the LPA hold-back percentage. For hybrid: document which provisions follow European mechanics and which follow American. Show every step's computation, the source amount, the destination, and the cumulative-to-date balance for each tier

6. **Apply preferred-return and catch-up arithmetic with the right convention.** Preferred-return rate: confirm the LPA-defined rate and the accrual convention (simple vs. compounded; ACT/360 vs. ACT/365 vs. 30/360); compute the per-LP preferred-return-to-date and the unaccrued-preferred-return; confirm that the cycle's preferred-return distribution clears the unpaid-preferred-return inventory before any catch-up begins. Catch-up: confirm the catch-up percentage (commonly 100% catch-up until GP achieves the carry-percentage participation; sometimes 80/20 partial catch-up); compute the catch-up amount and the carry split that follows; if the GP catch-up is not 100%, document the rationale and the LPA section

7. **Test the clawback (interim or fund-term).** Compute the cumulative carry distributed to GP through this cycle and the cumulative preferred-return / return-of-capital owed to LPs; if the carry-to-date exceeds the LPA cap on a what-if-fund-terminated basis, compute the clawback amount and reconcile to the GP-clawback hold-back / escrow balance; apply the after-tax-clawback convention if the LPA so provides (assume highest combined federal / state / local marginal rate per the LPA, less any tax-distribution true-up); document any LPAC consultation trigger

8. **Apply recall, recycling, and reinvestment provisions correctly.** Recall: confirm the recall window has not expired; confirm the recall-cap (typically a percentage of committed-capital or distributions) has not been exhausted; confirm the recallable-amount feeds back to LP unfunded; document the LPAC-notification posture. Recycling: confirm reinvestment of short-cycle realizations within the LPA-defined window does not increase per-LP commitment but does refill recallable; track the recycling cap separately from the recall cap. Default cascade: for any LP that fails to fund a call by the due date plus grace period, apply the LPA-defined cascade (interest-and-penalty / forfeit / sale-to-other-LP / drag-along-to-non-defaulting-LPs); document LPAC notification, cure-opportunity letter, and the cure-or-forfeit decision memo

9. **Apply fee-offsets and management-fee step-down arithmetic.** Fee-offset: aggregate the period's transaction / monitoring / portfolio-company / break-up / director / consulting / sourcing fees received by the GP or related party; apply the LPA-defined offset percentage (commonly 80–100%); apply the offset against the management-fee accrual; confirm the offset is reflected per-LP. Management-fee step-down: confirm the cycle is at or past the LPA-defined step-down date (commonly end of investment period); apply the stepped-down rate and the stepped-down base (often invested-capital rather than committed-capital); confirm the cycle's management-fee accrual aligns

10. **Build the per-LP capital-account roll-forward and the ILPA notice feed.** Per-LP opening capital + capital called + GP commitment + allocated income / loss + distributions − management fee − carry − recall-back + recycle = closing capital. Per-LP commitment / called-to-date / unfunded / recallable / expired-recallable / distributions-to-date / DPI / RVPI / TVPI / IRR / MOIC / PIC. Format the per-LP notice using the ILPA Capital Call Notice template fields (or the firm's conformed equivalent) for a call cycle, or the ILPA Distribution Notice template fields (or conformed equivalent) for a distribution cycle. Apply withholding (FATCA / CRS / chapter-3 / state-pass-through) per LP jurisdiction. Format the GP-side cycle summary and the LPAC-side aggregate feed

11. **Document the payment-rail and agentic-payment posture.** Per-LP wire instruction (correspondent bank / SWIFT / FedWire / SEPA / FX) tied to the side-letter / subscription-document instruction-of-record; verify any change-of-instruction has the required out-of-band confirmation per the firm's wire-fraud control. For any leg routed through an agentic-payment surface (MCP-treasury / AP2 / ACP): verify the mandate-scope discipline (merchant-bound / amount-bound / time-bound / single-use posture per `agentic-payments-kya-governance.md`), the KYA verification artifact for the operating agent, the decision-execution separation, and the audit-trail capture. Document any over-mandate or under-mandate event for follow-up. Confirm the four-eyes posture on any wire above the firm's wire-threshold

12. **Build the exception log, the four-eyes sign-off matrix, and the regulator-deliverable feed.** Exception log: every variance against threshold, every override, every late-funded LP, every defaulted LP, every clawback-trigger event, every recall / recycling election, every LPA-vs-side-letter conflict, every late-arriving subscription document, every payment-rail break, every agentic-payment mandate-scope event. Four-eyes sign-off: compute / review / approve / counter-sign per the cycle (CFO / COO / fund-controller / valuation-committee / GP-principal / auditor-coordination); LPAC consultation trigger evidenced where applicable. Regulator-deliverable feed: Form PF section update (private-fund advisers), AIFMD Annex IV update (EU / UK funds), SEC custody-rule confirmation, FATCA / CRS / withholding-agent reporting, ILPA-template investor-side feed. Save the package to `outputs/` if confirmed; archive in the immutable cycle record per the firm's retention policy (SEC Rule 204-2 / 17a-4 / equivalent)

**Output Templates (audience-specific):**

- **Capital Call Notice Cycle Pack** — Cycle scope memo / per-LP capital-call notice (ILPA-conformed) / payment-rail and KYA / mandate posture / commitment-ledger reconciliation / default-cascade pre-positioning / exception log / four-eyes sign-off
- **Distribution Cycle Pack** — Cycle scope memo / per-LP distribution notice (ILPA-conformed) / waterfall walk with stated arithmetic per tier / clawback test result / recall and recycling election / withholding posture / payment-rail and KYA / mandate posture / exception log / four-eyes sign-off
- **Continuation-Vehicle / Secondaries / GP-Led Restructuring Waterfall Audit** — Rollover-versus-cash election mechanics / status-quo-versus-new-fund LP-preference matrix / LPAC-disclosure posture / conflict-of-interest disclosure / valuation-committee Level-3 package alignment / SEC Rule 206(4)-7 compliance posture
- **Manager-Side Shadow Waterfall Audit (vs. Third-Party Administrator)** — Independent per-LP computation against the administrator's file / variance log per category (commitment-ledger / preferred-return / catch-up / carry / fee-offset / clawback / recall / equalization) / threshold-breach escalation / administrator-dispute resolution log / vendor-oversight scorecard / SOC-1 carve-out and complementary-user-controls inventory
- **Fund-Term Clawback Memo and LPAC-Communication Pack** — Carry-to-date and preferred-return / return-of-capital cumulative tables / what-if-fund-terminated walk / after-tax-clawback computation / GP-clawback hold-back and escrow reconciliation / LPAC consultation memo and resolution-of-clawback letter / investor-communication letter / SEC-disclosure trigger review
- **Default-LP Cycle Pack** — Default-event documentation / grace-period evidence / LPAC-notification mechanics / default-cascade walk (interest-and-penalty / forfeit / sale-to-other-LP / drag-along) / cure-or-forfeit-decision memo / re-cut waterfall after the default event / regulator-deliverable update

**Output requirements:**

- Every per-LP allocation traces to commitment-ledger, LPA section, and side-letter clause (where applicable)
- Every waterfall tier shows the source amount, the destination, the rate / percentage applied, the cumulative-to-date, and the LPA citation
- Every preferred-return accrual states the rate, the accrual convention (simple / compounded; day-count), and the basis (committed-capital / called-capital / invested-capital)
- Every clawback test states the carry-to-date, the preferred-return / return-of-capital owed, the after-tax convention, and the GP-clawback hold-back balance
- Every recall and recycling event states the cap, the to-date utilization, and the impact on per-LP unfunded
- Every default-LP event states the grace-period, the LPAC notification, the cascade applied, and the cure-or-forfeit decision
- Every wire instruction confirms the instruction-of-record and the change-of-instruction out-of-band protocol (where applicable)
- Every agentic-payment leg confirms the mandate scope, the KYA verification artifact, the decision-execution separation, and the audit-trail capture
- Four-eyes sign-off matrix evidenced with names / roles / timestamps; LPAC consultation evidenced where triggered
- ILPA Capital Call / Distribution Notice template (or firm-conformed equivalent) used for investor-facing notices; ILPA Reporting Template alignment for the per-LP capital-account statement
- Exception log carries category, magnitude, aging, owner, evidence reference, disposition
- Saved to `outputs/` if the user confirms; archived per the firm's record-retention policy (SEC Rule 204-2 / 17a-4 / equivalent)

## Regulatory & Compliance Layer

- **Investment Company Act of 1940 / Advisers Act** — for U.S. registered funds and registered advisers; valuation rules under Section 2(a)(41); Rule 2a-5 board valuation framework; Advisers Act fiduciary duty; Rule 206(4)-7 compliance program; Rule 206(4)-8 anti-fraud for pooled investment vehicles
- **SEC Custody Rule (Rule 206(4)-2)** — qualified-custodian framework, surprise-examination, and pooled-vehicle audit framework that the call / distribution mechanics must align with
- **SEC Marketing Rule 206(4)-1** — performance presentation framing for any investor-marketing material derived from the per-LP IRR / MOIC / DPI / RVPI / TVPI
- **SEC Rule 204-2 / 17a-4** — books-and-records retention applied to capital-call / distribution notices, commitment ledger, side letters, LPAC consultations
- **SEC Private Fund Advisers Rule** — quarterly statement and audit framework where applicable (subject to the August 2024 Fifth Circuit vacatur and any subsequent SEC re-promulgation); ILPA-template alignment as best-practice
- **Form PF (private-fund advisers)** — section feeds for liquidity, leverage, counterparty exposure, performance, beneficial ownership, master-feeder mapping; event-driven reporting where applicable
- **AIFMD Annex IV** — EU / UK alternative-investment-fund-manager reporting (asset-class exposure, leverage, market-risk, liquidity-profile) where applicable
- **ILPA Reporting Standards / ILPA Capital Call & Distribution Notice templates (2.0)** — investor-facing notice format and per-LP capital-account statement standardization
- **ASC 820 / IFRS 13 fair-value hierarchy** — Level 1 / 2 / 3 classification, observable-input versus unobservable-input documentation; relevant for the underlying-investment realization that drives a distribution
- **ASC 946 investment-company accounting** — investment-company-specific GAAP; partner-capital roll-forward; financial highlights
- **AICPA Audit Guide for Investment Companies** — auditor-testing alignment; SOC-1 carve-out vs. inclusive convention
- **FATCA / CRS / Chapter 3 / Chapter 4 withholding** — per-LP withholding agent posture and reporting on distributions
- **State pass-through entity tax (PTET) elections** — per-LP allocation impact in U.S. states that have enacted PTET
- **Tax-distribution gross-up (LPA-defined)** — per-LP tax-distribution computation, true-up against actual K-1 / K-3 allocations, recall-of-tax-distribution mechanics where applicable
- **FFIEC SR 11-7 model-risk management** — where the call / distribution cycle is computed with a vendor or internal model (Allvue / Cascade Suite / Carta / qashqade / Chronograph / 6lock / proprietary): model-inventory entry, validation evidence, ongoing-monitoring posture, change-control
- **Wire-fraud / payment-rail control** — instruction-of-record convention, change-of-instruction out-of-band confirmation, four-eyes posture above the firm's wire threshold; FFIEC handbook reference
- **Agentic-payment governance (where applicable)** — KYA verification for the operating agent, mandate-scope discipline (merchant-bound / amount-bound / time-bound / single-use), decision-execution separation, audit-trail capture per the firm's `agentic-payments-kya-governance.md` posture
- **SOC-1 Type II carve-out vs. inclusive** — administrator / payment-rail provider / third-party computation platform SOC-1 inventory, complementary-user-controls inventory, key-vendor SLA review
- **LPA controlling document / side-letter MFN matrix** — every per-LP override traces to a side-letter clause; MFN-election cycle adhered to

## Personalization Hooks

The following `config.yml` keys customize this skill:

- `fund.identity` — fund family roster, vehicle structure, domicile, regulator, vintage, fund-term, base currency
- `fund.waterfall_structure` — European whole-fund vs. American deal-by-deal vs. hybrid; preferred-return rate, accrual convention (simple / compounded, day-count); catch-up percentage and structure; carry split; clawback (interim / fund-term / after-tax); GP-clawback hold-back percentage; recall and recycling caps; fee-offset percentage and scope
- `fund.commitment_ledger_source` — administrator file / internal-system reference; LP-roster source of truth; subscription-document repository
- `fund.side_letter_inventory` — per-LP MFN matrix and per-LP special-arrangement record (fee discount, opt-out, co-investment priority, reporting frequency, transfer restriction, key-person provision, most-favored-investor election)
- `fund.default_cascade_sequence` — grace period, interest-and-penalty rate, LPAC-notification mechanics, forfeit / sale-to-other-LP / drag-along provisions
- `fund.equalization_method` — interest-on-prior-capital / actualized-cost / hybrid; subsequent-closing convention
- `fund.fee_offset_scope` — transaction / monitoring / portfolio-company / break-up / director / consulting / sourcing fees in scope; offset percentage; per-LP application
- `fund.management_fee_stepdown` — step-down date, stepped-down rate, stepped-down base (commitment vs. invested capital)
- `vendor.computation_platform` — Allvue / Cascade Suite / Carta / qashqade / Chronograph / 6lock / proprietary; SOC-1 reference; model-inventory entry; validation evidence reference
- `vendor.payment_rail` — correspondent bank / SWIFT / FedWire / SEPA / FX provider roster; out-of-band confirmation control
- `vendor.agentic_payment_surface` — AP2 / ACP / MCP-treasury surface if any; KYA verification artifact reference; mandate-template inventory
- `compliance.regulator_deliverables` — Form PF / AIFMD Annex IV / SEC custody-rule / ILPA / FATCA-CRS-withholding feed selection
- `compliance.lpac_consultation_triggers` — clawback / continuation-vehicle / GP-led restructuring / default / fee-offset / valuation / conflict-of-interest triggers
- `compliance.four_eyes_threshold` — wire amount above which four-eyes posture is required; LPAC consultation threshold
- `firm.record_retention_policy` — SEC Rule 204-2 / 17a-4 / equivalent retention period and immutable-archive convention

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]

## Handoff Contracts

**Inbound:**

- `skills/operations/fund-administration-nav-reviewer.md` — certified NAV, per-investment fair value, partner-capital roll-forward, accrued fees / carry that this cycle consumes
- `skills/operations/financial-model-documenter.md` — model documentation reference for any waterfall-computation model used (Allvue / Cascade Suite / Carta / qashqade / Chronograph / 6lock / proprietary)
- `skills/operations/stress-test-scenario-modeler.md` — stress-distribution computation where a distribution is conditional on a realization scenario or where a clawback test is run on a stress NAV
- `skills/operations/loan-covenant-compliance-monitor.md` — for any portfolio company whose distribution is conditional on covenant compliance (PE-fund context)

**Outbound:**

- `skills/admin/regulatory-filing-checker.md` — Form PF / AIFMD Annex IV / SEC custody-rule / FATCA-CRS-withholding feeds that consume the certified cycle
- `skills/admin/ai-controls-auditor-icfr.md` — SOC-1 / ICFR controls evidence (computation-control, four-eyes, exception-log review, vendor-oversight)
- `skills/customer-service/client-portfolio-update.md` — investor-facing capital-account statement and per-LP IRR / MOIC / DPI / RVPI / TVPI feed
- `skills/customer-service/financial-plan-constructor.md` — high-net-worth LP-side planning consumption of the per-LP cycle (where the LP is an HNW family-office relationship)
- `skills/_shared/email-drafter.md` — investor-notice transmittal, LPAC consultation letter, clawback letter, default-LP letter, change-of-instruction confirmation letter
- `skills/admin/sanctions-aml-alert-reviewer.md` — pre-wire screening of any new payee or change-of-instruction
- `skills/admin/aml-case-triage-evidence-dossier.md` — case-queue triage for any cycle that triggers an AML / SAR escalation
