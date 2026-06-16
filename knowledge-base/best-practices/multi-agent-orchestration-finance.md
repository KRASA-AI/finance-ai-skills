# Multi-Agent Orchestration Patterns for Finance

## Overview

The single most consequential architectural shift in financial AI through the first half of 2026 is the move from single-prompt assistants to **multi-agent orchestration** — systems in which a coordinating agent decomposes a task, dispatches it to specialized sub-agents, and consolidates their outputs into a single deliverable or action. The Anthropic May 2026 ten-agent reference release codified this as the de facto financial template (skills + governed connectors + subagents), and the surrounding vendor surface — Fiserv agentOS, the FIS Financial Crimes AI Agent, Allvue + RSM's Capital Operating Model, OneStream's Finance Agentic Layer, the OpenAI + PwC Native Finance Function, SAP Autonomous Finance, and the Experian Agent Operating System — all ship multi-agent compositions over a governed-data substrate.

This note is the deeper companion to `agentic-finance-patterns.md` (which introduces the supervisor-specialist idea at a high level). It specifies the topologies that work in regulated finance, the safety layers that keep them defensible, the evaluation discipline that keeps them honest, and how the patterns compose into KRASA skills. It is adjacent to `governed-data-agentic-layer.md` (the identity-and-data substrate underneath), `agentic-ai-model-risk-management.md` (the MRM discipline over the top), and `multi-persona-ensemble-debate.md` (the debate-and-consensus pattern as a specific orchestration topology).

## Why This Matters Now

Three signals converge in mid-2026:

**Supervisor-specialist is what production looks like.** The 2026 practitioner consensus is that the **supervisor (orchestrator) → specialist** topology is the production-grade default: a supervisor maintains global task state, decomposes the request via a planning step, selects and dispatches the right specialist, manages inter-agent handoffs, and consolidates results. Documented financial implementations follow this shape (e.g., a document-verification agent + remediation agent + underwriting specialist composed atop an origination system). The same publications are explicit that **free-form "swarm" topologies are not a defensible architecture for regulated domains** (finance, healthcare, EU AI Act Annex III), because they sacrifice the auditability, determinism, and bounded behavior that supervision provides.

**The orchestration runtime is now a build-vs-buy decision.** LangGraph (graph-based, deterministic), the Microsoft Agent Framework (the converged AutoGen + Semantic Kernel SDK), and CrewAI (role-based) lead the framework category, typically paired with an observability layer (e.g., LangSmith) and the firm's own domain logic. Financial institutions are converging on a hybrid posture: buy the orchestration runtime and observability, build the proprietary domain specialists and the guardrails.

**The research literature is now a safety literature.** The 2026 finance-agent papers are less about new capability and more about failure modes: information-leakage / look-ahead bias that inflates backtests (*Profit Mirage*), reliability and faithfulness gaps in trading agents (*TradeTrap*), execution-grounded agent-safety benchmarking (*FinVault*), fine-grained multi-agent task decomposition (*Toward Expert Investment Teams*), and evolutionary multi-agent alpha mining (*QuantaAlpha*) — alongside an audit-oriented evidence map reframing trading agents as expert-system decision pipelines. The lesson: orchestration buys throughput and decomposition, but each added agent is an added failure surface, so the eval and safety discipline must scale with the topology.

## The Topologies That Work in Finance

**Supervisor-specialist (the default).** A single orchestrator owns the task, plans the decomposition, dispatches to specialists, and consolidates. Best for workflows with a clear sequence and a single accountable output — credit memos, pitchbooks, month-end close, KYC screening. The supervisor is the natural place to enforce policy, log the plan, and gate irreversible actions.

**Sequential pipeline (hand-off chain).** Specialists run in a fixed order, each consuming the prior's output — data agent → analysis agent → risk agent → compliance agent → narrative agent. Best where the process is genuinely linear and each stage's output is a clean, validatable artifact. KRASA's existing handoff contracts already express this shape.

**Debate-and-consensus / ensemble.** Multiple specialist personas analyze the same input and a consolidator weighs their conclusions — bull/bear researcher debate, multi-analyst-persona consensus for buy/sell/hold. Covered in depth in `multi-persona-ensemble-debate.md`; useful for judgment-heavy tasks where diversity of view improves the decision, but it must be paired with the look-ahead / information-leakage cautions below.

**Hierarchical (supervisor-of-supervisors).** For large workflows, a top orchestrator dispatches to mid-level orchestrators that each own a sub-domain. Reserve for genuinely large compositions; every added layer adds latency, cost, and audit surface.

Avoid free-form swarms in regulated workflows. If agents can talk to any other agent with no supervisor and no fixed contract, the system is hard to audit, hard to bound, and hard to defend to a regulator.

## The Safety Layers That Keep Orchestration Defensible

Multi-agent systems multiply the surfaces from `ai-redteam-prompt-injection-defense-reviewer.md` — more tool calls, more inter-agent messages, more places for an indirect injection or a runaway loop. The non-negotiable layers:

**Handoff contracts.** Every inter-agent edge has a defined input schema, output schema, and acceptance check. An agent does not accept free-form text from a peer; it accepts a validated artifact. KRASA's bidirectional Handoff Contracts sections are this discipline expressed at the skill level.

**Loop detection and step limits.** A hard cap on total steps and a detector for repeated / cyclic states prevent an agent from looping indefinitely or burning cost — the safety layer documented in self-validating research-agent architectures (e.g., loop-detection + step-limit in autonomous financial-research agents). Every orchestrated KRASA-style workflow should carry an explicit step ceiling.

