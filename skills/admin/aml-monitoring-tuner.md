---
name: "AML Transaction-Monitoring Tuner & False-Positive-Reduction Memo"
category: admin
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~6 hr/tuning cycle"
version: 1.0
last_eval_score: null
---

# 📘 AML Transaction-Monitoring Tuner & False-Positive-Reduction Memo

## Purpose

Produce a BSA-officer-grade transaction-monitoring tuning memo for a bank, broker-dealer, money-services business, or fintech / crypto VASP. Output is a structured calibration deliverable — Coverage Map (typology → rule / model coverage), Threshold Calibration (per rule with above-the-line / below-the-line testing and false-positive / true-positive yield), Segmentation Refresh (customer-risk and behavior segments), Scenario / Typology Refresh (with regulatory-advisory and enforcement-action source mapping), Model-Validation and Tuning Backtest, Governance Memo (BSA officer / model-risk / audit / regulator), and Implementation Plan with rollout, parallel-run, and reversion criteria — designed to be reviewed by the BSA officer, model-risk management, internal audit, and the regulator. Differs from the Sanctions / AML Alert Reviewer (which dispositions individual alerts and drafts SAR / OFAC reports): this skill calibrates the monitoring system itself.

## When to Use

Use this skill whenever you need to:
- Run the periodic (typically annual or semi-annual) tuning cycle for the BSA / AML transaction-monitoring system
- Respond to an audit, regulatory examination, or consent-order finding that requires monitoring-system calibration evidence
- Onboard a new product, channel, geography, or customer segment where existing rules / models may not cover the new typology
- Refresh typology coverage in response to a FinCEN advisory (FIN-2023-Alert / FIN-2024-Alert / etc.), an OFAC advisory, an FATF mutual-evaluation finding, an FFIEC examination-manual update, or a public enforcement action
- Reduce alert volume and false-positive rate against a documented business case (operational cost / examiner expectation / risk-coverage trade-off)
- Tune segmentation when a population-drift or model-drift trigger fires
- Build the model-risk-management documentation required by SR 11-7 (FRB / OCC) or NY DFS Part 504 for a covered institution
- Document a typology-coverage gap closure (where a SAR should have fired but didn't, identified through look-back testing)

## Required Input

Provide the following:

1. **Institution profile** — Charter type (national bank / state bank / credit union / broker-dealer / MSB / payments fintech / crypto exchange / VASP), regulator(s) (FRB / OCC / FDIC / NCUA / FinCEN / SEC / FINRA / NY DFS / state regulator / FATF jurisdiction), customer base (retail / commercial / private wealth / institutional / correspondent / cross-border), product mix (deposit / lending / cards / payments / wires / FX / brokerage / crypto), geographic footprint, BSA / AML risk-assessment summary
2. **Current monitoring system** — Vendor (Actimize / SAS / Verafin / Quantexa / Hawk / Napier / ComplyAdvantage / in-house / hybrid), rule and scenario inventory (per typology, with current threshold(s) and segmentation), model inventory (supervised / unsupervised / network / graph), recent alert volume by rule per period, alert-disposition outcomes (false-positive / closed / escalated to SAR / SAR-filed / continuing-activity report)
3. **Typology coverage map** — Per typology (structuring / smurfing / cash-intensive-business misuse / trade-based ML / shell / nesting / layering / round-tripping / mule / scam-money / human-trafficking / drug / fentanyl / sanctions evasion / terrorist financing / proliferation financing / corruption / tax-evasion / hawala / virtual-asset-laundering / nested correspondent / NFT-laundering / OFAC-50%-rule / sanctions-evasion-via-shell), which rule(s) / model(s) cover it, and the typology-source citation (FATF / FinCEN advisory / OFAC advisory / FFIEC manual / FinCEN GTO / public enforcement action)
4. **Population data** — Customer-segment roster (retail consumer / small business / commercial / private wealth / institutional / cash-intensive / MSB-customer / NBFI-customer / PEP / high-risk-jurisdiction / high-risk-product), per-segment population, transactional baseline (median / 95th-percentile / max for the segment per product type), recent-period alert volume by segment
5. **Threshold-calibration testing data** — Above-the-line testing population (random-sample of alerts at and above current threshold; sized for statistical confidence), below-the-line testing population (random-sample of activity just below current threshold; sized for statistical confidence with known-risk-attribute oversampling), per-sample disposition labels (true positive / false positive)
6. **Productivity metrics** — Investigator-hours per alert by category, escalation-rate to QC, QC override rate, SAR-filing rate per investigator, look-back / re-review activity volume
7. **Recent typology signal** — Last-24-month FinCEN advisories, OFAC advisories, FATF jurisdictional updates, NY DFS Part 504 letter, FFIEC examination-manual-update, public enforcement actions naming similar institutions, peer-institution publicly-disclosed monitoring failures
8. **Model-risk governance** — Current model-risk-management inventory entry (model ID, owner, validator, last-validation date, validation-outcome, ongoing-monitoring metrics), model-tier classification, SR 11-7 / NY DFS Part 504 document reference
9. **Examiner expectation context** — Recent examiner feedback (examination report, MRA, MRIA, consent order), any open audit issues, any open NY DFS Part 504 certification gaps, any 314(a) / 314(b) information-sharing patterns
10. **Business and operational constraints** — Acceptable alert-volume change envelope (per rule and overall), required parallel-run period, change-management calendar, required regulator-notification convention, acceptable SAR-yield change envelope
11. **Compliance & legal references** — BSA / AML statute and regulation, OFAC framework, USA PATRIOT §314(a) / §314(b), FFIEC BSA / AML Examination Manual, NY DFS Part 504, SR 11-7 model-risk guidance, FinCEN AML Program Effectiveness final rule (when finalized) and the April 2026 program-effectiveness proposal direction, FinCEN AML / CFT Priorities

## Instructions

You are a finance professional's AI assistant specializing in BSA / AML compliance and model risk. Your job is to produce a tuning memo that the BSA officer can sign, model-risk management can validate, internal audit can test, and the regulator can examine without finding ambiguity. Every threshold change, every segmentation change, every typology addition, every model retraining is documented with rationale, evidence (above-the-line / below-the-line testing), forecast impact, parallel-run plan, and reversion criterion.

**Before you start:**
- Load `config.yml` for institution conventions (BSA-officer roster, model-risk-officer roster, audit liaison, regulator-of-record, examination calendar, vendor of record, parallel-run convention, change-management calendar, document-control conventions)
- Reference `knowledge-base/regulations/` for the BSA / AML / OFAC / FFIEC / SR 11-7 / NY DFS Part 504 framework and the FinCEN April 2026 program-effectiveness direction (controls evaluated on demonstrable effectiveness, not control-presence)
- Reference `knowledge-base/terminology/` for tuning-cycle terms (above-the-line / below-the-line testing, productivity metrics, true-positive / false-positive yield, recall, precision, segmentation, typology, scenario, rule, model, model risk, model tier, parallel run, reversion criterion)
- Cross-check with `skills/admin/sanctions-aml-alert-reviewer.md` for the per-alert disposition workflow that the tuning calibrates against
- Cross-check with `skills/admin/regulatory-filing-checker.md` for the SAR / OFAC report deliverables that the monitoring system feeds
- Cross-check with `skills/admin/ai-controls-auditor-icfr.md` for the model-risk-management governance evidence that overlaps
- Anti-plagiarism: every paragraph composed from the institution's own data and policies; no verbatim lift from vendor marketing (Hawk / Napier / Quantexa / Sumsub / Sardine / Cleareye / Silent Eight / Nexiant / Flagright / etc.) or from regulatory-summary sources. Regulatory references cite the rule and section, not third-party summaries

**Process:**

1. **Confirm scope, drivers, and approval architecture.** Restate the tuning cycle (annual / semi-annual / event-driven), the trigger(s) (calendar / regulatory advisory / examiner finding / new product / drift / SAR-yield change), the deliverable scope (rule-level / model-level / segmentation / typology coverage / governance memo), the approval architecture (BSA officer / model-risk / audit / regulator-notification), and the parallel-run / reversion convention

2. **Build the typology coverage map.** Per typology (per current FATF / FinCEN / OFAC / FFIEC / NY DFS source list), document which rule / model / scenario covers it and the source-citation. Identify coverage gaps explicitly. Cross-check with the institution's BSA / AML risk assessment to ensure that all elevated-risk typologies have at least one detection mechanism, and document the rationale for any deferred coverage. Source-tag every typology to the controlling regulatory advisory or enforcement action

3. **Refresh customer and behavior segmentation.** For each rule, document the segmentation in use (customer risk-tier / product / channel / geography / behavioral cohort). Test for population drift: has the underlying customer population shifted such that the segmentation no longer reflects the risk distribution? Test for behavior drift: has typical activity within a segment shifted such that thresholds calibrated last cycle no longer reflect normal behavior? Recommend segmentation refinement (split a segment / merge segments / add a behavioral cohort / add a peer-group benchmark) with the evidence

4. **Run above-the-line and below-the-line threshold testing.** For each rule under tuning: pull a random-sample of recent alerts at and above the current threshold (above-the-line); pull a random-sample of activity just below the current threshold, with known-risk-attribute oversampling for statistical power (below-the-line). Compute per sample: true-positive yield (alerts leading to escalation / SAR), false-positive yield, productivity-per-investigator-hour, regulator-significance markers (314(a) match / SAR-narrative-quality / look-back exposure). Plot the threshold-yield curve. Document the recommended threshold (raise / lower / unchanged) with the projected change to alert volume, false-positive rate, true-positive recall, and SAR yield

5. **Refresh scenario and typology library.** Add scenarios driven by recent FinCEN advisories (e.g., FIN advisory series), OFAC advisories, FATF mutual-evaluation findings, NY DFS Part 504 examiner letters, and public enforcement actions. Decommission scenarios that consistently produce zero true positives over a multi-cycle window with documented rationale. For new scenarios, document the typology, the source advisory, the proposed rule logic / model approach, the testing plan, and the parallel-run period

6. **Apply model-risk-management discipline (SR 11-7 / NY DFS Part 504).** For each model touched: document model ID, owner, validator, model tier (Tier 1 high-impact / Tier 2 medium / Tier 3 low), last validation, validation-outcome, ongoing-monitoring metrics (population stability index / characteristic-stability index / drift indicator / performance metrics over rolling window). Document any retraining (training-data window, feature changes, hyperparameter changes, validation-test results, champion-challenger comparison). Apply SR 11-7's three-pillar framework (model development / implementation / use; ongoing monitoring; validation); for NY DFS Part 504 institutions, apply the additional certification framework

7. **Forecast impact and parallel-run plan.** For each proposed change, forecast: alert-volume change (per rule and overall); false-positive-rate change; investigator-hour change; SAR-yield change; coverage delta against the typology map. Define the parallel-run period (typically 30–90 days for threshold changes; 90–180 days for new models; longer for high-tier models), the parallel-run success criteria (alert-volume convergence within envelope, no missed SAR-quality alerts in below-the-line look-back), and the reversion criterion (if metrics drift outside envelope, revert and document)

8. **Build the governance memo.** Sections: executive summary; tuning-cycle drivers; methodology and source-data; coverage map; segmentation refresh; rule-level threshold changes (with above-the-line / below-the-line evidence); model retraining (with SR 11-7 evidence); scenario / typology additions and decommissions; forecast impact; parallel-run and reversion plan; approval matrix (BSA officer / model-risk / audit / regulator-notification); appendix with evidence packs

9. **Document examiner-readiness alignment.** Per FFIEC BSA / AML Examination Manual: tie the memo to examination procedures (risk assessment, monitoring system, customer due diligence, suspicious activity monitoring and reporting, OFAC). Per NY DFS Part 504: tie to certification-of-compliance evidence. Per FinCEN April 2026 program-effectiveness direction: document the demonstrable-effectiveness evidence (investigator-hours saved / SAR-quality-improvement / typology-coverage-improvement / risk-coverage-against-risk-assessment) — not just the control-presence

10. **Define implementation plan and change-management calendar.** Sequence: shadow / parallel run / cutover / post-cutover monitoring / formal close. Include rollback steps, owner-approver matrix, regulator-notification trigger (if any), and post-implementation review schedule (typically 30 / 60 / 90 days post-cutover with metric checkpoints)

11. **Produce the SAR-yield and false-positive trend report.** Rolling 13-cycle (or 13-month) trend per rule and overall: alert volume; alert volume per million dollars of activity; false-positive rate; true-positive rate; SAR-filed rate; escalation-to-SAR ratio; productivity per investigator-hour; look-back / continuing-activity / restitution review activity. Compare to peer benchmarks if available (without disclosing peer data)

12. **Final controls pass and sign-off.** Owner-approver matrix: who computed, who reviewed, who approved. Document any dissent. Confirm legal-privilege framing (the tuning memo is typically work-product under attorney-client / work-product privilege; the BSA-officer-of-record signs but legal counsel is in the chain). Save to `outputs/` if confirmed; archive in the immutable BSA-AML governance record per the firm's retention policy (typically the longer of the BSA five-year retention or the institution's internal retention)

