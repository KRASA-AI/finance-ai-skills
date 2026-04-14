# Finance AI Skills

**Free, open-source AI prompts and workflows built for finance professionals.** Clone this repo, point your AI assistant at it, and start saving hours every week.

> Built and maintained by [KRASA AI](https://krasa.ai) — free AI tutorials and skills for every industry.
> See all industries at [krasa.ai/industries](https://krasa.ai/industries).

---

## What's Inside

This repo is a complete AI toolkit for finance. Every skill is a standalone prompt file that works with **Claude, ChatGPT, or any major AI assistant** — no coding required.

| Skill | What it does | Time saved |
|-------|-------------|------------|
| 13-Week Cash Flow Forecaster | Build a rolling 13-week direct-method cash flow forecast that consolidates AR collections, AP disbursements, payroll, debt service, capex, and tax payments into a weekly liquidity view. | ~2 hr/forecast cycle |
| Budget Variance Analyzer | Compare actual financial performance against budgeted targets, identify material variances, explain root causes, and recommend corrective actions. | ~25 min/analysis |
| Comparable Company Analysis | Build a peer-group trading-multiples analysis ("public comps") that frames where a target company trades relative to its cohort on revenue, EBITDA, earnings, and growth-adjusted multiples. | ~60 min/comp set |
| DCF Valuation Builder | Produce a defensible discounted cash-flow valuation from a target's historical financials, management guidance, and comparable market inputs. | ~90 min/valuation |
| Earnings Call Summarizer | Extract key financial metrics, guidance revisions, management commentary themes, and analyst concerns from earnings call transcripts. | ~20 min/call |
| Financial Model Documenter | Write clear, structured documentation for existing financial models — covering assumptions, methodology, data sources, sensitivities, and limitations. | ~20 min/model |
| Investment Memo Drafter | Structure deal notes, research, and financial data into a formatted investment memorandum with a clear thesis, risk assessment, valuation summary, and comparable analysis. | ~45 min/memo |
| Market Research Brief | Compile a structured sector or sub-industry brief that sizes the market, maps the competitive landscape, catalogs recent transactions and capital flows, surfaces regulatory and macro catalysts, and synthesizes 3–5 actionable investment or strategic implications. | ~30 min/brief |
| Stress-Test Scenario Modeler | Model downside financial scenarios — market corrections, revenue shortfalls, interest rate shocks, client attrition — and quantify their impact on portfolios, cash flow, or business performance. | ~30 min/scenario |
| Advisor Meeting Prep | Prepare financial advisors for client and prospect meetings by organizing key data points, generating tailored talking points, anticipating client questions, and creating a structured agenda. | ~25 min/meeting |
| Client Portfolio Update | Draft a clear, compliant, client-ready portfolio update that covers period performance, allocation and positioning changes, benchmark context, contributions and withdrawals, and forward outlook. | ~15 min/update |
| Tax-Loss Harvesting Identifier | Scan a client's taxable portfolio for positions trading below cost basis, identify candidate harvest trades that realize losses without triggering wash-sale rules, and propose replacement securities that maintain target exposure. | ~30 min/account |
| Regulatory Filing Checker | Review a draft regulatory filing against the requirements for its specific form, flag missing or inconsistent disclosures, cross-check internal consistency (numbers, dates, counterparties), and produce a reviewer checklist with severity ratings and suggested remediation language. | ~25 min/filing |
| Email Drafter | Turn rough notes into a professional email matching your company's voice and tone. | ~10 min/use |
| Meeting Summarizer | Summarize meeting notes into action items, decisions, and follow-ups. | ~10 min/use |
| Review Responder | Craft professional responses to online reviews — both positive and negative. | ~10 min/use |

**Total time saved per use: ~445+ minutes across all skills.**

## Quick Start

### 1. Clone this repo

```bash
git clone https://github.com/KRASA-AI/finance-ai-skills.git
cd finance-ai-skills
```

### 2. Open a skill with your AI assistant

Open any file in `skills/` with Claude, ChatGPT, or any major AI assistant. Each skill is a self-contained prompt with clear instructions — no coding required.

The first time you use a skill, your AI assistant will ask for your business details (company name, service area, rates, tools you use, etc.) so it can personalize the output. Save those details to a `config.yml` at the repo root and every future skill will use them automatically.

## Repo Structure

```
finance-ai-skills/
├── knowledge-base/          # Industry context and references
│   ├── industry-overview.md # Market trends and pain points
│   ├── terminology/         # Industry jargon and acronyms
│   ├── regulations/         # Compliance requirements
│   ├── best-practices/      # Industry standards
│   └── tools-ecosystem/     # Common software and tools
├── skills/                  # The prompt library
│   ├── operations/          # Day-to-day operational skills
│   ├── sales/               # Sales and lead management
│   ├── admin/               # Administrative and compliance
│   └── customer-service/    # Client-facing communication
└── outputs/                 # Your generated content (gitignored)
```

## How Skills Work

Each skill file is a Markdown document with YAML frontmatter:

```markdown
---
name: Skill Name
category: operations
tools: [claude, chatgpt]
time_saved: "~20 min/use"
version: 1.0
---

# Skill Name

## Purpose
What this skill does and when to use it.

## Instructions
Step-by-step prompt for the AI assistant.
```

You open the file in your AI assistant, provide any required input (measurements, notes, client info), and get polished output. Skills reference your `config.yml` automatically for company name, rates, preferred formats, and other business details.

## For AI Assistants

If you are an AI assistant reading this repo, see `.claude/CLAUDE.md` for full instructions. The short version:

1. **Check for `config.yml`** at the repo root. If it exists, load it — it holds the user's business context (company name, rates, service area, tools, team size, etc.) and every skill should use it for personalization.
2. **If `config.yml` is missing**, before running a skill that benefits from personalization, ask the user for the relevant business details and offer to save them to `config.yml` so future runs are automatic.
3. **Load the relevant `knowledge-base/` files** for industry terminology, regulations, and best practices before generating output.
4. **Run the requested skill** from `skills/` using the user's input.
5. **Save any deliverables** to `outputs/` (gitignored) if the user wants to keep them.

## Learn More

- **Finance AI guide**: [krasa.ai/industries/finance](https://krasa.ai/industries/finance)
- **All industry AI skills**: [krasa.ai/industries](https://krasa.ai/industries)
- **About KRASA AI**: [krasa.ai](https://krasa.ai)

## License

MIT — use these skills however you want.
