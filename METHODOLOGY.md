# Research Methodology and Provenance

This document records how the analysis in [README.md](README.md) and [PROPOSAL.md](PROPOSAL.md) was produced, including AI tool usage, prompts, sources, and editorial decisions. Its purpose is transparency and reproducibility.

## Why This Document Exists

There is no established standard for documenting AI-assisted research processes. Several partial frameworks exist:

| Framework | What it covers | What it misses |
|---|---|---|
| [PRISMA](https://www.prisma-statement.org/) | Systematic review reporting: search strategy, screening, inclusion criteria | Not designed for AI-assisted work or non-medical domains |
| [W3C PROV](https://www.w3.org/TR/prov-o/) | Provenance ontology: Entity, Activity, Agent relationships | Formal ontology — no lightweight documentation convention |
| [CRediT](https://credit.niso.org/) (ANSI/NISO Z39.104-2022) | 14-role taxonomy for contributor attribution in scholarly publications | No role for "AI tool" — assumes human authors only |
| Publisher AI guidelines ([Wiley](https://www.wiley.com/en-us/publish/article/ai-guidelines/), [Science](https://www.science.org/content/page/science-journals-editorial-policies)) | Require prompt disclosure, tool version, human review statement | Ad-hoc disclosure text, not a structured format |

This document borrows from all four to create a structured, reproducible record. The format described here is a proposal — not itself a standard yet.

---

## Process Overview

Adapted from [PRISMA 2020](https://www.prisma-statement.org/) flow structure, modified for AI-assisted policy research rather than medical systematic review.

```
Phase 1: SCOPING
  Input:  User-defined research question and candidate list
  Method: Human-specified candidates + AI web search for additional standards
  Output: 12 standards/initiatives identified

Phase 2: DATA COLLECTION
  Input:  URLs and names of candidates
  Method: AI web fetches of primary sources, specifications, GitHub repos,
          blog posts. Parallel retrieval of multiple sources.
  Output: Raw content from 30+ web sources

Phase 3: ANALYSIS
  Input:  Collected source material + user-defined evaluation criteria
  Method: AI synthesis with human-specified framing (procurement, supply
          chain, vendor credits, usage declarations)
  Output: Comparative evaluation (README.md)

Phase 4: PROPOSAL DEVELOPMENT
  Input:  Gap analysis from Phase 3 + user requirements
  Method: AI-drafted schema extensions with human iterative direction
          (5 prompt-response cycles refining scope and structure)
  Output: Concrete schema proposal (PROPOSAL.md)

Phase 5: REFINEMENT (2026-02-08)
  Input:  Completed README.md and PROPOSAL.md from Phases 1–4
  Method: Human-prompted gap analysis of the Goal section; AI identified
          a missing goal, human approved, AI updated both documents
  Output: New goal added (security and compliance assessment),
          PROPOSAL.md updated to connect Extension 2 to the new goal
```

---

## Tools and Models

| Component | Details |
|---|---|
| **AI model** | Claude Opus 4.6 (`claude-opus-4-6`) via Claude Code CLI |
| **Interface** | Claude Code VSCode extension |
| **Date of research** | 2026-02-07 (initial), 2026-02-08 (refinement) |
| **Knowledge cutoff** | May 2025 (supplemented by live web search) |
| **Web search** | Built-in web search and URL fetch tools |
| **Human role** | Problem framing, candidate selection, evaluation criteria, editorial direction, structural decisions, review of all outputs |

---

## Contributor Roles

Using the [CRediT taxonomy](https://credit.niso.org/contributor-roles-defined/) with an added `AI Tool` designation, following emerging publisher conventions:

| Role | Contributor |
|---|---|
| **Conceptualization** | Human |
| **Methodology** | Human (evaluation criteria, use cases) + AI (research structure) |
| **Investigation** | AI (web search, source retrieval, content extraction) |
| **Data curation** | AI (synthesis of sources) with human validation |
| **Formal analysis** | AI (comparative evaluation) with human editorial direction |
| **Writing – original draft** | AI |
| **Writing – review & editing** | Human (structural edits, framing corrections, scope refinement) |
| **Supervision** | Human |

---

## Research Prompts and Queries

Each phase is documented with the prompts/queries used and the sources they surfaced. This section is intentionally verbose to support reproducibility.

### Phase 1: Scoping

**Human input (initial prompt):**
> Evaluate the below candidates for their strength and weaknesses and chances at success to become a global standard: publiccode.yml, [OSS Taxonomy], [CodeMeta], [gov-codejson], [contribute.json]. Are there others?

The user also specified four evaluation dimensions: procurement discovery, vendor credit metadata, supply chain steering, and usage declarations.

### Phase 2: Data Collection

Queries were issued in parallel batches to maximize efficiency. Below is the full list.

#### Batch 1 — Primary candidate retrieval (parallel)

| Query / URL | Method | Purpose |
|---|---|---|
| `https://nesbitt.io/2025/11/29/oss-taxonomy.html` | Web fetch | OSS Taxonomy primary source |
| `https://codemeta.github.io/` | Web fetch | CodeMeta primary source |
| `https://github.com/DSACMS/gov-codejson` | Web fetch | gov-codejson primary source |
| `https://github.com/mozilla/contribute.json` | Web fetch | contribute.json primary source |
| `publiccode.yml standard software description open source public administration` | Web search | publiccode.yml background (user wrote "publicode.yml" — search resolved to publiccode.yml) |
| `opencode.de software index badges publiccode.yml Germany` | Web search | openCode.de badge system context |

#### Batch 2 — Schema details and adjacent standards (parallel)

| Query / URL | Method | Purpose |
|---|---|---|
| `publiccode.yml schema fields categories software type procurement metadata 2025 2026` | Web search | Full schema field inventory |
| `SPDX SBOM software bill of materials metadata standard open source` | Web search | SBOM standards context |
| `CycloneDX software metadata component identity open source` | Web search | CycloneDX context |
| `REUSE specification FSFE software metadata licensing standard` | Web search | REUSE spec context |
| `https://opencode.de/en/knowledge/software-index/badges-en` | Web fetch | Badge system details |

#### Batch 3 — Ecosystem and adoption context (parallel)

| Query / URL | Method | Purpose |
|---|---|---|
| `ecosyste.ms funds Andrew Nesbitt open source taxonomy CodeMeta integration` | Web search | ecosyste.ms ecosystem context |
| `"software heritage" CodeMeta metadata scholarly academic adoption` | Web search | CodeMeta adoption in academia |
| `https://yml.publiccode.tools/schema.core.html` | Web fetch | Full publiccode.yml schema |
| `EU open source solutions catalogue publiccode.yml 2025 OSOR interoperable europe` | Web search | EU adoption status |
| `Developers Italia software catalog publiccode.yml Italy adoption success` | Web search | Italian adoption history |

#### Batch 4 — Procurement and credit systems (parallel)

| Query / URL | Method | Purpose |
|---|---|---|
| `https://nesbitt.io/2026/01/28/the-dependency-layer-in-digital-sovereignty.html` | Web fetch | Digital sovereignty argument |
| `open source vendor marketplace credit system procurement public sector ecosystem metadata` | Web search | Procurement ecosystem landscape |
| `Drupal credit system marketplace vendors contribution attribution procurement` | Web search | Drupal credit system details |
| `https://github.com/ecosyste-ms/funds/issues/290` | Web fetch | Cross-ecosystem funding proposal |
| `https://github.com/ecosyste-ms/funds/issues/236` | Web fetch | Funder leaderboard proposal |
| `https://github.com/opensourcepledge/opensourcepledge.com/issues/418` | Web fetch | Contribution tracking discussion |

#### Batch 5 — Additional standards (parallel)

| Query / URL | Method | Purpose |
|---|---|---|
| `CITATION.cff software citation format GitHub adoption metadata` | Web search | CITATION.cff context |
| `OpenSSF scorecard security metadata open source supply chain` | Web search | OpenSSF Scorecard context |
| `schema.org SoftwareApplication SoftwareSourceCode metadata standard adoption` | Web search | schema.org software terms |

### Phase 4: Proposal Development — Additional Queries

| Query / URL | Method | Purpose |
|---|---|---|
| `https://yml.publiccode.tools/categories-list.html` | Web fetch | Full categories vocabulary (144 values retrieved) |
| `JSON-LD YAML representation yaml-ld specification linked data` | Web search | YAML-LD feasibility |
| `https://www.w3.org/community/reports/json-ld/CG-FINAL-yaml-ld-20231206/` | Web fetch | YAML-LD spec details |
| `Drupal marketplace API credit contribution data endpoint format` | Web search | Drupal API structure for credit registry API design |
| `https://www.drupal.org/drupalorg/blog/the-new-contribution-records-system` | Web fetch | Contribution records data model |
| `publiccode.yml scope allowed values list intendedAudience` | Web search | Scope vocabulary for migration mapping |
| `OpenSSF scorecard API URL format REST endpoint example` | Web search | Scorecard API URL patterns |
| `publicode French open source calculation rules engine` | Web search | Disambiguation: publicode vs publiccode |

### Phase 5: Refinement (2026-02-08)

**Human input (prompt 1):**
> Is there an additional goal not yet mentioned in the Goal section of the README.md that would make sense putting into the scope of this proposal?

The AI analyzed the four existing goals against the full document (gap analysis, recommendation, complementary standards) and identified that "security and compliance assessment" was extensively discussed as infrastructure (Extension 2, OpenSSF Scorecard, SBOM, REUSE) but never stated as an explicit goal. The AI distinguished this from existing goal 3 (supply chain steering) by framing it as the demand-side procurement perspective rather than the supply-side funding authority perspective.

**Human input (prompt 2):**
> OK add this new goal. Is there anything in the proposal that needs to be changed to account for this new goal?

The AI added goal 5 to README.md and identified two framing gaps in PROPOSAL.md where Extension 2's schema was already sufficient but the narrative didn't connect it to procurement assessment. No new schema extensions were needed — only the Procurement Office actor description and a new "What This Enables" section for Extension 2.

No web searches were performed in this phase — the analysis was based entirely on the existing documents.

**Human input (prompt 3):**
> There are various initiatives to create "digital sovereignty scores". Do those initiatives also fit into this proposal?

The user provided 6 URLs to sovereignty score initiatives: sovereigntyscore.io, Munich SDS (Heise article), EU Cloud Sovereignty Framework (Commission PDF), SUSE CSF Assessment, Nextcloud DSI (Robert Freund blog), and Bechtle Index (press release PDF).

#### Batch 6 — Digital sovereignty score initiatives (parallel)

| Query / URL | Method | Purpose |
|---|---|---|
| `https://sovereigntyscore.io/` | Web fetch | Sovereignty score tool — returned only a title ("European Digital Sovereignty Analysis"), no content |
| `https://www.heise.de/en/news/Munich-makes-digital-sovereignty-measurable-with-its-own-score-11164230.html` | Web fetch | Munich Digital Sovereignty Score details |
| `https://commission.europa.eu/document/download/09579818-64a6-4dd5-9577-446ab6219113_en` | Web fetch | EU Cloud Sovereignty Framework — PDF failed to parse |
| `https://www.suse.com/cloud-sovereignty-framework-assessment/` | Web fetch | SUSE CSF self-assessment tool |
| `https://www.robertfreund.de/blog/2025/08/15/digital-sovereignty-index-score/` | Web fetch | Nextcloud Digital Sovereignty Index |
| `https://www.bechtle.com/dam/jcr:32fae9a6-40fe-4c92-ac69-ae61a2d018a6/pm-bechtle-index-fuer-digitale-souveraenitaet-29092025-en.pdf` | Web fetch | Bechtle Index — PDF returned minimal content |

#### Batch 7 — Follow-up searches for sparse results (parallel)

| Query / URL | Method | Purpose |
|---|---|---|
| `sovereigntyscore.io European digital sovereignty analysis tool` | Web search | No results found for this domain |
| `Bechtle "index für digitale souveränität" digital sovereignty index 2025` | Web search | Bechtle Index dimensions: data sovereignty, tech independence, design freedom |
| `EU Commission "cloud sovereignty" framework assessment criteria SEAL rating 2025` | Web search | EU CSF 8 objectives, SEAL 0–4 rating, €180M tender |

#### Batch 8 — Detailed framework content (parallel)

| Query / URL | Method | Purpose |
|---|---|---|
| `https://www.heise.de/en/news/Cloud-Sovereignty-Framework-How-the-EU-will-assess-cloud-sovereignty-10922224.html` | Web fetch | EU CSF details and open source role |
| `https://www.connect-professional.de/netzwerke-it-infrastruktur/bechle-digital-souveraenitaet-403822.html` | Web fetch | Bechtle Index dimensions and open source connection |
| `site:sovereigntyscore.io OR "sovereignty score" European software assessment open source` | Web search | sovereigntyscore.io still not indexed |

#### Batch 9 — EU framework objectives and Nextcloud DSI (parallel)

| Query / URL | Method | Purpose |
|---|---|---|
| `https://www.safespring.com/blogg/2025/2025-11-the-eu-just-defined-sovereign-cloud-here-is-our-score/` | Web fetch | Full list of 8 SOV objectives; SOV-6 requires open source |
| `https://nextcloud.com/blog/digital-sovereignty-index-how-countries-compare-in-digital-independence/` | Web fetch | DSI methodology: 50 self-hosted tools, per-country deployment counts |
| `https://www.infoq.com/news/2025/11/eu-seal-framework-governance/` | Web fetch | SEAL governance trade-offs |
| `sovereigntyscore.io` | Web search | Final attempt — domain not found in any index |

The AI concluded that sovereignty score initiatives operate at a different level (organization/provider/country) than publiccode.yml (individual projects), but are *consumers* of the metadata publiccode.yml provides. Specifically, the EU CSF's SOV-5 (Supply Chain) and SOV-6 (Technology Sovereignty, requiring open source) directly need the kind of data the proposed extensions produce. The AI recommended adding these as context in the README rather than proposing new schema extensions.

**Human input (prompt 4):**
> Add the findings into the README as relevant context. Also create a new document PITCH.md with a 300-500 characters pitch each of the stakeholders in PROPOSAL.md using bullet points as much as possible.

The AI added the sovereignty score initiatives as a new "Digital Sovereignty Score Initiatives" subsection under "Other Relevant Standards and Initiatives" in README.md, with a table of 5 initiatives and framing text explaining why they are consumers of publiccode.yml metadata rather than competing standards. The AI also created PITCH.md with per-stakeholder pitches (one section per actor from the PROPOSAL.md Actors table). No web searches were performed — the analysis used previously retrieved research.

**Human input (prompt 5):**
> Make the pitch more based around economic and political goals. The current pitch focuses too much on the benefits around implementation without clarifying why implementation is even useful to them.

The AI rewrote all 8 stakeholder pitches in PITCH.md to lead with economic and political motivations (market access, contract competitiveness, procurement risk reduction, digital sovereignty compliance, funding efficiency, evidence-based policy) rather than technical implementation benefits. No web searches were performed.

---

## Sources Evaluated

All sources that informed the analysis, grouped by role.

### Primary sources (specifications and official documentation)

1. publiccode.yml specification — https://yml.publiccode.tools/schema.core.html
2. publiccode.yml categories list — https://yml.publiccode.tools/categories-list.html
3. publiccode.yml GitHub — https://github.com/publiccodeyml/publiccode.yml
4. CodeMeta project — https://codemeta.github.io/
5. OSS Taxonomy — https://nesbitt.io/2025/11/29/oss-taxonomy.html
6. gov-codejson — https://github.com/DSACMS/gov-codejson
7. contribute.json — https://github.com/mozilla/contribute.json
8. YAML-LD specification — https://www.w3.org/community/reports/json-ld/CG-FINAL-yaml-ld-20231206/
9. REUSE specification 3.3 — https://reuse.software/spec-3.3/
10. OpenSSF Scorecard — https://scorecard.dev/
11. Scorecard API — https://api.scorecard.dev/
12. CycloneDX specification — https://cyclonedx.org/specification/overview/
13. CITATION.cff — https://citation-file-format.github.io/
14. schema.org SoftwareSourceCode — https://schema.org/SoftwareSourceCode

### Institutional and adoption sources

15. EU OSS Catalogue — https://interoperable-europe.ec.europa.eu/eu-oss-catalogue
16. EU OSS Catalogue announcement — https://interoperable-europe.ec.europa.eu/collection/open-source-observatory-osor/news/eu-open-source-solutions-catalogue
17. publiccode.yml on Interoperable Europe Portal — https://interoperable-europe.ec.europa.eu/collection/open-source-observatory-osor/publiccodeyml-standard
18. Developers Italia — https://developers.italia.it/en/reuse/publication.html
19. openCode.de — https://opencode.de/en/sharing-and-developing-code
20. openCode.de badges — https://badges.opencode.de/en/
21. openCode.de publiccode.yml guide — https://open-code-guide-b6c70f.usercontent.opencode.de/en/softwareverzeichnis/publiccode.yml-anlegen/

### Context and analysis sources

22. The Dependency Layer in Digital Sovereignty (Nesbitt) — https://nesbitt.io/2026/01/28/the-dependency-layer-in-digital-sovereignty.html
23. Drupal contribution credit — https://www.drupal.org/drupalorg/contribution-credit
24. Drupal credit guide — https://www.drupal.org/drupalorg/blog/a-guide-to-issue-credits-and-the-drupal.org-marketplace
25. Drupal contribution records system — https://www.drupal.org/drupalorg/blog/the-new-contribution-records-system
26. Drupal REST APIs — https://www.drupal.org/drupalorg/docs/apis/rest-and-other-apis
27. CodeMeta/schema.org alignment discussion — https://github.com/codemeta/codemeta/issues/232
28. Software Heritage CodeMeta generator — https://www.softwareheritage.org/2024/08/30/codemeta-new-features/

### Discussion threads (user-provided)

29. Open Source Pledge #418 — https://github.com/opensourcepledge/opensourcepledge.com/issues/418
30. ecosyste.ms funds #290 — https://github.com/ecosyste-ms/funds/issues/290
31. ecosyste.ms funds #236 — https://github.com/ecosyste-ms/funds/issues/236

### Digital sovereignty score initiatives (Phase 5)

32. Munich Digital Sovereignty Score (Heise) — https://www.heise.de/en/news/Munich-makes-digital-sovereignty-measurable-with-its-own-score-11164230.html
33. EU Cloud Sovereignty Framework — https://commission.europa.eu/document/download/09579818-64a6-4dd5-9577-446ab6219113_en
34. SUSE Cloud Sovereignty Framework Assessment — https://www.suse.com/cloud-sovereignty-framework-assessment/
35. Nextcloud Digital Sovereignty Index — https://nextcloud.com/blog/digital-sovereignty-index-how-countries-compare-in-digital-independence/
36. Bechtle Index für digitale Souveränität — https://www.bechtle.com/ueber-bechtle/presse/pressemeldungen/2025/bechtle-entwickelt-index-fuer-digitale-souveraenitaet
37. Safespring EU CSF analysis (SOV-1 through SOV-8) — https://www.safespring.com/blogg/2025/2025-11-the-eu-just-defined-sovereign-cloud-here-is-our-score/
38. sovereigntyscore.io — https://sovereigntyscore.io/ (not indexed by search engines at time of research; only title "European Digital Sovereignty Analysis" retrieved)

### Sources consulted but not directly cited

39. Canada Open Resource Exchange — https://github.com/canada-ca/ore-ero
40. publicodes (French rules engine, disambiguation) — https://publi.codes/
41. FOSDEM 2022 publiccode.yml talk — https://archive.fosdem.org/2022/schedule/event/publiccodeyml/

---

## Editorial Decisions

Key decisions made by the human during the process:

1. **Candidate disambiguation.** The initial prompt listed "publicode.yml" (one c). AI search identified this as likely meaning "publiccode.yml" (two c's) and separately identified Publicodes (French rules engine) as a different project. Human confirmed the intended candidate was publiccode.yml.

2. **Evaluation framing.** The four use cases (procurement discovery, vendor credits, supply chain, usage declarations) were defined by the human. The AI did not add or remove evaluation dimensions.

3. **Recommendation direction.** The conclusion to build on publiccode.yml was reached by the AI based on evidence of adoption and institutional backing. The human reviewed and accepted this conclusion.

4. **Proposal scope.** The human directed which extensions to develop (faceted classification, SBOM, scorecard, credit registries, usage registries, JSON-LD) and specified the boundary between in-scope (schema pointers) and out-of-scope (credit data itself, usage data itself). The human specifically directed that credit and usage data are "too much in flux" for git-committed files.

5. **File structure.** The human directed that the proposal be a separate file (PROPOSAL.md) rather than appended to README.md.

6. **API outlines.** The Credit Registry API and Usage Registry API rough outlines were developed by the AI based on the Drupal Contribution Records system as a reference architecture, with human direction on scope boundaries.

7. **Removal of `usageRegistries` from publiccode.yml.** The initial AI draft included a `usageRegistries` field in publiccode.yml, mirroring `creditRegistries`. The human identified a fundamental asymmetry: projects have authority over credit endorsement (they know who contributes) but no authority over usage tracking (they don't control who uses them). Listing usage registries in a project file creates false gatekeeping. The AI then redesigned the architecture: usage registries are now fully decentralized, discovered via a companion Registry Discovery Standard rather than listed per-project. Credit registries remain in publiccode.yml because project endorsement is meaningful there.

8. **Addition of goal 5 (security and compliance assessment).** The human asked the AI to identify missing goals. The AI proposed "security and compliance assessment" as distinct from the existing "supply chain steering" goal — the former serves procurement officers evaluating specific software, the latter serves funding authorities steering ecosystem investment. The human accepted and directed the AI to update both README.md and PROPOSAL.md. The AI determined that no new schema fields were needed (Extension 2 already covered the technical mechanism) and limited changes to narrative framing: the Procurement Office actor description and a new "What This Enables" section for Extension 2.

9. **Digital sovereignty scores assessed as out-of-scope for schema extensions.** The human asked whether digital sovereignty score initiatives fit into the proposal, providing 6 URLs. The AI researched all six and concluded they operate at a different level (organization/provider/country) than publiccode.yml (individual projects). The AI identified them as *consumers* of publiccode.yml metadata rather than candidates for new schema fields — particularly the EU CSF's SOV-5 (Supply Chain) and SOV-6 (Technology Sovereignty, which requires open source). Decision on whether to add these to the README as contextual motivation is pending human direction.

---

## Limitations

- **Single-session research.** All data collection and analysis occurred in one session on 2026-02-07. No longitudinal tracking of standard evolution.
- **Web search bias.** Results are influenced by search engine ranking, recency bias, and the English language bias of queries.
- **No direct stakeholder input.** No interviews with publiccode.yml maintainers, openCode.de operators, or procurement officers.
- **AI synthesis limitations.** The AI model may have prior training knowledge that subtly influences synthesis beyond what the cited sources strictly support. The web search supplements but does not fully replace the model's training data.
- **No peer review.** This analysis has not been reviewed by domain experts.

---

## Reuse of This Format

This methodology document is itself experimental. If you adopt a similar approach, consider:

1. **Record prompts verbatim** when feasible, or summarize when prompts are conversational.
2. **Log all queries** — including failed or unhelpful ones — for honest reporting.
3. **Separate human decisions from AI outputs.** The "Editorial Decisions" section is critical for understanding what the AI would not have done on its own.
4. **Version the tools.** AI model versions matter for reproducibility. Record the exact model ID.
5. **Acknowledge limitations** including the inherent non-reproducibility of LLM outputs (same prompt may yield different results).

A future standard for this kind of documentation might combine:
- **PROV-O** vocabulary (Entity/Activity/Agent) for formal provenance chains
- **CRediT** roles for human-AI attribution
- **PRISMA-style** flow diagrams for the research process
- **Publisher AI disclosure** conventions for prompt/tool documentation

Until such a standard exists, a structured markdown document like this one is a pragmatic alternative.
