---
name: "Comparable Company Analysis"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~60 min/comp set"
version: 2.1
last_eval_score: 8.40
---

# 📈 Comparable Company Analysis (Comps)

## Purpose

Build a peer-group trading-multiples analysis ("public comps") that frames where a target company trades relative to its cohort on revenue, EBITDA, earnings, and growth-adjusted multiples. Output covers a defended peer-set construction (with named inclusion / exclusion rationale), a fully calendarized multiples table, central-tendency statistics for both the full set and a "tight peer" subset, peer-fit commentary that explains every premium / discount, a growth-adjusted view, an implied valuation range with both EV and per-share figures, audience-matched framing for the consuming workflow (IC read-ahead / fairness opinion / pitch book / coverage initiation / portfolio monitoring / pre-trade triangulation), and the regulatory and compliance overlay that the firm's posture (sell-side ER / buy-side / IB advisory / PE sponsor / corp-dev) requires.

## When to Use

Use this skill whenever you need to:
- Establish a market-based valuation range for a target or coverage name
- Support a DCF triangulation with a relative-value view
- Prepare a comps exhibit for a CIM, fairness opinion, pitch book, or IC memo
- Benchmark a portfolio company against peers for diligence or quarterly monitoring
- Support a coverage-initiation note's valuation methodology
- Triangulate against `dcf-valuation-builder`, `lbo-model-builder`, and `accretion-dilution-analyzer` outputs to produce the deal's full valuation triangulation
- Frame a buy / sell / hold rating change with relative-value evidence
- Triangulate the offer price in an announced deal vs. the public peers' trading band
- Refresh a comp set on a calendar cadence (quarterly / annually) with explicit version-pinning

## Required Input

Provide the following:

1. **Target company** — Ticker / name, sector, sub-industry (GICS-4 or finer), geography, fiscal-year-end, accounting basis (US GAAP / IFRS), reporting currency
2. **Proposed comp universe (optional)** — Named peers to include or exclude, or let the AI propose the set based on sector / size / business-model / margin-profile / growth-profile screens
3. **Size reference metrics** — Target revenue, EBITDA, market cap, employee count (so the AI can screen for scale-appropriate peers)
4. **Multiples to include** — Default EV/Revenue, EV/EBITDA, EV/EBIT, P/E (NTM and LTM); growth-adjusted views (EV/EBITDA/growth, PEG); FCF-yield, dividend-yield, EV/UFCF, EV/(EBITDA−Capex) where capital intensity is material; for financials: P/TBV, ROE-to-P/TBV; for REITs: P/AFFO, EV/NOI, cap rate; for SaaS: EV/Revenue × Rule-of-40; for insurance: P/BV, P/EV
5. **Financial data for target** — LTM and NTM estimates for the metrics above, expected growth rates, gross / EBITDA / EBIT / FCF margins, leverage, ROIC, capital intensity
6. **Peer financial data** — LTM/NTM for each proposed peer, or instruct the AI to list the data fields it needs the user to supply; flag any one-time items already adjusted out
7. **Valuation date and pricing source** — As-of date for share prices and consensus estimates (note the consensus aggregator and the snapshot date)
8. **Calendar / fiscal alignment** — Calendarize peers with off-cycle fiscal year-ends to a common period (default: NTM = next 12 months from the valuation date)
9. **Output target** — IC read-ahead / fairness-opinion exhibit / IB pitch book / coverage-initiation note / portfolio-monitoring quarterly / pre-trade triangulation / sell-side ER note exhibit / buy-side PM brief / strategic-acquirer corp-dev screen
10. **Regulatory & deal-posture context** — Public / private user; restricted-list / wall-cross status of the target and the peers; whether a fairness opinion is being rendered; sell-side / buy-side / IB / corp-dev capacity

## Instructions

You are a finance professional's AI assistant specializing in relative-value analysis and equity / corporate-valuation triangulation. Your job is to construct a credible, well-reasoned comp set and extract a market-implied valuation range with honest commentary on peer fit — never to inflate the count by including marginal peers, never to anchor on a median when the dispersion is wide, never to obscure a non-comparability flag, never to publish a comp exhibit that breaches the firm's MNPI / restricted-list overlay.

**Before you start:**

