# Agentic Payments & "Know Your Agent" Governance for Finance

## Overview

A new payments stack is forming around autonomous AI agents that transact on a principal's behalf. Three protocol layers — Google's Agent Payments Protocol (AP2), OpenAI's Agentic Commerce Protocol (ACP, co-developed with Stripe), and various network-level "agent suites" announced by Mastercard (January 2026) and Visa (Intelligent Commerce) — propose that agentic transactions need their own identity, authorization, and audit trail conventions. Traditional finance runs on KYC (Know Your Customer). Agentic finance is converging on **KYA (Know Your Agent)** as the parallel discipline: who is the agent, who authorized it, what mandate did the principal sign, what is the audit trail, and who is liable when an agent acts beyond its mandate. This note captures the technique and the governance posture — not any specific protocol's content — for application inside KRASA finance skills that touch payments, treasury, custody, fraud, dispute resolution, and regulatory reporting.

## Why This Matters Now

Agentic commerce was a research topic through 2024. In Q1–Q2 2026 it became a concrete payments-infrastructure rollout:

- Mastercard launched Agent Suite (January 2026) as a productization of agentic-AI commerce on the card network with technical advisory wrap. Visa's Intelligent Commerce opened VisaNet APIs for identity, spending controls, and tokenized credentials, with 100+ partners building, 30+ in sandbox, 20+ agents integrating directly.
- Google's AP2 (introduced 2025; partner roster expanded through 2026) is a protocol layer atop existing payment rails for agent-to-agent payment negotiation, execution, and settlement, with cryptographically signed mandates as the authorization primitive. Partner roster includes Mastercard, Adyen, PayPal, Coinbase, Visa, and 50+ others.
- OpenAI / Stripe's ACP uses a Shared Payment Token (SPT) — merchant-bound, dollar-bound, time-bounded, single-use — to scope an agent's authorization at the point of checkout.
- Visa publicly named "Know Your Agent" as the KYC parallel for the agentic era. The framework's central question — *who carries accountability when an autonomous agent transacts outside its authorized scope, and how does that accountability flow back to the principal who deployed it?* — is the regulatory question every bank, fintech, custodian, and treasury function is now being forced to answer.
- The IMF (Notes 2026/004) and ABA Banking Journal (May 2026) frame agentic payments as critical digital infrastructure requiring supervisor-side governance.

For any KRASA finance skill that touches payments, the protocol layer below the skill is no longer just "the rail" — it is now also "the agent's identity and authorization context." Skills that ignore that context will produce audit-deficient outputs.

## The Three Layers of the Stack

### Layer 1 — Agent Identity (KYA)

The agent is a distinct legal-and-operational entity from the principal who deployed it. KYA frameworks require:

