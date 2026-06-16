# Agentic AI Patterns for Finance

## Overview

Agentic AI in finance refers to multi-step, autonomous workflows where AI agents coordinate to complete complex financial tasks end-to-end. Unlike single-prompt interactions, agentic workflows orchestrate multiple specialized steps — data retrieval, analysis, decision logic, and output generation — in a unified pipeline.

## Key Architecture: Multi-Agent Orchestration

The dominant pattern in financial services is a **supervisor-specialist model**:

- A central orchestrator agent receives the high-level task
- Specialized sub-agents handle specific domains (data retrieval, risk analysis, compliance checking, narrative generation)
- The orchestrator consolidates results and manages handoffs

### Example: Loan Underwriting Pipeline
1. **Data Agent** — Pulls applicant financials, credit history, collateral data
2. **Financial Analysis Agent** — Computes ratios, coverage metrics, cash flow adequacy
3. **Risk Agent** — Scores risk factors, flags policy exceptions
4. **Compliance Agent** — Checks regulatory requirements, fair lending rules
5. **Narrative Agent** — Drafts the credit memo with findings and recommendation

## Adoption Stats (2026)

- 44% of finance teams will use agentic AI in 2026 (Wolters Kluwer), up 600% from 2025
- Only 11% of companies have deployed agents in production despite 99% planning to
- Organizations achieving 2.3x ROI within 13 months on agentic investments (KPMG)
- KYC processing time reduced by up to 99% at early adopters

## Relevance to KRASA Skills

While KRASA skills are currently single-agent (one skill, one task), the concepts inform skill design:

- **Decomposition**: Break complex tasks into clear sequential steps (mirrors multi-agent handoffs)
- **Specialization**: Each skill should do one thing well rather than trying to cover everything
- **Composability**: Skills should produce outputs that can feed into other skills (e.g., Earnings Call Summarizer output feeds into Market Research Brief)
- **Guardrails**: Include compliance and validation steps within skill instructions, mirroring the compliance agent pattern

## Future Considerations

As AI platforms add native multi-agent support, KRASA skills could evolve into composable agent components. Design skills with clean inputs/outputs to enable future orchestration.

> **Deeper companion:** see `multi-agent-orchestration-finance.md` for the production topologies (supervisor-specialist, sequential pipeline, debate-and-consensus, hierarchical), the safety layers (handoff contracts, loop detection / step limits, least-privilege identity inheritance, human-in-the-loop at irreversible boundaries, immutable tracing), and the evaluation discipline (information-leakage / look-ahead, reliability / faithfulness, execution-grounded safety) that keep multi-agent finance workflows defensible — plus how the patterns compose into KRASA skills and the deferred v2.2 Subagent Composition idiom.
