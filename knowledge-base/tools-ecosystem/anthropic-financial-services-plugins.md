# Anthropic Financial Services Plugins — Landscape Reference

## What it is

In Q1 2026, Anthropic shipped an official plugin suite for Claude targeted at finance professionals. The suite is organized around a shared `financial-analysis` core plugin plus four function-specific add-ons: `investment-banking`, `equity-research`, `private-equity`, and `wealth-management`. Partners (LSEG, S&P Global) contribute data-provider plugins that plug into the same surface. At launch the suite ships roughly forty skills and a few dozen slash commands covering the canonical workflows in each vertical.

The capability set is useful as a reference landscape because it is the first broadly published, vendor-curated map of "what finance AI skills should cover" — and it directly informs where the KRASA finance repo has coverage, overlap, and gaps.

## Function-area coverage (high level)

**Shared / modeling layer.** Comps analysis, DCF, LBO, 3-statement models, and output-quality checks on decks and documents. Emphasis is on producing auditable financial artifacts rather than chat output.

**Investment banking.** Deal-material drafting (CIM, teaser, process letter), buyer-list construction, merger modeling, strip profiles, and deal-milestone tracking.

**Equity research.** Initiating-coverage reports, earnings previews and post-earnings updates, thesis tracking, morning notes, and idea screens.

**Private equity.** Deal sourcing and screening, diligence checklists, unit-economics analysis, IC memos, and portfolio-company KPI monitoring.

**Wealth management.** Client-meeting prep, financial-plan construction, portfolio rebalancing (tax-aware, wash-sale-aware, lot-level), client reporting, and tax-loss harvesting.

**Data integrations.** LSEG adds bond pricing, yield-curve analysis, FX carry, option valuation, and macro dashboards. S&P Global adds company tearsheets, earnings previews, and funding digests sourced from Capital IQ.

## Implications for the KRASA finance repo

**Coverage KRASA already has (after this landscape run):**
- Investment memo drafter (overlaps IC memo)
- Earnings call summarizer (overlaps post-earnings update)
- Financial model documenter (overlaps model-audit patterns)
- Budget variance analyzer, stress-test modeler (FP&A-side workflows not heavily represented in the Anthropic plugin set)
- Advisor meeting prep, client portfolio update (wealth-management-adjacent)
- Regulatory filing checker (compliance)
- Market research brief (adjacent to sector overviews / initiating coverage)

**Coverage added by this run to close gaps:**
- DCF Valuation Builder (operations)
- Comparable Company Analysis (operations)
- 13-Week Cash Flow Forecaster (operations / treasury — not directly in the Anthropic set; surfaced by broader landscape evidence of treasury agent adoption)
- Tax-Loss Harvesting Identifier (customer-service / wealth management)

**Notable remaining gaps (candidates for future runs):**
- LBO model builder / returns analysis
- 3-statement model construction and update
- Merger / accretion-dilution model
- Deal sourcing and buyer list construction
- Due diligence checklist generator (PE diligence workflow)
- Morning note / catalyst calendar (sell-side equity research)
- Portfolio rebalancing with tax and lot-level awareness
- Financial-plan construction (WM)
- Pitch-book / deal-material QC

## How this should inform skill design

Three design patterns are worth adopting across KRASA finance skills:

1. **Auditable artifacts, not chat.** The Anthropic plugins emphasize producing decks, memos, and models that can be reviewed and signed off. KRASA skills should continue to require structured outputs with explicit sections, numbers that reconcile, and source references.

2. **Composable primitives.** The "core + function" pattern implies that shared tooling (comps, DCF, model QC) is reused across verticals. KRASA should keep modeling skills generic enough to be invoked from vertical-specific workflows (e.g., the DCF skill should serve both investment-memo and client-review contexts).

3. **Data-source awareness.** The partner-plugin pattern (LSEG, S&P Global) highlights that skill value scales with clean data inputs. KRASA skills should continue to require structured inputs, flag missing data rather than hallucinate, and note data-source and as-of dates on valuation outputs.

## Status

The Anthropic suite is distributed through Anthropic's enterprise channels and is not redistributed here. This note captures the workflow taxonomy and gap map only — no content has been copied. Concepts were extracted for directional planning; all skill writeups in the KRASA repo are original.

## References

- Anthropic financial-services-plugins repo overview (public README)
- Coverage reporting from Bloomberg, Inc., WealthManagement.com on the Q1 2026 launch
- LSEG and S&P Global partner plugin descriptions

_Added: 2026-04-13 by Landscape Monitor_
