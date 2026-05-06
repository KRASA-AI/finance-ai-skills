# Anthropic Finance Stack — May 2026 Update

## What changed (week of 2026-05-04)

Between May 4 and May 5, 2026 Anthropic shipped what is the largest single-week expansion of the Claude finance stack since the financial-services-plugins debut: ten new agent templates targeted at financial services and insurance, completion of Microsoft 365 add-in coverage, eight new third-party data connectors, an embedded Moody's MCP application, and a stand-alone enterprise-AI-services joint venture with Blackstone, Hellman & Friedman, and Goldman Sachs. A separate FIS partnership announcement on May 4 brought a Financial Crimes AI Agent into the same agent-template family. The stack pairs against Claude Opus 4.7 (GA April 16, 2026; Vals AI Finance Agent v1.1 64.37%; General Finance benchmark 0.813 vs. Opus 4.6 0.767).

This article extends `anthropic-financial-services-plugins.md` (Q1 2026 vintage) with the May 2026 stack expansion.

## Ten new agent templates

Anthropic's May 5, 2026 release organizes the agents into a research-and-modeling cluster and an operations-and-controls cluster:

- **Pitch builder** — generates comparable-company analysis, draft pitchbook, and audience-matched messaging
- **Meeting preparer** — pulls counterparty / portfolio / regulatory context into a meeting brief
- **Earnings reviewer** — earnings-call summarization, post-earnings update, thesis re-test
- **Model builder** — three-statement / DCF / LBO model construction from filings and data feeds
- **Market researcher** — sector research, catalyst calendar, competitor scan
- **Valuation reviewer** — valuation-output review against firm methodology and audit trail
- **General ledger reconciler** — GL reconciliation against subsidiary-ledger and bank-statement sources
- **Month-end closer** — close checklist, journal-entry preparation, close-report production
- **Statement auditor** — financial-statement consistency / completeness / audit-readiness pass
- **KYC screener** — entity-file assembly, source-document review, escalation packaging for compliance

Each template ships in three forms: as a Claude Cowork plugin, as a Claude Code plugin, and as a Claude Managed Agents cookbook for longer-running agentic workflows with full audit logs accessible to compliance and engineering teams.

The release framing is that each template is a **reference architecture** packaging three components — *skills* (instructions and domain knowledge for the task), *connectors* (governed access to the data the task runs on), and *subagents* (additional Claude models invoked for sub-tasks like comparables selection or methodology checks). This three-component packaging is the canonical signal for the broader 2026 ecosystem and informs how KRASA should think about composing skills + connectors + subagents inside a single template.

## Microsoft 365 integration completion

The May 5, 2026 release completes the Office add-in suite (Word add-in shipped April 2026; Outlook coming soon). The functional capability set:

- **Excel** — Claude builds financial models from filings and data feeds, audits formulas across linked workbooks, runs sensitivity analyses, and reads / writes through native add-in controls
- **PowerPoint** — Claude drafts decks that update when underlying numbers change; Excel-PowerPoint shared context allows a deck cell to refresh from a model cell without reopening the model
- **Word** — Claude edits credit memos, IC memos, and audit memos against firm templates with native tracked changes
- **Outlook (coming)** — email composition and triage carrying the same shared context