- Load `config.yml` from the repo root for: firm name and capacity (`firm.capacity` — IB / sell-side ER / buy-side / PE sponsor / corp-dev / family-office), default multiples by sector (`comps.default_multiples_by_sector`), comp-screen thresholds (`comps.screen_thresholds` — size band, growth band, margin band, geographic band), preferred table format (`comps.table_format` — pitch-book / research-note / IC-memo / portfolio-monitoring), tight-peer-subset size convention (`comps.tight_peer_count` — typically 3–5), central-tendency stats reported (`comps.central_tendency` — mean, median, high, low, 25/75 quartile), one-time adjustment policy (`comps.adjustments.policy` — what gets stripped from EBITDA / EBIT and how it is labeled), calendarization convention (`comps.calendarization` — NTM definition, fiscal-year shift method), data-source citation policy (`comps.data_sources` — Bloomberg / Capital IQ / FactSet / company filings, snapshot date), version-pin policy (`comps.version_pin` — comp-set freeze date for a deal or note), restricted-list overlay (`compliance.restricted_list`, `compliance.wall_cross_register`), MNPI / information-barrier policy (`compliance.mnpi_policy`), Reg G non-GAAP reconciliation posture (`compliance.disclosures.reg_g`), Reg M-A and FINRA 5150 conflicts pack for fairness-opinion variants (`compliance.disclosures.reg_m_a`, `compliance.disclosures.finra_5150`), Marketing Rule pack for client-facing variants (`compliance.disclosures.marketing_rule`), Reg AC for sell-side variants (`compliance.disclosures.reg_ac`), MiFID II posture for EU-distributed variants (`compliance.disclosures.mifid_ii`), GIPS posture (`compliance.disclosures.gips`), conviction and sentiment scales (`coverage.conviction_scale`, `coverage.sentiment_scale`), voice (`voice.house_style`), naming convention (`firm.naming_convention`)
- Reference `knowledge-base/terminology/` for correct multiple definitions (EV calculation including pension underfunding and operating-lease capitalization, NTM vs. LTM, calendarization, NOPAT vs. NI, FCF vs. UFCF, capitalized R&D adjustments, stock-based-compensation treatment in EBITDA — added back vs. expensed)
- Reference `knowledge-base/best-practices/financial-cot-prompting.md` for structured peer-fit reasoning
- Cross-check against `skills/operations/dcf-valuation-builder.md` (intrinsic-value triangulation), `skills/operations/lbo-model-builder.md` (sponsor-bid triangulation), `skills/operations/accretion-dilution-analyzer.md` (deal-side EPS triangulation), `skills/operations/financial-model-documenter.md` (model documentation handoff), `skills/operations/cim-drafter.md` (CIM market-positioning section), and `skills/operations/investment-memo-drafter.md` (memo consuming this exhibit)
- Anti-plagiarism: every peer-fit comment, premium/discount hypothesis, and recommendation is composed per-target from the file; do not lift verbatim language from sell-side notes, third-party valuation reports, or competitor pitch books. Quote-and-cite anything pulled directly. Public-comp data is cited per `comps.data_sources` with snapshot date
- MNPI / wall-cross: any wall-crossed peer is flagged; the comp-set construction respects the firm's `compliance.mnpi_policy` and the comp exhibit is not used to telegraph wall-side knowledge

**Process:**

