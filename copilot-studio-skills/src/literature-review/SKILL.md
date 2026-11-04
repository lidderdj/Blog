---
name: literature-review
description: Conduct comprehensive, systematic literature reviews using multiple academic databases (PubMed, arXiv, bioRxiv, Semantic Scholar, etc.). This skill should be used when conducting systematic literature reviews, meta-analyses, research synthesis, or comprehensive literature searches across biomedical, scientific, and technical domains. Produces professionally formatted review documents with verified citations in multiple citation styles (APA, Nature, Vancouver, etc.).
---

# Literature Review

## Overview

Conduct systematic, comprehensive literature reviews following rigorous academic methodology. Search multiple literature databases, synthesize findings thematically, verify all citations for accuracy, and produce a professional, well-structured review document.

Use whatever literature search and web access is available to the agent (search connectors, web search, or files and database exports supplied by the user). Never invent studies, authors, results, or DOIs: every citation must come from a source actually retrieved or provided.

## When to Use This Skill

Use this skill when:
- Conducting a systematic literature review for research or publication
- Synthesizing current knowledge on a specific topic across multiple sources
- Performing meta-analysis or scoping reviews
- Writing the literature review section of a research paper or thesis
- Investigating the state of the art in a research domain
- Identifying research gaps and future directions
- Requiring verified citations and professional formatting

## Core Workflow

Literature reviews follow a structured, multi-phase workflow:

### Phase 1: Planning and Scoping

1. **Define Research Question**: Use PICO framework (Population, Intervention, Comparison, Outcome) for clinical/biomedical reviews
   - Example: "What is the efficacy of CRISPR-Cas9 (I) for treating sickle cell disease (P) compared to standard care (C)?"

2. **Establish Scope and Objectives**:
   - Define clear, specific research questions
   - Determine review type (narrative, systematic, scoping, meta-analysis)
   - Set boundaries (time period, geographic scope, study types)

3. **Develop Search Strategy**:
   - Identify 2-4 main concepts from research question
   - List synonyms, abbreviations, and related terms for each concept
   - Plan Boolean operators (AND, OR, NOT) to combine terms
   - Select minimum 3 complementary databases

4. **Set Inclusion/Exclusion Criteria**:
   - Date range (e.g., last 10 years: 2015-2024)
   - Language (typically English, or specify multilingual)
   - Publication types (peer-reviewed, preprints, reviews)
   - Study designs (RCTs, observational, in vitro, etc.)
   - Document all criteria clearly

### Phase 2: Systematic Literature Search

1. **Multi-Database Search**:

   Select databases appropriate for the domain:

   **Biomedical & Life Sciences:**
   - PubMed / PubMed Central for peer-reviewed biomedical literature
   - bioRxiv / medRxiv for preprints
   - ChEMBL, KEGG, UniProt and similar resources for domain data

   **General Scientific Literature:**
   - arXiv (preprints in physics, math, CS, q-bio)
   - Semantic Scholar or OpenAlex (200M+ papers, cross-disciplinary)
   - Google Scholar for comprehensive coverage

   **Specialized Databases:**
   - AlphaFold DB / PDB for protein structures
   - COSMIC for cancer genomics
   - Data Commons or national statistics portals for demographic/statistical data
   - Other specialized databases as appropriate for the domain

   If the agent cannot query a database directly, give the user the exact search string to run and ask them to share the exported results (RIS, BibTeX, CSV, or JSON).

2. **Document Search Parameters**:
   ```markdown
   ## Search Strategy

   ### Database: PubMed
   - **Date searched**: 2024-10-25
   - **Date range**: 2015-01-01 to 2024-10-25
   - **Search string**:
     ```
     ("CRISPR"[Title] OR "Cas9"[Title])
     AND ("sickle cell"[MeSH] OR "SCD"[Title/Abstract])
     AND 2015:2024[Publication Date]
     ```
   - **Results**: 247 articles
   ```

   Repeat for each database searched.

3. **Export and Aggregate Results**:
   - Collect results from each database with full metadata (title, authors, year, journal, DOI, abstract)
   - Combine all results into a single list or table
   - Record which database each record came from

### Phase 3: Screening and Selection

1. **Deduplication**:
   - Remove duplicates by DOI (primary) or normalized title (fallback)
   - Keep the record with the most complete metadata
   - Document number of duplicates removed

