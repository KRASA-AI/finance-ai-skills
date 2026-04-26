# Multi-Persona Ensemble & Bull-Bear Debate Patterns for Investment Decisions

## Overview

A growing set of open-source equity-analysis projects deploys not a single AI persona but a panel of specialized analyst personas — fundamentals analyst, technical analyst, sentiment analyst, news analyst, valuation analyst, risk manager, portfolio manager — and aggregates their independent signals into a consensus recommendation. Several frameworks add a dialectical layer where bull-case and bear-case "researchers" debate the panel's findings before a portfolio-manager persona renders a final decision. The pattern has matured rapidly in 2025–2026 and is now reflected in multiple stargazed Claude Code / LLM frameworks. This note captures the underlying technique — not any specific framework's content — for application inside KRASA finance skills.

## Why It Works

A single-persona prompt collapses what is, in practice, a multi-disciplinary professional debate (fundamentals vs. technicals, growth vs. value, near-term vs. long-term) into one voice. That single voice tends to converge on the most fluent answer rather than the most calibrated one. A persona panel preserves the disagreement until late in the pipeline, which:

- Surfaces the variables that drive the decision instead of burying them in a synthesized narrative
- Forces the model to make explicit the weighting it applied — which the user can then accept, override, or reconfigure
- Produces a better-calibrated confidence number, because consensus across heterogeneous personas is more informative than a single confident assertion
- Mirrors how institutional analysts actually work — the IC challenge, the bull-bear briefing, the "red team" pre-mortem — so the output is recognizable to professional users
- Captures a richer evidence trail that supports the books-and-records story under SEC Marketing Rule 206(4)-1 and Advisers Act Rule 204-2

## The Two Dominant Topologies

### Topology A — Independent Personas → Weighted Consensus

Each persona analyzes the input independently using its own methodology. A consensus aggregator weights each persona's signal (Buy / Sell / Hold + confidence) by the firm's calibration of that persona's reliability for the asset class and time horizon. Final recommendation is the weighted majority, and the output exposes which personas agreed and which dissented.

Use when: the decision lends itself to clear quantitative aggregation (Buy / Sell / Hold, sizing tier), the personas have stable historical accuracy that can be tracked, and the user wants a "vote tally" output for transparency.

Risk: the consensus can be dominated by personas that share an underlying data dependency (e.g., a fundamentals persona and a valuation persona both built on the same accounting data), giving the appearance of independent confirmation when in fact a single bad input has cascaded.

### Topology B — Persona Panel → Bull / Bear Researchers → Portfolio Manager

The persona panel produces a research dossier. Two "researcher" personas (bullish and bearish) each take the dossier and build the strongest possible case for their side, citing the panel's findings. A portfolio-manager persona reviews both cases and renders a decision with explicit weighting commentary.