Word + Excel + PowerPoint share a single context, so work that starts in a model can finish in a deck without re-explaining anything in between. The March 2026 update introduced Shared Context between Excel and PowerPoint, reusable Skills for one-click workflows, and MCP Connectors support (S&P Global, LSEG, Daloopa, PitchBook, Moody's, FactSet); the May release widens that connector roster.

## Eight new third-party data connectors

The data-connector roster expanded with: **Dun & Bradstreet**, **Experian**, **Fiscal AI**, **Financial Modeling Prep**, **Guidepoint**, **GLG**, **IBISWorld**, **SS&C IntraLinks**, **Third Bridge**, and **Verisk** — joining the existing **LSEG**, **S&P Capital IQ**, **Morningstar**, **PitchBook**, **FactSet**, **Daloopa**, **Box**, and **Palantir** roster from prior cycles.

The mix tilts the stack toward expert-network research (Guidepoint / GLG / Third Bridge), private-company and credit data (D&B / Experian / Fiscal AI / Financial Modeling Prep), and insurance / industry data (Verisk / IBISWorld). SS&C IntraLinks brings deal-room and document-collaboration data into the same surface.

## Moody's MCP app

Moody's launched a Model Context Protocol app embedding Moody's Agentic Solutions (MAS) directly into Claude Desktop, Claude.ai, and Claude Enterprise. The MAS surface is grounded in **600 million entities, 2 billion ownership links**, and unified credit / compliance / operational risk intelligence. Moody's purpose-built agents support credit analysis (memo generation, peer comparisons, scorecard assessment) and compliance workflows (entity profiling, ownership-structure mapping, adverse-media screening, sanctions checks).

The Moody's MCP app is positioned as a complement (not a replacement) for the Anthropic-curated agent templates: a firm can compose, e.g., the Anthropic Pitch builder template against the Moody's MCP for entity-grounded ownership and adverse-media context.

## $1.5B Anthropic + Blackstone + Hellman & Friedman + Goldman Sachs joint venture

Announced May 4, 2026, a stand-alone enterprise-AI-services firm with approximately **$1.5 billion in committed capital**: $300 million each from Anthropic, Blackstone, and Hellman & Friedman; additional commitments from a consortium that includes General Atlantic, Leonard Green, Apollo Global Management, GIC, and Sequoia Capital. The structure embeds Anthropic engineering and partnership resources directly within a stand-alone team. The target market is mid-sized companies, with the founding sponsors' PE-owned portfolio companies as the initial proving ground (healthcare, manufacturing, financial services, retail, real estate).

The joint venture is the highest-profile signal yet that Claude in finance is becoming a **deployment-and-operations** problem more than a model-and-skill problem — a positioning that has direct implications for KRASA's deferred Multi-Agent Orchestration KB candidate (BMAD-CORE™ v6 / VoltAgent / Finance Guru / TauricResearch / 0ldh / ancs21 21-persona / Anthropic three-component architecture).

## FIS Financial Crimes AI Agent (May 4, 2026)

FIS announced May 4, 2026 a Financial Crimes AI Agent built with Anthropic. The agent compresses AML alert and case investigations from days to minutes by automatically assembling evidence across a bank's core systems, evaluating activity against typology libraries, and surfacing the highest-risk cases for investigator review. Functional outcomes: false-positive reduction, SAR-narrative-quality enhancement, audit-trail-traceable decisions inside FIS-controlled infrastructure. **BMO** and **Amalgamated Bank** are the announced launch deployments; broader availability is planned for H2 2026. Anthropic's Applied AI team and forward-deployed engineers are embedded with FIS to co-design the agent.

The roadmap covers credit decisioning, deposit retention, customer onboarding, and fraud prevention — a direct overlap with KRASA's existing AML Monitoring Tuner, Sanctions / AML Alert Reviewer, and KYC / CIP Onboarding Workflow skills, and a reinforcing data-point for the deferred Fair Lending / HMDA / CRA AI-Compliance Reviewer.

## AIG + Anthropic + Palantir (insurance underwriting)

AIG's gen-AI-powered underwriting assistant — built with Anthropic and Palantir — ingests and prioritizes every new excess-and-surplus submission, allowing review of additional policies without adding new staff. Verisk's MCP integration provides property, casualty, and specialty-insurance data for underwriting, claims, and risk analysis. Claude scored 88% as accurate as a human expert on insurance claims in a benchmark referenced at the May 5 release.

This is adjacent to KRASA's finance scope but the stack-level signal — that vertical agents are now common-enough across finance and insurance to share a connector roster — informs how KRASA should think about cross-vertical reuse of v2.1 patterns.

## Anthropic enterprise momentum (context)

- 2026 revenue run-rate climbed above **$30 billion**, up from $9 billion at year-end 2025
- Companies spending $1M+ annually doubled from 500 to **1,000+** in two months
- Google's investment in Anthropic was set at $40 billion across staged commitments (initial $10B at a $380B valuation, announced April 24, 2026)

## Implications for the KRASA finance repo

**Coverage convergence (KRASA already has):**
- Pitch builder ↔ KRASA Pitch-Book Generator
- Meeting preparer ↔ KRASA Advisor Meeting Prep
- Earnings reviewer ↔ KRASA Earnings Call Summarizer + Investment Thesis Tracker
- Model builder ↔ KRASA Three-Statement Model Constructor + DCF Valuation Builder + LBO Model Builder
- Market researcher ↔ KRASA Market Research Brief + Morning Notes Drafter
- Valuation reviewer ↔ KRASA DCF Valuation Builder + Comparable Company Analysis + Financial Model Documenter
- General ledger reconciler ↔ KRASA Month-End Close Flux Commentator (overlap; KRASA's flux-commentator covers commentary while a GL-reconciler is a sibling)
- Month-end closer ↔ KRASA Month-End Close Flux Commentator (close-report production overlap)
- Statement auditor ↔ KRASA AI Controls Auditor (ICFR) + Regulatory Filing Checker (audit-readiness review)
- KYC screener ↔ KRASA KYC / CIP Onboarding Workflow + Sanctions / AML Alert Reviewer

**Coverage near-miss / sibling-skill candidates (to consider next):**
- A General-Ledger Reconciler skill distinct from Month-End Close Flux Commentator (the reconciler is sub-ledger-to-GL line-item, while the flux-commentator is period-over-period explanation)
- A Statement-Auditor skill distinct from the AI Controls Auditor (ICFR) skill (the statement-auditor is consistency / completeness / audit-readiness review, while the ICFR auditor is internal-control review)
- A Pitch-Builder front-end orchestrator skill that calls KRASA Pitch-Book Generator + Comparable Company Analysis + DCF Valuation Builder as sub-agents under a unified template (matches Anthropic's three-component reference architecture)

**Coverage gaps (still open):**
- Hedging & Portfolio Protection — *closed in this cycle* (skills/customer-service/hedging-portfolio-protection.md)
- ESG / Sustainability Reporting Reviewer — deferred next cycle
- Capital-Call / Distribution Waterfall Audit — deferred
- CECL / Allowance for Credit Losses Documenter — deferred
- Fair Lending / HMDA / CRA AI-Compliance Reviewer — deferred (FIS partnership reinforces)
- Foreign Account / Cross-Border Tax Reporting (FATCA / CRS / FBAR) — deferred
- Bootstrapped-CFO Dashboard / SaaS Metrics Coach — deferred (joint-venture mid-market focus reinforces)

**Architecture alignment.** KRASA's v2.1 skill idiom (12-step process, 14 audience templates, Compliance Layer, Personalization Hooks, Handoff Contracts) maps cleanly onto Anthropic's three-component reference architecture (skills + connectors + subagents): the Compliance Layer corresponds to skill instructions; the Handoff Contracts correspond to subagent-and-skill composition; Personalization Hooks correspond to connector configuration. The gap-of-note is **explicit subagent declarations** — KRASA skills currently express subagent composition through Handoff Contracts, but the Anthropic reference architecture treats subagents as a first-class field. A future v2.2 lift could add a `Subagent Composition` section to formalize the equivalence, particularly for skills like Pitch-Book Generator that legitimately call multiple downstream skills as sub-agents in a single run.

## Vendor-and-data-source mapping (for KRASA's Personalization-Hooks `connector_register`)

| Provider | Surface | KRASA-relevant skills |
|---|---|---|
| LSEG | bond pricing, yield-curves, FX, options, macro | DCF Valuation Builder, Hedging & Portfolio Protection, Trade Surveillance Reviewer |
| S&P Capital IQ | tearsheets, earnings previews, funding | Market Research Brief, Pitch-Book Generator, Investment Memo Drafter |
| Morningstar | fund-and-stock data, manager reports | Tax-Aware Portfolio Rebalancer, IPS Generator, Client Portfolio Update |
| FactSet | fundamentals, estimates, ownership | DCF Valuation Builder, Comparable Company Analysis, Earnings Call Summarizer |
| PitchBook | private-market deals, fund data | PE Due Diligence Synthesizer, CIM Drafter, Investment Memo Drafter |
| Daloopa | normalized issuer fundamentals | Three-Statement Model Constructor, DCF Valuation Builder |
| Moody's MCP (MAS) | 600M entities, 2B ownership links, credit + compliance | KYC / CIP Onboarding Workflow, Sanctions / AML Alert Reviewer, Credit Memo Drafter, Loan Covenant Compliance Monitor |
| Dun & Bradstreet | private-company credit, hierarchies | KYC / CIP Onboarding Workflow, Credit Memo Drafter |
| Experian | consumer / commercial credit | Credit Memo Drafter, Loan Covenant Compliance Monitor |
| Fiscal AI | private-company fundamentals | PE Due Diligence Synthesizer, Three-Statement Model Constructor |
| Financial Modeling Prep | API fundamentals + economic calendar | Investment Thesis Tracker, Morning Notes Drafter |
| Guidepoint / GLG / Third Bridge | expert networks | PE Due Diligence Synthesizer, Market Research Brief, Investment Memo Drafter |
| IBISWorld | industry research | Market Research Brief, PE Due Diligence Synthesizer |
| SS&C IntraLinks | deal-room + document collaboration | CIM Drafter, PE Due Diligence Synthesizer |
| Verisk | property / casualty / specialty insurance data | (insurance-vertical adjacency — out of finance scope but informs cross-vertical KB) |
| Box | document store | every skill (file ingestion / output) |
| Palantir | data-integration + governance | (enterprise-grade pattern reference; informs deployment KB candidate) |

## References (concept-only)

Sources surveyed for this update were Anthropic's news posts on the financial-services agent release and the Blackstone / Hellman & Friedman / Goldman joint venture, the corresponding announcements from Blackstone, GIC, FIS, Moody's, and AIG, ecosystem coverage from Fortune, Bloomberg, CNBC, Reuters, Yahoo Finance, the Register, TechCrunch, Finextra, InvestmentNews, the CFO, ProgramBusiness, ResultSense, Releasebot, Crypto Briefing, Reinsurance News, PYMNTS, Disruption Banking, Hoodline, ts2.tech, the Next Web, and How2Shout. No verbatim text is lifted; all concepts paraphrased into KRASA / finance idiom. Vendor descriptions and connector mappings reference each provider's public-facing capability description only.