2. **Title Screening**:
   - Review all titles against inclusion/exclusion criteria
   - Exclude obviously irrelevant studies
   - Document number excluded at this stage

3. **Abstract Screening**:
   - Read abstracts of remaining studies
   - Apply inclusion/exclusion criteria rigorously
   - Document reasons for exclusion

4. **Full-Text Screening**:
   - Obtain full texts of remaining studies
   - Conduct detailed review against all criteria
   - Document specific reasons for exclusion
   - Record final number of included studies

5. **Create PRISMA Flow Diagram**:
   ```
   Initial search: n = X
   ├─ After deduplication: n = Y
   ├─ After title screening: n = Z
   ├─ After abstract screening: n = A
   └─ Included in review: n = B
   ```

### Phase 4: Data Extraction and Quality Assessment

1. **Extract Key Data** from each included study:
   - Study metadata (authors, year, journal, DOI)
   - Study design and methods
   - Sample size and population characteristics
   - Key findings and results
   - Limitations noted by authors
   - Funding sources and conflicts of interest

2. **Assess Study Quality**:
   - **For RCTs**: Use Cochrane Risk of Bias tool
   - **For observational studies**: Use Newcastle-Ottawa Scale
   - **For systematic reviews**: Use AMSTAR 2
   - Rate each study: High, Moderate, Low, or Very Low quality
   - Consider excluding very low-quality studies

3. **Organize by Themes**:
   - Identify 3-5 major themes across studies
   - Group studies by theme (studies may appear in multiple themes)
   - Note patterns, consensus, and controversies

### Phase 5: Synthesis and Analysis

1. **Create Review Document** using the structure in `assets/review_template.md`

2. **Write Thematic Synthesis** (NOT study-by-study summaries):
   - Organize Results section by themes or research questions
   - Synthesize findings across multiple studies within each theme
   - Compare and contrast different approaches and results
   - Identify consensus areas and points of controversy
   - Highlight the strongest evidence

   Example structure:
   ```markdown
   #### 3.3.1 Theme: CRISPR Delivery Methods

   Multiple delivery approaches have been investigated for therapeutic
   gene editing. Viral vectors (AAV) were used in 15 studies^1-15^ and
   showed high transduction efficiency (65-85%) but raised immunogenicity
   concerns^3,7,12^. In contrast, lipid nanoparticles demonstrated lower
   efficiency (40-60%) but improved safety profiles^16-23^.
   ```

3. **Critical Analysis**:
   - Evaluate methodological strengths and limitations across studies
   - Assess quality and consistency of evidence
   - Identify knowledge gaps and methodological gaps
   - Note areas requiring future research

4. **Write Discussion**:
   - Interpret findings in broader context
   - Discuss clinical, practical, or research implications
   - Acknowledge limitations of the review itself
   - Compare with previous reviews if applicable
   - Propose specific future research directions

### Phase 6: Citation Verification

**CRITICAL**: All citations must be verified for accuracy before final submission.