**Output Templates (audience-specific):**

- **Annual Tuning Cycle Memo (BSA Officer Sign-Off)** — Full deliverable above with executive summary, methodology, coverage map, threshold changes, model retraining, scenario library refresh, forecast impact, parallel-run plan, examiner-readiness alignment, approval matrix
- **Event-Driven Threshold-Recalibration Memo** — Trigger-specific (regulatory advisory / enforcement action / look-back finding); narrower scope; same evidence rigor; faster cycle
- **New-Product / Channel / Geography Coverage-Gap-Closure Memo** — Coverage-gap inventory for the new product / channel / geography; proposed scenario / model / rule additions; parallel-run plan; staged-rollout calendar
- **Model-Validation Pack (SR 11-7 / NY DFS Part 504)** — Model ID, tier, owner / validator, training-data, feature documentation, hyperparameter justification, performance metrics on hold-out, ongoing-monitoring plan, champion-challenger comparison, validation-outcome and recommendation
- **Look-Back / Look-Forward Recalibration Memo** — Where a look-back identified missed-SAR-quality activity; documents the gap, the root cause (threshold / segmentation / typology / model drift), the remediation, and the look-forward trigger if any
- **Examiner Response Memo (MRA / MRIA / Consent-Order)** — Examiner-finding-by-finding remediation, with above-the-line / below-the-line evidence, parallel-run plan, certification language draft, regulator-notification draft
- **NY DFS Part 504 Annual Certification Pack** — Per-element evidence (transaction monitoring + watch-list filtering): risk assessment alignment, governance, design and implementation, ongoing monitoring; certification-officer sign-off draft
- **Model Decommissioning Memo** — For a scenario / rule / model being retired: rationale, multi-cycle zero-true-positive evidence, coverage transfer plan to remaining rules / models, regulator-notification consideration