Use when: the decision is qualitative and contested (high-conviction / low-conviction theses, event-driven trades, contrarian positioning), the user wants a defensible "we considered both sides" record, and the aggregation step itself is decision-relevant (the PM's reasoning, not a vote count, is the deliverable).

Risk: the dialectic can become performative if either researcher is allowed to invent facts not in the panel's dossier. The dossier must be the single source of truth — researchers argue interpretation, never invent evidence.

## Application Inside KRASA Skills

The pattern is a technique, not a skill on its own. It composes into existing KRASA skills as follows:

- **`skills/operations/investment-memo-drafter.md`** — In the IC Vote Block, the multi-persona pattern naturally maps to named IC members with documented conviction taxonomies; the dialectical layer maps to the firm's bull-bear / red-team convention. The skill's existing handoff to `comparable-company-analysis.md` and `dcf-valuation-builder.md` already produces the per-persona inputs
- **`skills/operations/investment-thesis-tracker.md`** (new this cycle) — The Confirmed / Pending / Challenged / New classifier benefits from a persona-panel pre-pass: each persona scores the new evidence against its own methodology before the consensus classifier runs, producing a richer evidence-log entry
- **`skills/operations/morning-notes-drafter.md`** — The "what matters today" triage gains calibration from a fundamentals + technicals + sentiment + macro mini-panel; the synthesizer keeps the output to a single voice but the underlying triage is multi-perspective
- **`skills/operations/earnings-call-summarizer.md`** — Bull-bear debate over the post-call thesis update produces a stronger Confirmed / Pending / Challenged / New marker than a single-pass extraction
- **`skills/operations/pe-due-diligence-synthesizer.md`** — The contradiction-sweep step is itself a panel pattern; the bull-bear debate maps cleanly onto the Go / No-Go / Conditional recommendation step
- **`skills/operations/credit-memo-drafter.md`** — Risk Assessment section benefits from a credit-bull / credit-bear / portfolio-construction-PM panel before the final risk-rating recommendation

## Implementation Cautions

**Independence is the value.** The pattern fails the moment two personas implicitly share a prompt-pool, a system prompt, or a shared chain-of-thought. Each persona must be instantiated with its own methodology, terminology, and evaluation criteria — not a Buy/Sell/Hold templater behind different costumes.

**Calibrate the weights.** A static "all personas weighted equally" is a placeholder, not a discipline. Track per-persona historical hit rate and Brier score against the firm's track record and recalibrate the weights at least quarterly. Without calibration, the consensus is not better than a single well-prompted persona.

**Compliance posture.** Multi-persona output that mixes bull and bear cases, displayed externally, can cross the Marketing Rule 206(4)-1 line on cherry-picking and balanced-presentation. Internal use is unambiguous; external use should run through the Marketing Rule screen in `skills/admin/regulatory-filing-checker.md` and the Compliance Notes block in the audience-specific output templates.

**Dossier discipline.** In Topology B, the bull and bear researchers must be hard-constrained to the panel's dossier. Any researcher fabrication is a hallucination disguised as advocacy. Enforce with a "cite-the-dossier-row" requirement on every claim.

**ICFR / SOX-AI relevance.** When any persona's output flows into a GL, a JE, a reconciliation decision, or a financial disclosure, the persona panel itself is in scope for the AI Controls Auditor for ICFR (`skills/admin/ai-controls-auditor-icfr.md`). Apply the same four-eyes / change-management / monitoring discipline to the panel as to a single-agent system.

**Books and records.** Both topologies produce a richer audit trail than single-pass synthesis (every persona's signal, every researcher's case, the PM's weighting commentary). Ensure the persistence layer captures the full panel output, not just the final recommendation, and that retention satisfies Advisers Act Rule 204-2 (RIA) or SEC Rule 17a-4 (broker-dealer) applicable to the firm.

## Adoption Signals (2025–2026)

- Multiple Claude Code / LLM frameworks have shipped multi-persona aggregation patterns in the past two quarters; the technique has crossed into the mainstream of equity-research tooling
- Open-source projects in this space have collectively accumulated several thousand stargazers, indicating durable practitioner interest beyond a single research wave
- The pattern aligns with the broader 2025–2026 shift in enterprise AI from monolithic single-agent prompts to multi-agent / supervisor-specialist topologies (see `agentic-finance-patterns.md`)
- Bull-bear debate as a formal LLM technique has appeared in academic literature on financial-trading frameworks and has been operationalized in several open-source trading-agent projects

## Relationship to `agentic-finance-patterns.md`

The persona-panel-with-debate is a specialization of the supervisor-specialist multi-agent topology covered in `agentic-finance-patterns.md`. Where that note generalizes the pattern across reconciliation / variance / JE / disclosure agents, this note specializes it to the investment-decision use case where the value of preserving disagreement until the final synthesis is highest. Both notes should be considered together when designing any KRASA skill that produces a Buy / Sell / Hold / Conviction-change output, a Go / No-Go / Conditional recommendation, or a credit risk-rating.

## Anti-Plagiarism Note

This note documents a technique observed across a class of publicly-released frameworks (TauricResearch/TradingAgents, ancs21/ai-sub-invest, and adjacent projects). No code, prompt, or proprietary methodology has been copied. The persona taxonomy described above is a paraphrase of the standard analyst-discipline split in the institutional research industry and predates any specific implementation.