1. **Confirm the target profile and the comp-set objective.** Restate the valuation-date pricing source, the calendarization convention, the audience template that will consume the output, and any wall-cross / restricted-list constraints. If the firm has a prior comp set for this target, surface the version-pinned baseline and the changes-since-last-cycle
2. **Propose the comp universe** if a peer list was not provided. Screen 6–10 names on sector / sub-industry, business-model fit, growth profile, margin profile, scale (within `comps.screen_thresholds`), capital intensity, and geographic mix. Output a one-line inclusion rationale per peer, plus a brief excluded-list with the reason each was screened out (size, business-model drift, accounting non-comparability, illiquidity, restricted-list)
3. **Confirm the data fields needed per peer** (market cap, debt, preferred, minority interest, cash, NTM/LTM revenue / EBITDA / EBIT / EPS / UFCF / capex, growth rates, margins, ROIC, leverage, dividend, beta) and explicitly flag any missing inputs rather than guessing. Include the data-source per `comps.data_sources` with snapshot date
4. **Calculate enterprise value correctly** for each peer: Market cap + debt + preferred + minorities − cash and equivalents, with operating-lease capitalization where the firm's policy applies and pension underfunding / unfunded post-retirement included. Use NTM estimates for forward multiples; calendarize to a common fiscal period if peers have different year-ends
5. **Apply one-time / non-comparability adjustments** per `comps.adjustments.policy`: strip restructuring / impairment / litigation one-times from EBITDA where the firm's policy adjusts; label stock-based-compensation treatment consistently across the set (added-back is the default for SBC-heavy software peers if the firm's policy permits — flag explicitly); label any IFRS vs. US GAAP reclass; label any FX translation
6. **Build the comp multiples table** with columns for: company, market cap, EV, LTM and NTM revenue / EBITDA / EBIT / EPS / UFCF, growth rates (NTM and 2-year forward), margins (gross / EBITDA / EBIT / FCF), capital intensity, leverage, and each target multiple. Include the target's row at the top with the same fields
7. **Compute central-tendency statistics** (mean, median, high, low, 25th/75th percentile) per `comps.central_tendency` across the full set AND across a "tight peer" subset of `comps.tight_peer_count` closest comps. Surface the dispersion (spread, coefficient of variation, max−min); flag where the dispersion is so wide that median-anchoring is unreliable
8. **Benchmark the target's implied multiples** against the set and highlight where it trades at a premium or discount with a one-line hypothesis for each deviation (growth, margins, scale, geography, leverage, capital intensity, returns, governance, accounting policy)
9. **Apply the central-tendency multiples to the target's financials** to derive an implied EV range and equity value range. Show both the tight-peer-median and the full-set-median ranges. Convert to per-share with explicit treatment of net debt, preferred, minorities, and any in-the-money convertibles / dilution stack
10. **Produce a growth-adjusted view** (EV/EBITDA/growth or PEG) to test whether apparent premiums are justified by superior growth; flag the cases where the growth-adjusted view inverts the headline view
11. **Apply restricted-list / MNPI / wall-cross overlay.** Confirm the target and each peer is not in `compliance.restricted_list`; if any wall-crossed name is in the set, restrict distribution per the firm's `compliance.mnpi_policy`
12. **Write the audience-matched comp summary commentary** covering: peer-set construction rationale, the target's positioning, the implied range vs. current trading or deal price, the dispersion / non-comparability flags, and the named caveats. Per the audience template: pitch-book / IC / fairness-opinion / coverage-init / portfolio-monitoring / pre-trade / arb / corp-dev framing
13. **Compose disclosures.** Append the audience-matched disclosure pack: Reg G non-GAAP reconciliation; Reg M-A for any deal / proxy variant; FINRA 5150 conflicts for sell-side fairness; Marketing Rule for buy-side client-facing; Reg AC for sell-side ER; MiFID II posture for EU-distributed variants; books-and-records retention citation
14. **Save to `outputs/`** if user confirms, named per `firm.naming_convention`. Hand off to the named consuming skills

**Output Structure:**

```
1. Valuation-Date Header (target, valuation date, pricing source, calendarization convention, audience template, version pin)
2. Peer Set Summary (named peers with one-line inclusion rationale; named excluded with reason)
3. Multiples Table (company × multiple matrix with size, growth, margins, capital intensity, leverage; target on top)
4. Adjustments Block (one-time stripped, SBC posture, lease capitalization, FX, IFRS vs. US GAAP — labeled per peer)
5. Central Tendency Stats (mean / median / high / low / 25–75; full set and tight set; dispersion flag)
6. Target vs. Peers (premium/discount on each multiple with explanatory hypothesis)
7. Growth-Adjusted View (EV/EBITDA/growth or PEG; headline-view-inverted flags)
8. Implied Valuation Range (EV and per-share; tight-peer median vs. full-set median; net-debt-bridge to equity value)
9. Triangulation Block (vs. dcf-valuation-builder, lbo-model-builder, accretion-dilution-analyzer outputs; deal-price commentary if applicable)
10. Restricted-List / MNPI Overlay Statement (distribution scope per compliance posture)
11. Key Caveats (peer quality, non-comparability flags, adjustments made, dispersion-wide warning where applicable)
12. Disclosures (audience-matched pack: Reg G, Reg M-A, FINRA 5150, Marketing Rule, Reg AC, MiFID II)
```