**Output requirements:**
- Every threshold change has above-the-line and below-the-line testing evidence with sample size and confidence
- Every model change has SR 11-7 / NY DFS Part 504 governance evidence (owner, validator, validation outcome, ongoing-monitoring plan)
- Every scenario / typology change ties to a regulatory advisory, enforcement action, or risk-assessment update
- Every forecast change projects alert volume, false-positive rate, true-positive recall, and SAR yield
- Every change carries a parallel-run plan and a reversion criterion
- The governance memo aligns to FFIEC examination procedures, NY DFS Part 504 certification, and FinCEN program-effectiveness direction
- Privilege framing applied (work-product / attorney-client) where appropriate
- Saved to `outputs/` if the user confirms; archived per the firm's BSA-AML retention policy

## Regulatory & Compliance Layer

- **Bank Secrecy Act / 31 USC 5311 et seq.** — base statute for the AML monitoring obligation
- **31 CFR Chapter X** — FinCEN rules across institution types (banks 1020, broker-dealers 1023, MSBs 1022, mutual funds 1024, casinos 1021, insurance 1025, futures 1026, dealers in precious metals 1027, loan / finance 1029, housing-GSE 1030)
- **Suspicious Activity Reporting (SAR)** — 31 CFR 1020.320 / 1023.320 / 1024.320 / 1022.320 / 1025.320 / 1026.320; tipping-off prohibition (31 USC 5318(g)(2)); SAR-information-disclosure restrictions (31 CFR 1020.320(e))
- **Currency Transaction Reports (CTR)** — 31 CFR 1010.311
- **OFAC** — 31 CFR Part 501; 50%-rule; sectoral lists; secondary sanctions
- **USA PATRIOT §314(a) / §314(b)** — information-sharing safe harbor; subject-tracking and outbound-request mechanics
- **FFIEC BSA / AML Examination Manual** — examination procedures (risk assessment, monitoring system, customer due diligence, suspicious activity monitoring and reporting, OFAC, beneficial ownership, USA PATRIOT)
- **FinCEN advisories and Geographic Targeting Orders** — typology-source citations
- **NY DFS Part 504** — transaction-monitoring and watch-list filtering certification framework with annual board / senior-officer certification
- **SR 11-7 (FRB / OCC) Model Risk Management** — three-pillar framework (development / implementation / use; ongoing monitoring; validation); model-tier classification; champion-challenger discipline
- **Federal Reserve SR 21-8 / OCC equivalents** — model risk for AI / ML models specifically
- **FinCEN AML Program Effectiveness final rule (when finalized) / April 2026 proposal direction** — controls evaluated on demonstrable effectiveness (investigator-hours saved / SAR-quality / typology-coverage / risk-coverage), not on control-presence; this shifts the tuning-memo evidence emphasis from "we have a rule" to "we have evidence the rule is producing risk-relevant outcomes"
- **FinCEN AML / CFT Priorities** — corruption, cybercrime / CSAM, foreign and domestic terrorist financing, fraud, transnational criminal organization, drug trafficking, human trafficking and human smuggling, proliferation financing
- **FATF Recommendations and Mutual Evaluation findings** — typology-source citations and jurisdictional-risk-tiering inputs
- **Privilege framework** — attorney-client / work-product privilege for the tuning memo and the supporting testing; legal-counsel-of-record in the approval chain
- **Record retention** — BSA five-year retention (31 CFR 1010.430); SR 11-7 model-development and validation records aligned with model lifecycle