1. **Verify All DOIs**:
   - List every DOI cited in the document
   - Confirm each one resolves (https://doi.org/<DOI>) to the intended article, using CrossRef (https://api.crossref.org/works/<DOI>) or the publisher page where available
   - Compare the retrieved metadata (authors, title, journal, year, volume, pages) with the citation

2. **Report Verification Results**:
   - Flag any DOI that cannot be confirmed and state that it is unverified
   - Correct author names, titles, and publication details that do not match
   - Remove or replace any citation that cannot be traced to a real source
   - Repeat until every citation is confirmed

3. **Format Citations Consistently**:
   - Choose one citation style and use throughout (see `references/citation_styles.md`)
   - Common styles: APA, Nature, Vancouver, Chicago, IEEE
   - Format each reference from its verified metadata
   - Ensure in-text citations match reference list format

### Phase 7: Document Generation

1. **Produce the Final Document**:
   - Deliver the review as a well-structured document with headings, numbered sections, and an optional table of contents
   - Apply the chosen citation style (APA, Nature, Chicago, Vancouver, IEEE) consistently
   - If the user needs PDF or Word, produce it with the document tools available to the agent, or provide the content ready to paste into their editor

2. **Review Final Output**:
   - Check formatting and layout
   - Verify all sections are present
   - Ensure citations render correctly
   - Check that figures/tables appear properly
   - Verify table of contents is accurate

3. **Quality Checklist**:
   - [ ] All DOIs verified against CrossRef or doi.org
   - [ ] Citations formatted consistently
   - [ ] PRISMA flow diagram included (for systematic reviews)
   - [ ] Search methodology fully documented
   - [ ] Inclusion/exclusion criteria clearly stated
   - [ ] Results organized thematically (not study-by-study)
   - [ ] Quality assessment completed
   - [ ] Limitations acknowledged
   - [ ] References complete and accurate
   - [ ] Final document renders cleanly in the requested format

## Database-Specific Search Guidance

### PubMed / PubMed Central

Build complex queries with the PubMed Advanced Search Builder (https://pubmed.ncbi.nlm.nih.gov/advanced/).

**Search tips**:
- Use MeSH terms: `"sickle cell disease"[MeSH]`
- Field tags: `[Title]`, `[Title/Abstract]`, `[Author]`
- Date filters: `2020:2024[Publication Date]`
- Boolean operators: AND, OR, NOT
- See MeSH browser: https://meshb.nlm.nih.gov/search

### bioRxiv / medRxiv

Search at https://www.biorxiv.org and https://www.medrxiv.org.

**Important considerations**:
- Preprints are not peer-reviewed
- Verify findings with caution
- Check if preprint has been published (CrossRef)
- Note preprint version and date

### arXiv

Search at https://arxiv.org or via the arXiv API:
```text
# Example search categories:
# q-bio.QM (Quantitative Methods)
# q-bio.GN (Genomics)
# q-bio.MN (Molecular Networks)
# cs.LG (Machine Learning)
# stat.ML (Machine Learning Statistics)

# Search format: category AND terms
search_query = "cat:q-bio.QM AND ti:\"single cell sequencing\""
```

### Semantic Scholar

Search at https://www.semanticscholar.org (an API with a free tier is also available):
- 200M+ papers across all fields
- Excellent for cross-disciplinary searches
- Provides citation graphs and paper recommendations
- Use for finding highly influential papers

### Specialized Biomedical Databases

- **ChEMBL**: chemical bioactivity
- **UniProt**: protein information
- **KEGG**: pathways and genes
- **COSMIC**: cancer mutations
- **AlphaFold DB**: predicted protein structures
- **PDB**: experimental structures

### Citation Chaining

Expand search via citation networks:

1. **Forward citations** (papers citing key papers):
   - Use Google Scholar "Cited by"
   - Use Semantic Scholar or OpenAlex APIs
   - Identifies newer research building on seminal work

2. **Backward citations** (references from key papers):
   - Extract references from included papers
   - Identify highly cited foundational work
   - Find papers cited by multiple included studies

## Citation Style Guide

Detailed formatting guidelines are in `references/citation_styles.md`. Quick reference:

### APA (7th Edition)
- In-text: (Smith et al., 2023)
- Reference: Smith, J. D., Johnson, M. L., & Williams, K. R. (2023). Title. *Journal*, *22*(4), 301-318. https://doi.org/10.xxx/yyy

### Nature
- In-text: Superscript numbers^1,2^
- Reference: Smith, J. D., Johnson, M. L. & Williams, K. R. Title. *Nat. Rev. Drug Discov.* **22**, 301-318 (2023).

### Vancouver
- In-text: Superscript numbers^1,2^
- Reference: Smith JD, Johnson ML, Williams KR. Title. Nat Rev Drug Discov. 2023;22(4):301-18.

**Always verify citations** against CrossRef or doi.org before finalizing.

## Best Practices

### Search Strategy
1. **Use multiple databases** (minimum 3): Ensures comprehensive coverage
2. **Include preprint servers**: Captures latest unpublished findings
3. **Document everything**: Search strings, dates, result counts for reproducibility
4. **Test and refine**: Run pilot searches, review results, adjust search terms

### Screening and Selection
1. **Use clear criteria**: Document inclusion/exclusion criteria before screening
2. **Screen systematically**: Title → Abstract → Full text
3. **Document exclusions**: Record reasons for excluding studies
4. **Consider dual screening**: For systematic reviews, have two reviewers screen independently

### Synthesis
1. **Organize thematically**: Group by themes, NOT by individual studies
2. **Synthesize across studies**: Compare, contrast, identify patterns
3. **Be critical**: Evaluate quality and consistency of evidence
4. **Identify gaps**: Note what's missing or understudied

### Quality and Reproducibility
1. **Assess study quality**: Use appropriate quality assessment tools
2. **Verify all citations**: Confirm every DOI and its metadata
3. **Document methodology**: Provide enough detail for others to reproduce
4. **Follow guidelines**: Use PRISMA for systematic reviews

### Writing
1. **Be objective**: Present evidence fairly, acknowledge limitations
2. **Be systematic**: Follow structured template
3. **Be specific**: Include numbers, statistics, effect sizes where available
4. **Be clear**: Use clear headings, logical flow, thematic organization

## Common Pitfalls to Avoid

1. **Single database search**: Misses relevant papers; always search multiple databases
2. **No search documentation**: Makes review irreproducible; document all searches
3. **Study-by-study summary**: Lacks synthesis; organize thematically instead
4. **Unverified or invented citations**: Leads to errors; always verify every reference against a real source
5. **Too broad search**: Yields thousands of irrelevant results; refine with specific terms
6. **Too narrow search**: Misses relevant papers; include synonyms and related terms
7. **Ignoring preprints**: Misses latest findings; include bioRxiv, medRxiv, arXiv
8. **No quality assessment**: Treats all evidence equally; assess and report quality
9. **Publication bias**: Only positive results published; note potential bias
10. **Outdated search**: Field evolves rapidly; clearly state search date

## Example Workflow

Complete workflow for a biomedical literature review:

1. **Start from the template**: Use the section structure in `assets/review_template.md` for the CRISPR / sickle cell review.
2. **Search multiple databases**: PubMed and bioRxiv for biomedical work; arXiv and Semantic Scholar for computational work. Record each search string, date, and result count.
3. **Aggregate and process results**: Combine records, remove duplicates by DOI or title, restrict to 2015–2024, and rank by relevance or citation count.
4. **Screen and extract**: Screen titles, then abstracts, then full texts; extract key data and organize it by theme.
5. **Write the review** following the template structure:
   - Introduction with clear objectives
   - Detailed methodology section
   - Results organized thematically
   - Critical discussion
   - Clear conclusions
6. **Verify all citations**: Check every DOI and its metadata; fix or remove anything that fails, then re-check.
7. **Produce the final document** in the requested citation style (e.g., Nature) and format.
8. **Review the final output** for completeness and accuracy.

## Related Skills

This skill pairs well with these companion skills, if they are also installed:
- **scientific-critical-thinking**: Assess evidence quality and bias in included studies
- **statistical-analysis**: Interpret or pool effect sizes for meta-analysis
- **scientific-writing**: Polish the final manuscript
- **scientific-visualization**: Build PRISMA diagrams and summary figures

## Resources

### Bundled Resources

**References:**
- `references/citation_styles.md`: Detailed citation formatting guide (APA, Nature, Vancouver, Chicago, IEEE)
- `references/database_strategies.md`: Comprehensive database search strategies

**Assets:**
- `assets/review_template.md`: Complete literature review template with all sections

### External Resources

**Guidelines:**
- PRISMA (Systematic Reviews): http://www.prisma-statement.org/
- Cochrane Handbook: https://training.cochrane.org/handbook
- AMSTAR 2 (Review Quality): https://amstar.ca/

**Tools:**
- MeSH Browser: https://meshb.nlm.nih.gov/search
- PubMed Advanced Search: https://pubmed.ncbi.nlm.nih.gov/advanced/
- Boolean Search Guide: https://www.ncbi.nlm.nih.gov/books/NBK3827/

**Citation Styles:**
- APA Style: https://apastyle.apa.org/
- Nature Portfolio: https://www.nature.com/nature-portfolio/editorial-policies/reporting-standards
- NLM/Vancouver: https://www.nlm.nih.gov/bsd/uniform_requirements.html

## Summary

This literature-review skill provides:

1. **Systematic methodology** following academic best practices
2. **Multi-database search strategy** across biomedical and general sources
3. **Citation verification** ensuring accuracy and credibility
4. **Professional output** in a consistent citation style
5. **Comprehensive guidance** covering the entire review process
6. **Quality assurance** with verification and validation checklists
7. **Reproducibility** through detailed documentation requirements

Conduct thorough, rigorous literature reviews that meet academic standards and provide comprehensive synthesis of current knowledge in any domain.
