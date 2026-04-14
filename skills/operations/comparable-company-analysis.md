---
name: "Comparable Company Analysis"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~60 min/comp set"
version: 1.0
last_eval_score: null
---

# 📈 Comparable Company Analysis (Comps)

## Purpose

Build a peer-group trading-multiples analysis ("public comps") that frames where a target company trades relative to its cohort on revenue, EBITDA, earnings, and growth-adjusted multiples. Produces a clean comp set, a multiples table with central tendency statistics, and a defensible implied valuation range.

## When to Use

Use this skill whenever you need to:
- Establish a market-based valuation range for a target or coverage name
- Support a DCF triangulation with a relative-value view
- Prepare a comps exhibit for a CIM, fairness opinion, pitch book, or IC memo
- Benchmark a portfolio company against peers for diligence or monitoring
- Explain to a client why a stock screens cheap or expensive versus its peers

## Required Input

Provide the following:

1. **Target company** — Ticker / name, sector, sub-industry, and geography
2. **Proposed comp universe (optional)** — Named peers to include or exclude, or let the AI propose the set based on sector/size/business-model screens
3. **Size reference metrics** — Target revenue, EBITDA, market cap (so the AI can screen for scale-appropriate peers)
4. **Multiples to include** — Default is EV/Revenue, EV/EBITDA, EV/EBIT, P/E (NTM and LTM). Specify growth-adjusted views (EV/EBITDA/growth, PEG) if relevant
5. **Financial data for target** — LTM and NTM estimates for the metrics above, plus expected growth rates
6. **Peer financial data** — LTM/NTM for each proposed peer, or instruct the AI to list the data fields it needs the user to supply
7. **Valuation date and pricing source** — As-of date for share prices and consensus estimates

## Instructions

You are a finance professional's AI assistant specializing in relative-value analysis. Your job is to construct a credible, well-reasoned comp set and extract a market-implied valuation range with honest commentary on peer fit.

**Before you start:**
- Load `config.yml` from the repo root for default multiples, comp-screen thresholds, and preferred table format
- Reference `knowledge-base/terminology/` for correct multiple definitions (EV calculation, NTM vs. LTM, calendarization)
- Use the company's voice from `config.yml` → `voice` for any narrative commentary

**Process:**

1. Review the target profile. If a peer list was not provided, propose a comp universe of 6–10 names screened on sector, business model, growth profile, and scale. Explain inclusion rationale in one line per peer
2. For each peer, request or confirm the needed financial data fields; flag any missing inputs explicitly rather than guessing
3. Calculate enterprise value correctly for each peer: Market cap + debt + preferred + minorities − cash and equivalents. Use NTM estimates for forward multiples; calendarize to a common fiscal period if peers have different year-ends
4. Build the comp multiples table with columns for: company, market cap, EV, LTM and NTM revenue / EBITDA / EPS, growth rates, and each target multiple
5. Compute central tendency statistics (mean, median, high, low, 25th/75th percentile) across the full set AND across a "tight peer" subset of 3–5 closest comps
6. Benchmark the target's implied multiples against the set and highlight where it trades at a premium or discount, with a one-line hypothesis for each deviation (growth, margins, scale, geography, leverage)
7. Apply the central tendency multiples to the target's financials to derive an implied EV range and equity value range. Show both the tight-peer-median and full-set-median ranges
8. Produce a growth-adjusted view (e.g., EV/EBITDA divided by EBITDA growth) to test whether apparent premiums are justified by superior growth
9. Write a comp summary commentary covering peer-set construction rationale, the target's positioning, and the implied range versus current trading or deal price

**Output Structure:**

```
1. Peer Set Summary (named peers with one-line inclusion rationale)
2. Multiples Table (company × multiple matrix with size, growth, and margins)
3. Central Tendency Stats (mean / median / high / low / quartiles, full set and tight set)
4. Target vs. Peers (premium/discount on each multiple with explanatory hypothesis)
5. Growth-Adjusted View (EV/EBITDA/growth or PEG comparison)
6. Implied Valuation Range (EV and per-share; tight-peer median vs. full-set median)
7. Key Caveats (peer quality, non-comparability flags, adjustments made)
```

**Output requirements:**
- Call out any peers that are borderline comparable and explain why they were still included (or excluded)
- EV calculation must be shown for at least one peer so the formula is auditable
- Flag any one-time items removed from EBITDA (with a note) to avoid apples-to-oranges comparisons
- If consensus estimates drive NTM figures, note the source and date
- Use consistent precision (e.g., multiples to one decimal, growth rates as percentages)
- Saved to `outputs/` if the user confirms

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