## Personalization Hooks

The following `config.yml` keys customize this skill:

- `institution.charter` — national bank / state bank / credit union / broker-dealer / MSB / payments fintech / crypto exchange / VASP
- `institution.regulators` — FRB / OCC / FDIC / NCUA / FinCEN / SEC / FINRA / NY DFS / state regulator / FATF jurisdiction roster
- `institution.bsa_officer` — BSA officer of record and deputies
- `institution.risk_assessment_reference` — pointer to current BSA / AML risk assessment
- `monitoring.vendor` — vendor of record (Actimize / SAS / Verafin / Quantexa / Hawk / Napier / ComplyAdvantage / in-house / hybrid)
- `monitoring.rule_inventory` — pointer to the current rule and scenario inventory with owners and last-tuning dates
- `monitoring.model_inventory` — pointer to the current model inventory with model IDs, tiers, owners, validators
- `monitoring.tuning_cycle_calendar` — annual / semi-annual / event-driven cadence
- `monitoring.parallel_run_convention` — default parallel-run period per change-type, reversion-criterion convention
- `governance.audit_liaison` — internal-audit lead and external-audit firm of record
- `governance.model_risk_officer` — model-risk officer of record
- `governance.legal_counsel` — BSA / AML counsel of record (in the privilege chain)
- `governance.examination_calendar` — current and upcoming examination calendar with regulator and scope

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]

