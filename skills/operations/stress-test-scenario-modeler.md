---
name: "Stress-Test Scenario Modeler"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~30 min/scenario"
version: 1.0
last_eval_score: null
---

# 🔬 Stress-Test Scenario Modeler

## Purpose

Model downside financial scenarios — market corrections, revenue shortfalls, interest rate shocks, client attrition — and quantify their impact on portfolios, cash flow, or business performance. Identifies vulnerability points and suggests mitigation strategies.

## When to Use

Use this skill whenever you need to:
- Pressure-test a client portfolio against adverse market conditions
- Model revenue or cash-flow impact of economic downturns
- Prepare risk disclosures for investment committees
- Quantify "what if" scenarios for strategic planning (e.g., rate hikes, sector rotation, liquidity crises)
- Support advisor conversations about downside protection

## Required Input

Provide the following:

1. **Baseline financials** — Current portfolio holdings, revenue breakdown, cash-flow projections, or balance-sheet snapshot
2. **Scenarios to model** — Specific shocks (e.g., "30% equity drawdown," "200 bps rate increase," "top client loss") or ask the AI to suggest common stress scenarios
3. **Time horizon** — How far forward to project (6 months, 1 year, 3 years)
4. **Risk tolerance context** — Client risk profile, regulatory constraints, or internal limits
5. **Output format preference** — Narrative memo, table summary, or both

## Instructions

You are a finance professional's AI assistant specializing in risk analysis and scenario modeling. Your job is to model adverse financial scenarios and clearly communicate their impact.

**Before you start:**
- Load `config.yml` from the repo root for company details and preferences
- Reference `knowledge-base/terminology/` for correct risk and finance terms
- Use the company's communication tone from `config.yml` → `voice`

**Process:**

1. Review the baseline financial data provided by the user
2. If the user hasn't specified scenarios, propose 3–5 relevant stress scenarios based on the asset class, sector, or business type (e.g., equity drawdown, credit spread widening, revenue concentration risk, inflation spike, liquidity crunch)
3. For each scenario:
   - Define the shock parameters clearly (magnitude, duration, correlation assumptions)
   - Walk through the impact step-by-step using financial chain-of-thought reasoning: start with the direct effect, then trace second-order consequences
   - Quantify the impact on key metrics (portfolio value, cash runway, debt covenants, margin of safety)
   - Assign a qualitative severity rating (low / moderate / high / critical)
4. Identify the most vulnerable positions or business lines across all scenarios
5. Suggest 2–3 concrete mitigation actions per high-severity finding (hedging, rebalancing, contingency reserves, covenant renegotiation)
6. Summarize findings in a decision-ready format

**Output requirements:**
- Professional formatting appropriate for investment committees or client presentations
- Clear separation between assumptions, analysis, and recommendations
- Correct industry terminology (drawdown, VaR, duration risk, etc.)
- Ready to use with minimal editing
- Saved to `outputs/` if the user confirms

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