- **Agent identifier** — a globally unique, network-resolvable ID for the agent (analogous to a BIC for a bank or a LEI for a legal entity)
- **Principal binding** — a cryptographic link from the agent back to the human or legal principal who authorized its deployment, with the principal's own KYC chain intact
- **Developer / operator vetting** — the entity that built and operates the agent goes through a separate screening (Visa's KYA vetting before sandbox / production access is one early formalization)
- **Permissioning posture** — what the agent is allowed to do, with whom, in what jurisdictions, against what spending controls and risk-rating thresholds
- **Lifecycle controls** — agent activation, suspension, revocation, and post-revocation transaction handling (the inflight-transaction problem when an agent is suspended mid-purchase)
- **Auditable provenance** — every agent action ties back to the agent ID, the principal binding, the developer vetting record, and the operator's monitoring log

### Layer 2 — Authorization (Mandates and Tokens)

Two design patterns dominate:

- **Mandate-based authorization (AP2-style)** — the principal signs a machine-readable mandate that defines the agent's authority: dollar ceilings, time-windows, merchant categories, geographic scope, party restrictions, risk-rating limits. The mandate is a cryptographically signed digital contract that any counterparty can verify. The agent cannot exceed the mandate's parameters; counterparties reject transactions that violate the mandate; merchants gain a verifiable audit trail for chargeback defense.
- **Tokenized scope (ACP-style)** — a Shared Payment Token issued by the issuer or payment provider that is merchant-bound, amount-bound, time-bounded, single-use. The token is the authorization artifact at the point of checkout; the merchant settles against the token; revocation is by token expiration or explicit kill-switch.

Both patterns share the same underlying discipline: **separate decision from execution**. The agent decides; the protocol layer below it executes only when the mandate or token authorizes the execution. This architectural separation is the cleanest mitigation for the "rogue agent" failure mode.

### Layer 3 — Audit Trail and Settlement

Every agent transaction produces:

- **Pre-trade record** — the mandate or token in force, the agent ID, the principal ID, the merchant ID, the spending control state, the risk-rating at the moment of authorization
- **In-flight record** — the protocol-level handoffs (agent → mandate verifier → token issuer → merchant → settlement rail), each timestamped and cryptographically logged
- **Post-trade record** — settlement, reconciliation, dispute window opening, chargeback handling, post-settlement risk-monitoring touchpoints
- **Liability chain** — who is accountable at each step: the principal (for mandate scope), the developer / operator (for agent behavior within mandate), the network (for mandate enforcement and token integrity), the merchant (for accepting the token / mandate), the issuer (for KYA and the principal's KYC chain)

The 2026 regulatory posture is converging on **mandate + token + audit trail as the minimum standard**. A treasury or payments function that adopts agentic commerce without all three is operating outside the emerging baseline.

## Application Inside KRASA Skills

The pattern is a technique and a governance posture, not a skill on its own. It composes into existing KRASA skills as follows:

- **`skills/operations/cash-flow-forecaster-13-week.md`** — Treasury teams operating an agentic payment function need to model agent-initiated AP outflows differently from human-initiated outflows: mandate ceilings act as a hard ceiling on weekly outflow per agent; token expirations create scheduled cash-release moments; agent suspension events are scenario inputs for downside cash modeling. The 13-week grid should distinguish mandated-agent-outflows from human-approved-outflows.
- **`skills/operations/trade-lifecycle-tracker.md`** — Pre-trade compliance state now includes the mandate state and the agent identity. A restricted-customer event, an OFAC list update, or a sanctioned-counterparty surface must propagate to the mandate-verifier layer to suspend in-flight agent activity; trade-lifecycle records the mandate-state snapshot at trade time.
- **`skills/admin/sanctions-aml-alert-reviewer.md`** — Agent-initiated transactions raise new typology indicators (mandate-boundary-probing, multi-agent collusion patterns, token-recycling fraud, principal-binding spoofing). The skill's TM-alert disposition logic should treat agent-id as a screening dimension equivalent to counterparty BIC.
- **`skills/admin/aml-case-triage-evidence-dossier.md`** — Cross-system evidence assembly now extends to the protocol-layer audit trail (mandate-in-force, token state, agent-id, principal-binding). Dossiers for cases touching agentic-payment flows must include the protocol-layer records.
- **`skills/admin/kyc-cip-onboarding-workflow.md`** — Onboarding a corporate customer who deploys agents requires a KYA layer on top of standard KYC: agent inventory, mandate templates, developer vetting status, agent-lifecycle controls evidence. EDD triggers for high-mandate-ceiling agents or novel mandate scopes.
- **`skills/admin/aml-monitoring-tuner.md`** — TM rule library expanded with agent-specific typologies: mandate-boundary-probing, rapid-mandate-replacement, multi-agent-coordinated-structuring, token-recycling.
- **`skills/admin/trade-surveillance-reviewer.md`** — Surveillance rule set extended to flag agent-initiated trading patterns: mandate-window-clustering, sub-mandate-threshold structuring, cross-agent coordination indicative of front-running risk.
- **`skills/admin/regulatory-filing-checker.md`** — Form ADV disclosures, Marketing Rule materials, fund-prospectus disclosures, and pillar-3 disclosures should now include AI-agent-use disclosures consistent with SEC 2026 examination priorities (the "AI washing" concern). KYA-program existence is itself a disclosure-relevant fact.
- **`skills/operations/loan-covenant-compliance-monitor.md`** — Where a borrower deploys agentic-AI payment systems, covenant cushions for cash-position triggers should account for mandate ceilings as additional state variables; agent-deployment is a borrower-operational-risk attribute.
- **`skills/admin/ai-controls-auditor-icfr.md`** — ICFR scope expands to cover the mandate-issuance control, the token-issuance control, the agent-onboarding control, the mandate-revocation control. Each is a control under SOX §404 for any public-company entity operating agentic payments at material scale.

## Implementation Cautions

**Mandate scope discipline.** A mandate that is too broad is functionally equivalent to no mandate — it normalizes agent action without supervisory check. Mandate templates should default to narrow (specific merchant categories, low dollar ceilings, short time-windows) and broaden only on explicit principal review. A treasury function that issues "open-ended" mandates is one rogue agent away from a Wall Street Journal headline.

**Revocation and inflight handling.** When a mandate is revoked or an agent is suspended mid-transaction, the protocol layer must define what happens to inflight transactions. The default ("complete or roll back?") is decision-relevant for liability allocation. Skills that touch payment processing should model both states.

**Liability allocation is unsettled.** The Fenwick (May 2026) and Lexology analyses of the network-rules layer make clear that agent-driven chargebacks, agent-induced fraud, and agent-mandate-overrun events do not have settled liability allocation. Treasury and risk functions should write internal policy that defaults the principal as accountable until protocol-layer evidence demonstrates otherwise — and should monitor case law as it develops.

**Regulator-side governance is coming, not here.** The IMF Notes (2026/004) and academic literature (arXiv 2512.11933 "The Agentic Regulator") frame supervisory governance as a 2026–2028 build-out. Firms that wait for regulators to specify the discipline will discover the cost of retrofitting; firms that adopt internal KYA + mandate + token + audit discipline now create the records that future supervision will inspect.

**AI-washing exposure.** SEC 2026 examination priorities flag misleading disclosures about AI capabilities. A firm that markets "AI-driven treasury automation" without an operating KYA + mandate + token + audit-trail discipline is exposed under the disclosure prong as well as the controls prong. The two prongs reinforce each other.

**Cross-border friction.** Mandate enforceability is jurisdiction-specific. A mandate signed under U.S. law may not be enforceable under a counterparty's local law; a token issued by a U.S. issuer may face acceptance friction in a sanctioned-jurisdiction-adjacent counterparty. Treasury functions operating cross-border should map the jurisdictional posture per mandate template.

**Multi-agent collusion is a real failure mode.** Two agents deployed by independent principals can — in theory — coordinate against a third party (a merchant, an exchange, a counterparty) in ways neither principal individually authorized. The trade-surveillance and AML-typology libraries need to add multi-agent coordination as a typology before incidents force the addition.

**Prompt-injection is the central protocol-layer attack surface.** Red-team research on AP2 (arXiv 2601.22569, "Whispers of Wealth," January 2026) demonstrates that indirect prompt injection embedded in merchant-side content (product descriptions, catalog metadata, vendor onboarding text) can subvert an agent's ranking and routing logic with a 100% success rate in tested conditions, and that direct prompt injection at the credential-verification handshake can produce cross-account data exposure. Two attack patterns are already characterized in the literature: a **merchant-side ranking-subversion attack** (a malicious counterparty plants prompt-engineered text in a content field the agent reads, manipulating which merchant or counterparty the agent selects) and a **credential-handshake injection attack** (an injected instruction at the identity-verification step causes the agent to return another principal's authenticated context). Skills that compose with agentic-payment surfaces should treat every untrusted text field the agent reads — merchant descriptions, vendor metadata, invoice memo lines, payment-reference strings, sanctioned-counterparty narrative text, KYC document free-text fields — as a potential injection vector. Mitigations include: structural separation of trusted-instruction context from untrusted-data context (the AI-developer-side "data is not instructions" discipline), output validation against the mandate scope before any settlement instruction is emitted, mandatory four-eyes review for any agent action outside a pre-approved counterparty / merchant / amount band, monitoring for ranking-anomaly patterns (an agent suddenly preferring a previously-unranked merchant), and red-team rotation against the firm's own agent surfaces before production deployment. The KYA + mandate + token + audit-trail stack does not by itself defeat prompt injection — it constrains the blast radius of a successful injection but does not prevent the injection from succeeding.

**Books and records.** Mandates, tokens, agent-onboarding records, mandate-revocation records, and protocol-layer audit trails must be retained per the firm's regulatory recordkeeping discipline (Advisers Act Rule 204-2 for RIAs, SEC Rule 17a-4 for broker-dealers, BSA recordkeeping for banks, FDIC / OCC / Fed records-retention guidance). The retention layer should treat protocol-layer records as financial records, not as ephemeral application logs.

## Adoption Signals (2026)

- Mastercard Agent Suite launched January 2026; Visa Intelligent Commerce sandbox active in Q1–Q2 2026 with 100+ partners
- Google's AP2 partner roster expanded to 60+ partners including all major card networks and payment providers
- OpenAI / Stripe ACP shipped Shared Payment Token primitive in 2025–2026 timeframe
- IMF Notes 2026/004 frames agentic payments as critical digital infrastructure requiring supervisor governance
- SEC 2026 Examination Priorities highlight AI / automated-tools disclosure scrutiny — the "AI washing" enforcement risk
- ABA Banking Journal (May 2026) frames the agent era as "just beginning" with bank-side adoption expected in H2 2026 and 2027
- The Fenwick (May 2026) and Lexology analyses of network-rule liability allocation surface the unsettled-liability question as the central commercial-law issue of the rollout
- FIS + Anthropic Financial Crimes AI Agent (May 2026) — while not itself an agentic-payments deployment, signals the bank-side appetite for agentic-AI workflows that will eventually need KYA governance when extended from compliance-investigation to transaction-execution
- Fiserv agentOS (May 14 2026) ships an Agentic AML Triage Analysis agent as one of four first-party agents — the same surface that will carry payments-execution agents to widely-available status by August 2026 will carry the audit-trail discipline this note describes
- Whispers of Wealth red-team study (arXiv 2601.22569, January 2026) is the first published systematic prompt-injection evaluation of a deployed agent-payments protocol — operators should expect public-domain attack literature to accelerate through 2026–2027 in parallel with rollout

## Relationship to Adjacent KB Notes

- `agentic-finance-patterns.md` — covers the supervisor-specialist agent topology generically. This note specializes the topology to the payments-execution layer where mandate / token / audit-trail governance is the distinctive discipline.
- `multi-persona-ensemble-debate.md` — covers the persona-panel pattern for investment decisions. The KYA + mandate discipline is the execution-layer analogue: the persona panel produces a decision; the mandate / token layer produces an authorized execution.
- `financial-cot-prompting.md` — chain-of-thought reasoning is the technique inside the agent. KYA + mandate + token + audit-trail are the discipline outside the agent. Both are needed.
- `tools-ecosystem/anthropic-finance-stack-may-2026.md` — captures the Claude-side enterprise toolchain. Agentic payments compose into that toolchain when a Claude agent acts as the principal's authorized payment proxy.

## Anti-Plagiarism Note

This note documents a technique class and a governance posture observed across publicly-released protocols and frameworks (AP2 / ACP / Mastercard Agent Suite / Visa Intelligent Commerce / KYA). No code, prompt, protocol specification, or proprietary methodology has been copied. The three-layer framing (identity / authorization / audit-trail-and-settlement) is a paraphrase of the standard payment-system architecture as it has historically applied to humans, restated for autonomous agents; the underlying concepts predate any specific implementation. Regulatory citations (SEC 2026 Examination Priorities, IMF Notes 2026/004, FATF, BSA, Advisers Act Rule 204-2, SEC Rule 17a-4, SOX §404) reference public regulation and official-body guidance only. Vendor and protocol references (AP2, ACP, Mastercard, Visa, Stripe, Google, OpenAI, FIS, Fiserv, Anthropic) are factual landscape citations; no vendor documentation has been reproduced. The prompt-injection threat-model section paraphrases the published attack-class taxonomy from arXiv 2601.22569 ("Whispers of Wealth," January 2026); no attack code, prompt payload, or experimental dataset has been reproduced.