**Least-privilege identity inheritance.** Each specialist inherits permissions from the principal via the governed-data layer (`governed-data-agentic-layer.md`); no agent holds standing write-capability it does not need. The supervisor never grants a specialist more scope than its sub-task requires.

**Human-in-the-loop at irreversible boundaries.** The supervisor gates every irreversible action (post a JE, release a payment, submit a filing, send an external communication) behind an informed human approval — never an agent-to-agent auto-approval. Auto-action across an agent boundary at a high materiality tier is a presumptive red-team / MRM finding.

**Immutable, reconstructable tracing.** The full plan, every dispatch, every tool call, and every consolidation step are logged immutably so an incident can be reconstructed and a validator can audit the decision path.

## The Evaluation Discipline That Keeps Orchestration Honest

Orchestration's failure modes are subtle and must be tested for explicitly:

- **Information leakage / look-ahead bias** (*Profit Mirage*) — when an agent in a backtest or research pipeline sees data it would not have had at decision time, performance is inflated. Test temporal integrity end-to-end across the agent graph, not just at the final step.
- **Reliability and faithfulness** (*TradeTrap*) — does each agent's stated reasoning actually match the action it took, and is behavior stable across repeated runs of a non-deterministic system? Require repeated-trial, worst-case evaluation, not a single happy-path pass.
- **Execution-grounded safety** (*FinVault*) — evaluate the system in an environment where actions have consequences, with no live write-capability and no production data during testing.
- **Per-agent and end-to-end metrics** — each specialist carries its own quality metrics (precision/recall for screening agents, hallucination rate for narrative agents, override rate for human-gated agents), and the orchestrated whole carries an end-to-end accuracy and a consolidation-fidelity metric. A pipeline can have healthy components and an unhealthy whole.

This overlay is a Measure/Manage activity under the MRM spine in `agentic-ai-model-risk-management.md`; each orchestrated surface is a distinct inventory entry and is tiered on its end-to-end decision impact, not on any single agent.

## How This Composes Into KRASA Skills

KRASA skills are authored as single-responsibility units with clean inputs/outputs and explicit Handoff Contracts — which is precisely what makes them composable into the topologies above. The design rules that keep them orchestration-ready:

- **One skill, one responsibility** — a specialist does one thing well; the orchestrator composes, the skill does not try to be the orchestrator.
- **Validatable artifacts, not free text** — a skill's output is structured enough that the next skill (or a supervisor) can accept-check it; the Handoff Contracts sections name the upstream and downstream skills explicitly.
- **Guardrails inside the skill** — each skill carries its own Regulatory & Compliance Layer and validation steps, so policy enforcement does not depend solely on the orchestrator.
- **A natural supervisor surface** — front-end / orchestrator skills (e.g., a pitch-builder composing comparable-company-analysis + dcf-valuation-builder + earnings-call-summarizer + market-research-brief) are the place to put the plan log, the step limit, and the human-approval gate.

The deferred v2.2 "Subagent Composition" skill idiom is the formalization of this: marking a skill as a supervisor that declares its specialist set, its handoff schema, its step ceiling, and its human-approval boundaries — turning KRASA's existing handoff graph into an explicit, auditable orchestration spec.

## Adoption Signals — 2026

- **Anthropic May 2026 ten-agent reference release** — skills + governed connectors + subagents as the canonical financial multi-agent template
- **Supervisor-specialist as the documented production default**; swarm explicitly rejected for regulated domains
- **LangGraph / Microsoft Agent Framework / CrewAI** lead the orchestration-runtime category; hybrid build-vs-buy is the institutional norm
- **Vendor platforms shipping multi-agent finance compositions** — Fiserv agentOS, FIS Financial Crimes AI Agent, Allvue + RSM Capital Operating Model, OneStream Finance Agentic Layer, OpenAI + PwC Native Finance Function, SAP Autonomous Finance, Experian Agent Operating System
- **A maturing safety/eval literature** — Profit Mirage (information leakage), TradeTrap (reliability/faithfulness), FinVault (execution-grounded safety), Toward Expert Investment Teams (task decomposition), QuantaAlpha (evolutionary alpha mining), and the 77-study expert-system-decision-pipeline survey

## Anti-Plagiarism Note

This note is composed in KRASA / finance terminology and synthesizes only concepts — topologies, safety layers, evaluation failure modes, and adoption signals — from public practitioner publications, vendor press materials, and research abstracts. No framework documentation, vendor white-paper text, or research-paper content (payloads, prompts, code, or datasets) is reproduced. Framework and runtime references (LangGraph, Microsoft Agent Framework, CrewAI, LangSmith, OWASP Agentic Top 10, NIST AI RMF / AI 100-2, ISO/IEC 42001, EU AI Act) name public tools / standards only. Research references (Profit Mirage 2510.07920; TradeTrap 2512.02261; QuantaAlpha 2602.07085; Toward Expert Investment Teams 2602.23330; FinVault; the 77-study trading-agent survey) are paraphrased at the topology / failure-mode / methodology level only. Vendor-launch paraphrases (Anthropic ten-agent release; Fiserv agentOS; FIS; Allvue + RSM; OneStream; OpenAI + PwC; SAP Autonomous Finance; Experian Agent Operating System) are drawn from public announcements and trade-press coverage with no verbatim text reproduced.