## Handoff Contracts

**Inbound:**
- `skills/admin/sanctions-aml-alert-reviewer.md` — per-alert disposition outcomes that feed the above-the-line testing population and the productivity metrics
- `skills/admin/regulatory-filing-checker.md` — SAR / CTR / OFAC report content and frequency that feed the SAR-yield trend
- `skills/admin/ai-controls-auditor-icfr.md` — overlapping model-risk-management governance evidence
- `skills/admin/kyc-cip-onboarding-workflow.md` — customer-risk-tier and segmentation feed
- `skills/operations/trade-lifecycle-tracker.md` — trade-and-transaction activity feed for monitoring
- `skills/operations/stress-test-scenario-modeler.md` — scenario / typology framing for stressed-condition monitoring

**Outbound:**
- `skills/admin/regulatory-filing-checker.md` — examiner response, NY DFS Part 504 certification, MRA / MRIA / consent-order remediation memos
- `skills/admin/sanctions-aml-alert-reviewer.md` — refreshed rule / model / segmentation / typology coverage that the alert reviewer uses going forward
- `skills/_shared/email-drafter.md` — examiner-correspondence, vendor-change-control, internal-stakeholder-communication drafts
- `skills/_shared/meeting-summarizer.md` — model-risk-committee minutes, BSA-officer review minutes, audit-committee minutes capture
