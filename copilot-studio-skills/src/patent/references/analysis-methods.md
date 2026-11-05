# Analysis Methods

Step-by-step methods for the three core deliverables: a prior-art search plan, an IP landscape map, and a patentability score. Apply them directly to the information the user provides. None of these methods queries a patent database; patents, prior art and inventor judgments come from the user or from searches the agent can actually run.

---

## 1. Prior-Art Search Plan

### Inputs
- Invention title
- Novel elements (the key features claimed as new)
- Keyword groups: each group is a concept plus its synonyms, e.g. `["neural network", "deep learning model"]`
- CPC/IPC classifications, e.g. `G06N3/04`, `H04L63/02`
- Target jurisdictions (US, EP, JP, CN, WO)
- Filing deadline (or priority date)
- Non-patent search areas, e.g. arXiv, GitHub, conference proceedings

### Build the queries
1. **Keyword query:** inside each synonym group, join terms with `OR` and put multi-word terms in quotes; wrap the group in parentheses. Join groups with `AND`.
   - Example: `("neural network" OR "deep learning model") AND (pruning OR sparsification)`
2. **Classification query:** join classifications with `OR`: `(CPC="G06N3/04" OR CPC="H04L63/02")`
3. **Primary query:** keyword query `AND` classification query. If no classifications were given, use the keyword query alone.

### Choose databases by jurisdiction
| Jurisdiction | Databases |
|--------------|-----------|
| US | USPTO Patent Public Search; Google Patents (US filter) |
| EP | EPO Espacenet; Google Patents (EP filter) |
| JP | JPO J-PlatPat; Espacenet |
| CN | CNIPA (English interface); Espacenet |
| WO | WIPO PATENTSCOPE |
| Global | Espacenet (worldwide); Google Patents (no filter) |

List each database once, even if several jurisdictions recommend it. Always include **Espacenet (worldwide)**. Default to US if no jurisdiction is given.

For every database, apply the primary query and a filter of **publication date ≤ filing deadline**.

### Non-patent and citation searches
- Add each non-patent area the user named; default to "academic literature" if none.
- **Citation walking:** from the top 3 hits in each database, follow backward and forward citations, and note any non-patent literature they cite.

### Search log template
Record one row per search:

| Date executed | Database | Query | Filters | Total hits | Relevant hits | Notes per relevant hit |
|---------------|----------|-------|---------|------------|---------------|------------------------|

### Warnings to raise
- No CPC/IPC classifications → keyword-only search misses art that uses different terms.
- No non-patent search areas → in software and biotech, non-patent literature is often more than half of the relevant art.
- Only one jurisdiction → international art (especially JP, CN, EP) may be missed.
- No filing deadline → search depth cannot be calibrated.

### Output layout
1. Title and filing deadline
2. Novel elements claimed
3. Patent searches: one subsection per database with its query (in a code block) and date filter
4. Non-patent searches
5. Citation walking
6. Warnings

---

## 2. IP Landscape Map

### Inputs
A landscape name, an "as of" date, and a list of patents. For each patent, capture as many of these as available: ID, title, assignee, filing date, publication date, issued (yes/no), classifications, primary classification, claim type (method, system, apparatus, composition, use, combination), independent claim count, citation count, family size, and status (active, expired, pending, abandoned).

### Analysis
1. **Summary:** total patents; counts of active, expired and pending.
2. **Filings by year:** count patents by the year of the filing date (use "unknown" when missing). List years in ascending order.
3. **Top assignees:** the 10 assignees with the most patents.
4. **Claim types:** count of patents per claim type.
5. **Heatmap:** a table with one row per primary classification and one column per assignee, showing patent counts in each cell.
6. **Crowded zones:** primary classifications with **5 or more** patents, sorted from most to fewest. These areas have high competitive density.
7. **White-space candidates:** primary classifications with **1–2** patents. These areas have thin coverage and may be open. Treat them as leads to verify, not conclusions: thin coverage in the sample may only mean the search missed patents there.

### Output layout
1. Landscape name and "as of" date
2. Summary counts
3. Filings by year
4. Top assignees
5. Claim types
6. Crowded zones
7. White-space candidates
8. Strategy implications (where to play, where to design around)

State clearly that the map reflects only the patents supplied and is current only as of its date.

---

## 3. Patentability Score

### Inputs
- Invention title and novel elements
- Closest prior art: for each reference, its name, publication date, and which novel elements it discloses
- Filing jurisdiction (default US)
- Subject-matter category: method, system, composition, software, diagnostic, business, other
- Would the invention be obvious to a person skilled in the art? (the user's judgment)
- Is it useful for its stated purpose?
- Does the specification enable a skilled person to make and use it?
- Evidence of commercial success: low, medium, high
- Evidence of long-felt need: low, medium, high

### Scoring rubric (100 points)

**Novelty (max 25)**
- No novel elements stated → 0 ("state the novel elements claimed")
- Any single reference discloses every novel element → 0, **anticipated**
- Every element is disclosed somewhere, but split across references → 8 ("check obviousness")
- Otherwise → 25 × (share of novel elements not disclosed in any reference), rounded down. Verdict "novel" if that share is at least 50%, otherwise "partly novel; review obviousness".

**Non-obviousness (max 25)**
- If judged obvious: start at 0; add 4 if long-felt need is high and 4 if commercial success is high. Verdict: "obvious (secondary considerations only partial)".
- If judged not obvious: start at 20; add 2 if long-felt need is medium or high and 3 if commercial success is medium or high (capped at 25). Verdict: "non-obvious".

**Utility (max 15)**
- Useful for stated purpose → 15; otherwise 0 (risk of §101 rejection).

**Subject-matter eligibility (max 20)**
| Category | Jurisdiction | Score | Note |
|----------|--------------|-------|------|
| Software | US | 8 | Alice/Mayo risk: must show a specific technical improvement, not just automate a known process |
| Software | Other (EU, UK, etc.) | 15 | Technical character required; usually achievable |
| Diagnostic | Any | 5 | High §101 risk in the US (Mayo); consider adding an actionable step |
| Business method | Any | 3 | High Alice risk; generally unpatentable |
| Anything else | Any | 18 | Likely eligible |

**Enablement (max 15)**
- Specification enables construction → 15; otherwise 5 ("add sufficient implementation detail").

### Recommendation
Compute the percentage: total ÷ 100.

| Condition | Recommendation |
|-----------|----------------|
| Novelty verdict is "anticipated" (whatever the other scores) | NOT PATENTABLE: invention is anticipated |
| ≥ 75% | STRONG: proceed with filing |
| 55–74.9% | MODERATE: address the weakest dimensions |
| 35–54.9% | WEAK: major revisions needed |
| < 35% | POOR: reconsider whether to file |

**Major risks:** list every dimension scoring below half its maximum, with its verdict.

### Output layout
1. Title, jurisdiction, total score, percentage, recommendation
2. Major risks
3. Table of dimensions: score, maximum, verdict
4. Notes for each dimension

Always state that the score is a structured research aid based on the user's inputs, not a legal opinion, and that a licensed patent attorney must give any formal patentability opinion.