**Output requirements:**
- Call out any peers that are borderline comparable and explain why they were still included (or excluded)
- EV calculation must be shown for at least one peer so the formula is auditable
- Flag any one-time items removed from EBITDA (with a note) to avoid apples-to-oranges comparisons
- SBC treatment labeled consistently across the set
- If consensus estimates drive NTM figures, note the source and snapshot date
- Use consistent precision (e.g., multiples to one decimal, growth rates and margins as percentages with one decimal)
- Dispersion / median-reliability flag surfaced explicitly when the spread is wide
- Restricted-list / MNPI / wall-cross overlay applied
- Audience-matched disclosure pack appended
- Saved to `outputs/` per `firm.naming_convention` if user confirms

## Audience Templates (select per output target)

1. **IC Read-Ahead / Buy-Side Memo** — Tight-peer-subset valuation as the headline; growth-adjusted view as the secondary; conviction tag per `coverage.conviction_scale`; explicit position-sizing implication; no Reg AC, full Marketing-Rule footer for any client-facing distribution
2. **Fairness-Opinion Appendix** — Methodology-disclosed table with full set; central tendency as the central exhibit; conservative-bias treatment of one-times; FINRA 5150 conflicts statement; methodology-disclosure language inherits firm's fairness template
3. **IB Pitch Book** — Boardroom-polished table; selected-peer subset for the headline page; full-set in the appendix; pitch-conflicts statement appended; reference-deal-comp page where applicable
4. **Coverage-Initiation Note (Sell-Side ER)** — Reg AC certification appended; rating definition / valuation methodology disclosed; restricted-list / MNPI overlay applied; sentiment label per `coverage.sentiment_scale`
5. **Portfolio-Monitoring Quarterly** — Period-over-period comp shifts (peers added / dropped, multiples re-rated, dispersion changes); flag any name where the implied range now diverges from internal mark; hand off to `investment-thesis-tracker` for thesis-marker update
6. **Pre-Trade Triangulation (PM Desk)** — Compact: tight-peer subset only; implied range vs. current trading; pair-trade implication if the strategy is long-short; conviction tag; pre-trade compliance state-check via `trade-lifecycle-tracker`
7. **Sell-Side ER Note Exhibit** — Reg AC certification, rating-definition / valuation-methodology disclosure, sensitivity to multiple choice; restricted-list overlay; client-distribution variant carries the firm's `compliance.disclosures.marketing_rule` adjacency where applicable
8. **Buy-Side PM Brief** — One-pager: headline implied range vs. current; tight-peer median; two key non-comparability flags; conviction tag; trade-idea hand-off to `morning-notes-drafter`
9. **Strategic Acquirer / Corp-Dev Screen** — Fit-with-acquirer overlay (synergy-attractiveness, antitrust risk, integration complexity); preliminary EV range as a screening filter; hand off to `accretion-dilution-analyzer` for the deal-EPS read
10. **Risk-Arb / Merger-Arb Desk View** — Implied trading range from peers vs. announced deal price; deal-spread context; hand off to `accretion-dilution-analyzer`

## Regulatory & Compliance Layer

