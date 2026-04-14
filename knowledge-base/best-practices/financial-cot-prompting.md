# Financial Chain-of-Thought (CoT) Prompting

## Overview

Financial Chain-of-Thought prompting is a technique where AI models decompose complex financial problems into logical, sequential reasoning steps before arriving at conclusions. This approach significantly improves accuracy on multi-step financial analysis tasks.

## Origin

Popularized by the FinRobot platform (AI4Finance Foundation), Financial CoT adapts general chain-of-thought prompting specifically for finance by incorporating domain-specific reasoning patterns like waterfall calculations, ratio decomposition, and scenario branching.

## How It Works

Instead of asking an AI to directly answer a complex financial question, you structure the prompt to require step-by-step reasoning:

1. **Identify the direct effect** — What is the immediate impact of the change or event?
2. **Trace second-order consequences** — How does the direct effect cascade through related metrics?
3. **Quantify each step** — Attach numbers, ranges, or directional estimates to each link in the chain
4. **Synthesize into a conclusion** — Combine the chain into a final assessment with confidence level

## Example Pattern

**Without CoT:** "What happens to this portfolio if rates rise 200bps?"

**With Financial CoT:** "Walk through the impact of a 200bps rate increase step by step: First, calculate the duration impact on the fixed-income allocation. Then, estimate the effect on equity valuations through the discount rate channel. Next, assess the impact on any floating-rate debt. Finally, synthesize the total portfolio impact and identify the most vulnerable positions."

## When to Apply

- Multi-variable financial analysis (stress tests, scenario modeling)
- Valuation exercises with multiple assumptions
- Regulatory impact assessments with cascading effects
- Investment thesis construction requiring logical argument chains
- Budget variance analysis requiring root-cause decomposition

## Integration with KRASA Skills

Skills that benefit most from Financial CoT:
- Stress-Test Scenario Modeler (operations)
- Investment Memo Drafter (operations)
- Financial Model Documenter (operations)
- Budget Variance Analyzer (operations)

When building or refining skills, include explicit CoT scaffolding in the Instructions section to guide the AI through step-by-step reasoning rather than jumping to conclusions.