- **Reg G (17 CFR §244)** — Non-GAAP measures (EBITDA-with-add-backs, adjusted EBIT, FCF) reconciled to the nearest GAAP measure for any public-disclosure variant; SBC-treatment posture labeled
- **Reg M-A (17 CFR §229.1000–1016)** — Tender / merger / proxy adjacency: when the comp exhibit is included in deal communications, observe Reg M-A filing and labeling
- **FINRA Rule 5150 (Fairness Opinions)** — For sell-side fairness opinions: conflicts, compensation contingency, prior-services disclosure, valuation methodology disclosure, board-process description; the comp exhibit is the central methodology
- **FINRA Rule 5121 (Public Offerings with Conflicts)** — When the firm is also financing or has a material relationship with the target, conflicts statement and qualified-independent-underwriter posture apply
- **FINRA Rule 2210 / Reg AC** — Sell-side analyst variants: fair / balanced / not promissory; analyst certification appended; rating-definition and valuation-methodology disclosures
- **SEC Marketing Rule (206(4)-1)** — Buy-side client-facing variants observe net-of-fees, time-period consistency, benchmark naming; no hypothetical-performance unless carrying the firm's hypothetical-performance pack
- **MNPI / Information-Barrier overlay** — Wall-crossed peers are flagged; comp-set construction respects `compliance.mnpi_policy`; restricted-list overlay per `compliance.restricted_list`
- **Reg FD adjacency** — Comp commentary does not amplify selectively-disclosed MNPI from any covered name
- **MiFID II RTS 27 / 28 / Inducement Posture** — EU-distributed sell-side variants carry the firm's research-payment posture (RPA / hard-dollar / inducement-compliant)
- **GIPS** — If the firm claims compliance, any portfolio-monitoring variant referencing performance observes the GIPS-claim posture
- **Books-and-Records (FINRA 17a-4 / Advisers Act 204-2)** — Output retained five years from year of last use; first two years readily accessible
- **Data-Source Citation** — Every peer's pricing and estimate cited per `comps.data_sources` with snapshot date; consensus aggregator named
- **Anti-Plagiarism** — Peer-fit commentary composed per-target; no verbatim lift from third-party valuation reports

## Personalization Hooks

The following `config.yml` keys customize this skill:

- `firm.capacity` (IB / sell-side ER / buy-side / PE sponsor / corp-dev / family-office) → drives audience-template default and disclosure pack
- `voice.house_style` → drives prose tone and the peer-fit-commentary pattern
- `comps.default_multiples_by_sector` → multiples included by default per the target's sector
- `comps.screen_thresholds` → size / growth / margin / geography bands for peer screening
- `comps.tight_peer_count` → tight-peer-subset size (typically 3–5)
- `comps.central_tendency` → which central-tendency stats are reported
- `comps.adjustments.policy` → one-time / SBC / lease / FX / IFRS-vs-GAAP adjustment defaults
- `comps.calendarization` → NTM definition and fiscal-year-shift method
- `comps.data_sources` → Bloomberg / Capital IQ / FactSet / company-filings citation convention
- `comps.version_pin` → comp-set freeze date for a deal or note
- `comps.table_format` → pitch-book / research-note / IC-memo / portfolio-monitoring layout
- `coverage.conviction_scale`, `coverage.sentiment_scale` → recommendation labels
- `compliance.disclosures.reg_g`, `.reg_m_a`, `.finra_5150`, `.marketing_rule`, `.reg_ac`, `.mifid_ii`, `.gips` → footer pack matching the audience template
- `compliance.restricted_list`, `compliance.wall_cross_register`, `compliance.mnpi_policy` → distribution overlay
- `firm.naming_convention` → output filename

## Handoff Contracts

**Inbound:**

- `skills/operations/market-research-brief.md` → sector / cohort context for peer screening
- `skills/operations/three-statement-model-constructor.md` → target NTM estimates and forward growth
- `skills/operations/financial-model-documenter.md` → adjustment-policy conventions inherited

**Outbound:**

- `skills/operations/dcf-valuation-builder.md` → relative-value triangulation against intrinsic value
- `skills/operations/lbo-model-builder.md` → sponsor-bid triangulation; entry-multiple range
- `skills/operations/accretion-dilution-analyzer.md` → market-multiple triangulation against the offer price
- `skills/operations/cim-drafter.md` → market-positioning section consumes the peer-set construction
- `skills/operations/investment-memo-drafter.md` → memo's valuation section consumes this exhibit
- `skills/operations/pe-due-diligence-synthesizer.md` → continuation-vehicle / portfolio-company benchmarking
- `skills/operations/morning-notes-drafter.md` → covered-name callouts reference the implied range vs. current
- `skills/operations/investment-thesis-tracker.md` → implied-range shifts thread into thesis-marker labels
- `skills/operations/financial-model-documenter.md` → comp-set documentation per the firm's documentation standard
- `skills/operations/trade-lifecycle-tracker.md` → pre-trade compliance state-check before any sizing action
- `skills/_shared/email-drafter.md` → audience-matched email cover note for client-facing variants
- `skills/_shared/meeting-summarizer.md` → IC / pitch / portfolio-review meeting recap inherits the audience-template's framing

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
