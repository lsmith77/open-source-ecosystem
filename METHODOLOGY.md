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

Phase 6: RISK ANALYSIS (2026-02-08)
  Input:  All existing documents (README.md, PROPOSAL.md, PITCH.md,
          METHODOLOGY.md)
  Method: Human-prompted risk examination; AI performed systematic risk
          identification across 7 categories with likelihood/impact
          assessment and mitigations
  Output: RISK_ANALYSIS.md (28 risks, 5 critical)

Phase 7: ROADMAP (2026-02-09)
  Input:  All existing documents + human-provided information about
          existing allies, relationships, and institutional connections
  Method: Human described the coalition landscape (ZenDiS, BFH/CHopen,
          OSBA, Schleswig-Holstein, Sovereign Tech Agency, Drupal/PHP
          community, publiccode.yml maintainers, Andrew Nesbitt, CHAOSS).
          AI synthesized a phased implementation plan sequenced around
          these relationships. Multiple human correction cycles refined
          ally descriptions and eliminated structural duplication.
  Output: ROADMAP.md (6 phases over ~24 months, ally map, key players)

Phase 8: ORGANIZATION-LEVEL USAGE DECLARATIONS (2026-02-10)
  Input:  All existing documents + human proposal to use .well-known
          for tracking public-facing open source usage by organizations
  Method: Human proposed using .well-known on deploying organizations'
          domains to declare software usage, with three key insights:
          (1) the file should be maintained as part of deployment
          processes (not a separate bureaucratic artifact), (2) security
          risk is low given existing technology detection services like
          BuiltWith, (3) internal crawlers could scan and publish in
          bulk. AI analyzed the proposal's fit with existing architecture,
          identified risks, then updated all documents.
  Output: New subsection in PROPOSAL.md (Organization-Level Usage
          Declarations with schema, design rationale, intake mechanisms),
          updates to RISK_ANALYSIS.md (revised S5, new S6), ROADMAP.md
          (expanded Phase 4), PITCH.md (deploying org and maintainer
          pitches), and this methodology document

Phase 9: TEMPORAL FIELD DEPRECATION (2026-02-10)
  Input:  All existing documents + external feedback from Andrew Nesbitt
          (ecosyste.ms) via GitHub issue #2
  Method: Andrew Nesbitt provided empirical feedback based on indexing
          thousands of publiccode.yml files via ecosyste.ms: most files
          are out of date because temporal fields (softwareVersion,
          releaseDate, dependsOn versionMin) are not updated between
          releases. He proposed keeping only human-authored, slow-changing
          data: classification, contacts/maintenance, legal, and registry
          pointers. AI analyzed the feedback against existing documents,
          identified alignment with Design Principle 1, and integrated
          the feedback as a new extension and cross-cutting updates.
  Output: New Extension 5 in PROPOSAL.md (Deprecate Temporal Fields),
          new Design Principle 2 (slow-changing data only), updated full
          example, renumbered YAML-LD to Extension 6. New weakness in
          README.md, new recommendation item. New risk A5 in
          RISK_ANALYSIS.md (renumbered A5→A6 for classification
          complexity), updated critical risks. New pitch point in
          PITCH.md (maintainer). ROADMAP.md Phase 1 expanded to include
          temporal field deprecation alongside supply chain references.
          Updated this methodology document.

Phase 11: CREDIT SYSTEM REFERENCE ARCHITECTURE (2026-02-12)
  Input:  All existing documents + human-provided URLs for Drupal's
          contribution_records module and credit weight documentation
  Method: Human identified that the proposal referenced Drupal's credit
          system as inspiration but lacked detail on: (1) the
          contribution_records module being open-source, ticket-system-
          agnostic, and API-ready — reusable by other projects or as a
          SaaS, (2) the range of credit system complexity across
          projects (from Drupal's weighted system to GitHub Sponsors),
          (3) the credit stickiness property (credits stay with vendor
          after contributor changes employer), (4) an explicit scope
          boundary that specifying credit system details is beyond this
          proposal. AI fetched both URLs, distilled findings into
          PROPOSAL.md (Extension 3 intro, Credit Registry API design
          principles and scope boundary), PITCH.md (vendor and credit
          registry sections), and this methodology document.
  Output: Enhanced Extension 3 and Credit Registry API sections in
          PROPOSAL.md with reference architecture details and explicit
          scope boundary. New pitch points in PITCH.md for vendors
          (credit stickiness) and credit registries (reusable
          infrastructure). Updated this methodology document.

Phase 10: PUBLIC PROCUREMENT POLICY INTEGRATION (2026-02-11)
  Input:  All existing documents + 4 human-provided URLs on public
          procurement and open source policy
  Method: Human identified that the proposal lacked explicit connection
          to "Public Money, Public Code" principle, procurement criteria
          frameworks, and legislative models. Provided URLs to FSFE PMPC
          campaign, Dries Buytaert article on contribution-based
          procurement, Switzerland's EMBAG law (via OSOR), and OSBA
          procurement selection criteria. AI fetched all 4 URLs plus
          supplementary web search, then integrated findings across all
          documents.
  Output: New "Policy Context" section in PROPOSAL.md (PMPC framing,
          extension-to-procurement mapping table, OSBA criteria
          alignment, legislative models). New "Policy Maker / Legislator"
          actor in PROPOSAL.md and PITCH.md. New allies in ROADMAP.md
          (FSFE, Swiss Federal Chancellery, APELL, EuroStack).
          Procurement policy milestones woven into Phases 0, 2, 3, and
          5 of ROADMAP.md. Updated sequencing rationale with procurement
          policy track. Updated this methodology document.

Phase 12: HUMAN EDITORIAL PASS (2026-02-13)
  Input:  All existing documents
  Method: Human performed a comprehensive manual edit pass across
          PROPOSAL.md, ROADMAP.md, and RISK_ANALYSIS.md with two goals:
          (1) shorten documents by cutting implementation details —
          especially in the roadmap and risk sections — to avoid giving
          the impression that everything is already set in stone, and
          (2) reduce the number of specific terms proposed, since those
          will likely need to go through a governance process. The
          underlying motivation was to leave more space for stakeholders
          to affect the proposal and make it partially their own, to
          increase buy-in rather than presenting a top-down document.
  Output: Shorter, less prescriptive versions of PROPOSAL.md,
          ROADMAP.md, and RISK_ANALYSIS.md. No AI involvement — all
          edits were made directly by the human in the IDE.

Phase 13: METHODOLOGY REVIEW (2026-02-13)
  Input:  METHODOLOGY.md + external feedback from Andrew Nesbitt
          via Slack conversation
  Method: Andrew Nesbitt reviewed the methodology document and provided
          feedback: web search query strings are not useful (the search
          engine and results are not reproducible), but the individual
          URLs investigated are helpful as a reproducible research log
          and reference document. AI restructured the Research Prompts
          and Queries section accordingly.
  Output: Restructured research log: removed web search query strings,
          kept investigated URLs as the reproducible trail, added
          summary lines for web-search-discovered context. Sources
          Evaluated section retained as reference document.
```

---

## Tools and Models

| Component | Details |
|---|---|
| **AI model** | Claude Opus 4.6 (`claude-opus-4-6`) via Claude Code CLI |
| **Interface** | Claude Code VSCode extension |
| **Date of research** | 2026-02-07 (initial), 2026-02-08 (refinement), 2026-02-09 (roadmap), 2026-02-10 (usage declarations), 2026-02-11 (procurement policy), 2026-02-12 (credit system reference architecture), 2026-02-13 (human editorial pass, methodology review) |
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

Each phase is documented with the human prompts, the sources investigated, and key editorial decisions. Web searches were used to discover sources but the query strings are not recorded here as they are not reproducible (the search engine, ranking, and results vary). The investigated URLs — the reproducible research trail — are listed below and compiled in the [Sources Evaluated](#sources-evaluated) section as a reference document.

### Phase 1: Scoping

**Human input (initial prompt):**
> Evaluate the below candidates for their strength and weaknesses and chances at success to become a global standard: publiccode.yml, [OSS Taxonomy], [CodeMeta], [gov-codejson], [contribute.json]. Are there others?

The user also specified four evaluation dimensions: procurement discovery, vendor credit metadata, supply chain steering, and usage declarations.

### Phase 2: Data Collection

Sources were retrieved in parallel batches. Web searches supplemented user-provided URLs to discover additional sources.

#### Batch 1 — Primary candidate retrieval

| URL | Purpose |
|---|---|
| https://nesbitt.io/2025/11/29/oss-taxonomy.html | OSS Taxonomy primary source |
| https://codemeta.github.io/ | CodeMeta primary source |
| https://github.com/DSACMS/gov-codejson | gov-codejson primary source |
| https://github.com/mozilla/contribute.json | contribute.json primary source |

Additional sources discovered via web search: publiccode.yml background, openCode.de badge system context.

#### Batch 2 — Schema details and adjacent standards

| URL | Purpose |
|---|---|
| https://opencode.de/en/knowledge/software-index/badges-en | Badge system details |

Additional sources discovered via web search: publiccode.yml schema field inventory, SPDX/SBOM context, CycloneDX context, REUSE spec context.

#### Batch 3 — Ecosystem and adoption context

| URL | Purpose |
|---|---|
| https://yml.publiccode.tools/schema.core.html | Full publiccode.yml schema |

Additional sources discovered via web search: ecosyste.ms ecosystem context, CodeMeta adoption in academia, EU adoption status, Italian adoption history.

#### Batch 4 — Procurement and credit systems

| URL | Purpose |
|---|---|
| https://nesbitt.io/2026/01/28/the-dependency-layer-in-digital-sovereignty.html | Digital sovereignty argument |
| https://github.com/ecosyste-ms/funds/issues/290 | Cross-ecosystem funding proposal |
| https://github.com/ecosyste-ms/funds/issues/236 | Funder leaderboard proposal |
| https://github.com/opensourcepledge/opensourcepledge.com/issues/418 | Contribution tracking discussion |

Additional sources discovered via web search: procurement ecosystem landscape, Drupal credit system details.

#### Batch 5 — Additional standards

Sources discovered via web search: CITATION.cff context, OpenSSF Scorecard context, schema.org software terms.

### Phase 4: Proposal Development — Additional Sources

| URL | Purpose |
|---|---|
| https://yml.publiccode.tools/categories-list.html | Full categories vocabulary (144 values retrieved) |
| https://www.w3.org/community/reports/json-ld/CG-FINAL-yaml-ld-20231206/ | YAML-LD spec details |
| https://www.drupal.org/drupalorg/blog/the-new-contribution-records-system | Contribution records data model |

Additional sources discovered via web search: YAML-LD feasibility, Drupal API structure, scope vocabulary, Scorecard API patterns, publicode vs publiccode disambiguation.

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

#### Sources investigated — Digital sovereignty score initiatives

User-provided URLs and follow-up sources discovered via web search:

| URL | Purpose | Notes |
|---|---|---|
| https://sovereigntyscore.io/ | Sovereignty score tool | Only a title retrieved; not indexed by search engines |
| https://www.heise.de/en/news/Munich-makes-digital-sovereignty-measurable-with-its-own-score-11164230.html | Munich Digital Sovereignty Score | Details on scoring methodology |
| https://commission.europa.eu/document/download/09579818-64a6-4dd5-9577-446ab6219113_en | EU Cloud Sovereignty Framework | PDF failed to parse |
| https://www.suse.com/cloud-sovereignty-framework-assessment/ | SUSE CSF self-assessment tool | |
| https://www.robertfreund.de/blog/2025/08/15/digital-sovereignty-index-score/ | Nextcloud Digital Sovereignty Index | |
| https://www.bechtle.com/dam/jcr:32fae9a6-40fe-4c92-ac69-ae61a2d018a6/pm-bechtle-index-fuer-digitale-souveraenitaet-29092025-en.pdf | Bechtle Index | PDF returned minimal content |
| https://www.heise.de/en/news/Cloud-Sovereignty-Framework-How-the-EU-will-assess-cloud-sovereignty-10922224.html | EU CSF details and open source role | Follow-up for failed PDF |
| https://www.connect-professional.de/netzwerke-it-infrastruktur/bechle-digital-souveraenitaet-403822.html | Bechtle Index dimensions | Follow-up for failed PDF |
| https://www.safespring.com/blogg/2025/2025-11-the-eu-just-defined-sovereign-cloud-here-is-our-score/ | Full list of 8 SOV objectives; SOV-6 requires open source | |
| https://nextcloud.com/blog/digital-sovereignty-index-how-countries-compare-in-digital-independence/ | DSI methodology: 50 self-hosted tools, per-country deployment counts | |
| https://www.infoq.com/news/2025/11/eu-seal-framework-governance/ | SEAL governance trade-offs | |

Additional context discovered via web search: Bechtle Index dimensions, EU CSF objectives and SEAL rating system.

The AI concluded that sovereignty score initiatives operate at a different level (organization/provider/country) than publiccode.yml (individual projects), but are *consumers* of the metadata publiccode.yml provides. Specifically, the EU CSF's SOV-5 (Supply Chain) and SOV-6 (Technology Sovereignty, requiring open source) directly need the kind of data the proposed extensions produce. The AI recommended adding these as context in the README rather than proposing new schema extensions.

**Human input (prompt 4):**
> Add the findings into the README as relevant context. Also create a new document PITCH.md with a 300-500 characters pitch each of the stakeholders in PROPOSAL.md using bullet points as much as possible.

The AI added the sovereignty score initiatives as a new "Digital Sovereignty Score Initiatives" subsection under "Other Relevant Standards and Initiatives" in README.md, with a table of 5 initiatives and framing text explaining why they are consumers of publiccode.yml metadata rather than competing standards. The AI also created PITCH.md with per-stakeholder pitches (one section per actor from the PROPOSAL.md Actors table). No web searches were performed — the analysis used previously retrieved research.

**Human input (prompt 5):**
> Make the pitch more based around economic and political goals. The current pitch focuses too much on the benefits around implementation without clarifying why implementation is even useful to them.

The AI rewrote all 8 stakeholder pitches in PITCH.md to lead with economic and political motivations (market access, contract competitiveness, procurement risk reduction, digital sovereignty compliance, funding efficiency, evidence-based policy) rather than technical implementation benefits. No web searches were performed.

**Human input (prompt 6):**
> Examine the proposal for potential risks and create another document RISK_ANALYSIS.md

The AI read all four existing documents (README.md, PROPOSAL.md, PITCH.md, METHODOLOGY.md) and produced a systematic risk analysis identifying 28 risks across 7 categories: adoption (5), technical (5), security/trust (5), governance (3), political/jurisdictional (3), economic/sustainability (4), and dependency/external (4). Each risk was assessed for likelihood and impact with proposed mitigations. The analysis highlighted 5 critical risks (high likelihood + high impact): branding deterring non-government adoption, chicken-and-egg bootstrapping for registries, registry API interoperability failures, credit gaming, and split governance blocking progress. No web searches were performed — the analysis was based entirely on close reading of the existing documents and domain knowledge.

### Phase 7: Roadmap (2026-02-09)

**Human input (prompt 1):**
> While PROPOSAL contains the final solution and PITCH sells it to the stakeholders, there is the question on what steps to take in what order to reasonably achieve this vision. This obviously depends on which allies can be sold on the vision early on.

The human then described the existing coalition landscape:
- ZenDiS and Schleswig-Holstein (openCode.de) — likely willing to collaborate
- Switzerland's close collaboration with ZenDiS
- BFH owns ossdirectory.com, would benefit from the data sources
- Schleswig-Holstein and the OSBA have approached BFH
- Connections to the Sovereign Tech Agency
- Close relationship with Drupal and the PHP community
- Acquaintances with publiccode.yml maintainers

The human asked two questions: (1) what could be a sensible plan toward the proposal, and (2) what other key players would be necessary.

The AI read all existing documents (README.md, PROPOSAL.md, PITCH.md, RISK_ANALYSIS.md) and produced a phased implementation plan (ROADMAP.md) with 6 phases over ~24 months, sequenced to leverage existing relationships and address critical risks in order. The plan started with governance (the #1 risk from RISK_ANALYSIS.md), proceeded through schema-only extensions requiring no new infrastructure, then used Drupal's existing credit system to bootstrap the registry architecture, and deferred global scaling to the final phase. No web searches were performed.

**Human correction 1:**
The human corrected "BFH" to "BFH / CHopen" in the allies table, removed Switzerland as a separate ally (implicitly covered by ZenDiS connection), and trimmed some engagement pathway descriptions. These edits were made directly in the IDE.

**Human input (prompt 2):**
> There is a connection to Andrew Nesbitt and CHAOSS as well.

The AI added Andrew Nesbitt / ecosyste.ms and CHAOSS to the allies table. Both had also appeared in the "Additional Key Players Needed" section further down the document.

**Human input (prompt 3):**
> "Confirmed or Likely Allies" content is somewhat duplicate of "Additional Key Players Needed" and vice versa.

The AI restructured to eliminate duplication: the allies table became the single source for players with existing relationships (Andrew Nesbitt, CHAOSS, Drupal, publiccode.yml governance body), and "Additional Key Players Needed" was reduced to only players with no current relationship (Developers Italia, EU Commission, OpenSSF, Foundation for Public Code, and global-scale players).

**Human correction 2:**
The human simplified the CHAOSS description and adjusted the relationship status to "Direct contact." These edits were made directly in the IDE.

**Human input (prompt 4):**
> github.com/publiccodeyml is about the file spec, tooling and such, while github.com/publiccodenet is a Dutch foundation. publiccodenet also have a subproject called The Standard for Public Code. So the owner of the publiccode.yml standard is github.com/publiccodeyml. The foundation might be a relevant ally to work with but so far there is no connection to them.

The human provided a link to https://standard.publiccode.net/docs/review-template.html. The AI fetched this URL and confirmed that The Standard for Public Code is a broader framework (18 criteria for public software projects) that recommends publiccode.yml as one tool for meeting its findability criteria — it does not control or govern the publiccode.yml spec itself.

This corrected a misunderstanding present across all documents since Phase 2: the AI had treated the two GitHub organizations (publiccodeyml and publiccodenet) as a "split governance" problem (risk G1), when in fact they are separate entities with different scopes. The AI updated all documents:
- README.md: Reframed the weakness from "governance spread across multiple orgs" to "small governance community relative to institutional adoption"
- RISK_ANALYSIS.md: Reframed G1 from "split governance creates confusion" to "small spec governance community relative to institutional adoption," updated the risk matrix row and critical risks summary
- ROADMAP.md: Reframed the critical prerequisite from governance clarity to maintainer buy-in, updated Phase 0 actions to focus on capacity and support rather than determining which org is canonical, updated the Foundation for Public Code entry from "governance clarification" to "potential amplifier with no current connection"

No additional web searches were performed beyond the Standard for Public Code URL provided by the human.

### Phase 9: Temporal Field Deprecation (2026-02-10)

**External input:** Andrew Nesbitt (ecosyste.ms, OSS Taxonomy author) filed [GitHub issue #2](https://github.com/lsmith77/open-source-ecosystem/issues/2) with feedback based on his experience indexing thousands of publiccode.yml files via ecosyste.ms. His core argument: "across the thousands of files I index via ecosyste.ms, most are out of date because nobody remembers to update a metadata file between releases."

He proposed that publiccode.yml should exclude fields that become outdated quickly (version numbers, release dates, dependency version minimums) — data that already exists in authoritative sources like forge APIs and package registries — and retain only:
- **Classification** — project categorization unavailable elsewhere
- **Contacts and maintenance** — support contacts and arrangements
- **Legal** — license and copyright declarations
- **Registry pointers** — links to credits, supply chains, and SBOMs

**Human input (prompt 1):**
> Review the feedback from https://github.com/lsmith77/open-source-ecosystem/issues/2 and make relevant updates, while keeping METHODOLOGY.md updated.

The AI fetched the issue, analyzed the feedback against all existing documents, and identified that Andrew's proposal directly aligns with Design Principle 1 ("publiccode.yml points to external data, doesn't replicate it") but applies it more aggressively — to fields already in the current spec, not just proposed extensions. The AI also identified that the feedback strengthens the case for Extension 2 (Supply Chain References), since `supplyChain.sbom` becomes the authoritative replacement for `dependsOn[].versionMin`.

The AI updated all documents:
- **PROPOSAL.md:** Added Design Principle 2 ("only slow-changing, human-authored data"), renumbered subsequent principles. Added Extension 5 (Deprecate Temporal Fields) with schema, rationale, and the four-category framework from Andrew's feedback. Updated the full example to remove `softwareVersion`, `releaseDate`, and `dependsOn[].versionMin`. Renumbered YAML-LD from Extension 5 to Extension 6.
- **README.md:** Added "temporal fields cause widespread staleness" to publiccode.yml weaknesses with Andrew's empirical evidence. Added "deprecation of temporal fields" as recommendation item 5.
- **RISK_ANALYSIS.md:** Added A5 (temporal field staleness, High/High) with ecosyste.ms evidence. Renumbered faceted classification complexity from A5 to A6. Added A5 to critical risks summary. Updated A3 mitigation to cross-reference A5. Updated Extension 5→6 references for YAML-LD risk T4.
- **ROADMAP.md:** Expanded Phase 1 to include temporal field deprecation alongside supply chain references (they pair naturally — SBOM replaces `dependsOn.versionMin`). Updated risk reference A5→A6 for classification complexity. Updated Extension 5→6 reference for YAML-LD.

No web searches were performed beyond fetching the GitHub issue — the analysis was based on the existing documents and Andrew's domain expertise.

---

### Phase 8: Organization-Level Usage Declarations (2026-02-10)

**Human input (prompt 1):**
> For public facing use of open source, use could also be tracked via .well-known. It would also help in tracking what is no longer in use. Via the domain this could be mapped to organizations. Is this worth considering? What are the risks?

The AI analyzed the proposal's existing `.well-known` usage (for registry discovery at line 750 of PROPOSAL.md) and identified that the human's idea was architecturally distinct: deploying organizations publishing `/.well-known/publiccode-usage.json` on their own domains, rather than registries publishing registry manifests. The AI assessed the idea against the proposal's design principles and identified benefits (domain as identity, deprecation signals, low barrier, decentralization) and risks (security exposure, stale data, organizational complexity, adoption friction, IANA registration, biased visibility).

**Human input (prompt 2):**
> The file should be maintained as part of the deployment itself, ie. Drupal project would integrate such a file as part of its documented deployment processes. Security risk is fairly low given the reliability of services like BuiltsWith (for which there is unfortunately no open source alternative). There could even be crawlers that public organizations could use to scan internally and then publish in bulk via some defined mechanism for registries to index. Update the relevant documents with these findings, including the METHODOLOGY.md.

The human provided three key corrections/additions to the AI's initial analysis:
1. The file should be maintained as part of deployment processes, not as a separate IT operations task — projects like Drupal would integrate it into their deployment documentation
2. The security risk assessment should be lowered because technology detection services like BuiltWith already expose comparable information for public-facing applications
3. Internal scanning tools could inventory deployed software and publish results in bulk

The AI updated all documents:
- PROPOSAL.md: Added "Organization-Level Usage Declarations" subsection with schema, design rationale (deployment-integrated maintenance, domain as identity, no version details, explicit retirement), and three intake mechanisms (`.well-known` crawling, direct declaration, bulk import from internal scans). Updated the "Why Credit and Usage Are Architecturally Different" table with a "How data enters" row. Updated "What This Enables" with two new items.
- RISK_ANALYSIS.md: Revised S5 (privacy risks) to acknowledge BuiltWith precedent and lower likelihood from Medium to Low-Medium. Added S6 (stale `.well-known` files) as a new risk. Updated risk summary matrix.
- ROADMAP.md: Expanded Phase 4 with three new actions (`.well-known` spec, deployment integration with Drupal/Nextcloud, internal scan tool prototype) and updated deliverables.
- PITCH.md: Added deployment integration points to OSS Project/Maintainer and Deploying Organization pitches.

**Human input (prompt 3):**
> In the proposal also mention that tooling could be created that would help organizations crawl both especially their internally (and publically) deployed tools that each publish their own .well-known publiccode-usage.json to build the public facing publiccode-usage.json that serves as the public gateway to this information. This way risk of stale data is minimized and both internal and public facing open source use could be declared.

The human refined the architecture from a single organizational file to a two-tier model: individual deployments (both internal and public-facing) each publish their own per-deployment `/.well-known/publiccode-usage.json`, and an organizational aggregation tool crawls these to assemble the public-facing file on the organization's primary domain. This addresses the stale data risk (S6) more fundamentally — data is maintained at the source by deployment processes, and the aggregation tool serves as both an assembly mechanism and an audit tool.

The AI updated PROPOSAL.md (added "Two-tier aggregation" design rationale, restructured intake mechanisms to distinguish per-deployment aggregation from legacy scanning), RISK_ANALYSIS.md (updated S6 likelihood and mitigation to reference the two-tier model), and this methodology document.

No web searches were performed — the analysis was based on the existing documents and the human's domain knowledge about BuiltWith and deployment processes.

---

### Phase 10: Public Procurement Policy Integration (2026-02-11)

**Human input (prompt 1):**
The human identified that the proposal lacked explicit connection to the "Public Money, Public Code" principle and public procurement policy. They provided four URLs and directed the AI to incorporate these aspects and identify relevant partners and milestones:

1. https://publiccode.eu/ — FSFE's Public Money? Public Code! campaign
2. https://dri.es/funding-open-source-for-digital-sovereignty — Dries Buytaert on contribution-based procurement
3. https://interoperable-europe.ec.europa.eu/collection/open-source-observatory-osor/news/new-open-source-law-switzerland — Switzerland's EMBAG law
4. https://osb-alliance.de/publikationen/veroeffentlichungen/selection-criteria-for-the-sustainable-procurement-of-open-source-software — OSBA procurement selection criteria

#### Sources investigated — Public procurement policy

| URL | Purpose | Notes |
|---|---|---|
| https://publiccode.eu/ | PMPC campaign principles | Fetch failed, content not extractable |
| https://dri.es/funding-open-source-for-digital-sovereignty | Contribution-based procurement argument | Drupal credit system as procurement model |
| https://interoperable-europe.ec.europa.eu/collection/open-source-observatory-osor/news/new-open-source-law-switzerland | EMBAG law details | Mandatory source code disclosure, Article 9, Prof. Stürmer quote |
| https://osb-alliance.de/publikationen/veroeffentlichungen/selection-criteria-for-the-sustainable-procurement-of-open-source-software | OSBA procurement criteria | Community relationship, upstream publication, Level 3 support, supply chain security |

Additional context discovered via web search: PMPC campaign details (200+ orgs, 31,000+ signatories).

The AI synthesized findings across all sources and identified:
- The PMPC principle creates demand for the extensions: once governments commit to open source, they need the discovery/evaluation infrastructure this proposal provides
- The OSBA's four procurement criteria map directly to specific extensions (creditRegistries → criterion 1 and 2, maintenance + credits → criterion 3, supplyChain → criterion 4)
- Switzerland's EMBAG demonstrates that legislation works but reveals a gap: mandating release is not sufficient without procurement infrastructure
- APELL and EuroStack (mentioned in the Dries article) are advancing EU-level policy that needs this metadata as evidence infrastructure

**Human input (prompt 2):**
The human directed that APELL and EuroStack be listed as separate allies (not combined), noting acquaintances in both initiatives.

**Human input (prompt 3):**
> Should new stakeholders be added? Update the PITCH and METHODOLOGY documents

The AI identified that "Policy Maker / Legislator" is a distinct actor from "Federal Authority / Funder" — legislators craft laws and procurement regulations (EMBAG, Buy Open Source Act), while funders allocate money (Sovereign Tech Fund, CISA). The AI added the new actor to PROPOSAL.md and a corresponding pitch section to PITCH.md.

### Phase 11: Credit System Reference Architecture (2026-02-12)

**Human input (prompt 1):**
The human identified that the proposal referenced Drupal's credit system as inspiration but lacked important details. They provided two URLs and the following context:
- Drupal's `contribution_records` module is architecturally ticket-system-agnostic and has an API — it could be adopted by other OSS projects or operated as a SaaS
- Drupal has documented their rating system in detail (credit weights, contribution types, marketplace ranking)
- Some projects might prefer simpler metrics (GitHub Sponsors, code contributor mapping)
- A critical feature of Drupal's credit system is that credits attributed to a vendor through a contributor remain with that vendor even after the contributor changes employer
- Specifying the details of a credit system is beyond the scope of this proposal

#### Sources investigated — Credit system reference architecture

| URL | Purpose | Notes |
|---|---|---|
| https://git.drupalcode.org/project/contribution_records | contribution_records module | Open-source, 361 commits, ticket-system-agnostic |
| https://www.drupal.org/drupalorg/docs/marketplace/contribution-credit-weight-and-impact-on-ranking | Credit weight and ranking details | Issue credits scaled by project usage, case studies, org membership, event sponsorships |

The AI distilled the critical aspects into three documents:
- **PROPOSAL.md:** Added a new paragraph in Extension 3 intro on the spectrum of credit system complexity (from Drupal's weighted system to GitHub Sponsors), with an explicit scope boundary. Enhanced Credit Registry API Design Principle 4 with the `contribution_records` module's reusability. Added new "Attribution policies" bullet in "What This Doesn't Standardize" covering credit stickiness.
- **PITCH.md:** Added "Protect your investment in contributions" (credit stickiness) to Vendor pitch. Added "Build on proven infrastructure" (reusable `contribution_records` module) to Credit Registry pitch.
- **METHODOLOGY.md:** This phase entry and new sources.

Documents updated:
- PROPOSAL.md: New "Policy Context: Public Procurement and Open Source" section with extension-to-procurement mapping table, OSBA criteria alignment, and legislative models subsection. Updated table of contents. New "Policy Maker / Legislator" actor in Actors table.
- PITCH.md: New "Policy Maker / Legislator" section with four bullet points on enforceable criteria, legislative models, sovereignty, and level playing field.
- ROADMAP.md: New allies (FSFE, Swiss Federal Chancellery, APELL, EuroStack). Removed FSFE duplicate from Phase 5 table. New procurement policy actions in Phases 0, 2, 3, and 5. Updated sequencing rationale with procurement policy track description.
- METHODOLOGY.md: This phase entry, new sources, updated research date.

### Phase 12: Human Editorial Pass (2026-02-13)

The human performed a comprehensive manual edit pass across PROPOSAL.md, ROADMAP.md, and RISK_ANALYSIS.md. No AI involvement — all edits were made directly in the IDE. The goals were:

1. **Shorten documents** by cutting implementation details, especially in the roadmap and risk sections, to avoid giving the impression that everything is already decided.
2. **Reduce proposed terminology** — specific field names and terms were trimmed back, since these will likely need to go through a governance process that the human did not want to preclude.
3. **Leave space for stakeholders** — the underlying motivation was to make the documents less prescriptive so that collaborators can affect the proposal and make it partially their own. A top-down document discourages buy-in; a proposal with room for discussion invites it. The shape of the final result will depend on which stakeholders sign on and what expertise, influence, and time they bring.

### Phase 13: Methodology Review (2026-02-13)

**External input:** Andrew Nesbitt reviewed this methodology document and provided feedback via Slack. His assessment: the web search query strings are not useful as a reproducibility aid because the search engine, ranking, and results are opaque and non-reproducible. However, the individual URLs that were actually investigated are helpful — they form a reproducible research log and can serve as a reference document to refer back to later.

The AI restructured the "Research Prompts and Queries" section accordingly: web search query strings were removed from the batch tables, investigated URLs were retained as the reproducible trail, and brief summary lines note what additional context was discovered via web search without recording the non-reproducible queries. The "Sources Evaluated" section was retained as the compiled reference document.

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

### External feedback (Phase 9)

39. GitHub issue #2: Remove temporal fields — https://github.com/lsmith77/open-source-ecosystem/issues/2

### Governance clarification (Phase 7)

40. The Standard for Public Code — https://standard.publiccode.net/docs/review-template.html

### Public procurement policy sources (Phase 10)

41. Public Money? Public Code! (FSFE) — https://publiccode.eu/
42. Funding Open Source for Digital Sovereignty (Dries Buytaert) — https://dri.es/funding-open-source-for-digital-sovereignty
43. Switzerland's EMBAG open source law (OSOR) — https://interoperable-europe.ec.europa.eu/collection/open-source-observatory-osor/news/new-open-source-law-switzerland
44. OSBA Selection Criteria for Sustainable Procurement of OSS — https://osb-alliance.de/publikationen/veroeffentlichungen/selection-criteria-for-the-sustainable-procurement-of-open-source-software
45. APELL initiative — https://apell.info/
46. EuroStack coalition — https://eurostack.eu/
47. Switzerland EMBAG law text — https://www.fedlex.admin.ch/eli/cc/2023/682/en

### Credit system reference architecture (Phase 11)

48. Drupal contribution_records module — https://git.drupalcode.org/project/contribution_records
49. Drupal contribution credit weight and impact on ranking — https://www.drupal.org/drupalorg/docs/marketplace/contribution-credit-weight-and-impact-on-ranking

### Sources consulted but not directly cited

50. Canada Open Resource Exchange — https://github.com/canada-ca/ore-ero
51. publicodes (French rules engine, disambiguation) — https://publi.codes/
52. FOSDEM 2022 publiccode.yml talk — https://archive.fosdem.org/2022/schedule/event/publiccodeyml/

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

10. **Risk analysis as a separate document.** The human directed the AI to examine the proposal for risks and create a dedicated RISK_ANALYSIS.md. The AI chose to organize risks into 7 categories (adoption, technical, security/trust, governance, political, economic, dependency) with per-risk likelihood/impact assessment and mitigations — a structure not specified by the human but consistent with standard risk analysis practice. The AI identified 5 critical risks and flagged governance (split governance blocking progress) as the highest-priority prerequisite.

11. **Roadmap sequencing driven by relationships, not technical complexity.** The human framed the roadmap question around existing allies rather than asking for a technically optimal sequence. The AI adopted this framing: phases are ordered by which allies can be activated and what they can deliver, not by logical dependency between extensions. For example, supply chain references (Extension 2) comes before faceted classification (Extension 1) because ZenDiS/openCode.de can implement it immediately with their existing badge system, even though classification is arguably more impactful.

12. **Allies table as single source of truth for relationships.** The human identified duplication between the "Confirmed or Likely Allies" section and "Additional Key Players Needed." The AI restructured so that all players with existing relationships appear only in the allies table, and the key players section is limited to players requiring new outreach. This was a structural decision by the human; the AI had originally listed some players in both places with different framing.

13. **Institutional corrections by human.** The human corrected "BFH" to "BFH / CHopen," removed Switzerland as a standalone ally, and added Andrew Nesbitt and CHAOSS as direct connections. These corrections reflected relationship knowledge the AI did not have and could not have inferred from the existing documents.

14. **Governance clarification by human.** The AI had treated the two GitHub organizations (publiccodeyml and publiccodenet) as a "split governance" problem since the initial analysis. The human clarified that publiccodeyml owns the spec and tooling, while publiccodenet is the Foundation for Public Code — a separate Dutch foundation maintaining "The Standard for Public Code," a broader framework that recommends publiccode.yml as one tool. These are not competing governance bodies. The AI corrected this misunderstanding across all documents: the risk was reframed from "split governance" to "small governance community relative to institutional adoption," and the Foundation for Public Code was repositioned as a potential amplifier (no current connection) rather than a governance blocker.

15. **Organization-level usage declarations via `.well-known` proposed by human.** The human proposed that deploying organizations publish `/.well-known/publiccode-usage.json` on their domains to declare software usage. The AI initially assessed security exposure as a significant risk; the human corrected this by noting that technology detection services like BuiltWith already expose comparable data, and that the schema deliberately excludes version numbers. The human also reframed maintenance from an IT operations task to a deployment-integrated process — projects like Drupal would include usage declaration updates in their deployment documentation. The human additionally proposed internal scanning tools as a bulk publishing mechanism. All three insights were incorporated across the documents.

16. **Temporal field deprecation driven by external feedback.** Andrew Nesbitt (ecosyste.ms) filed GitHub issue #2 with empirical evidence that most publiccode.yml files are stale because temporal fields aren't maintained between releases. The AI identified this as a strengthening of Design Principle 1 and proposed Extension 5 to deprecate `softwareVersion`, `releaseDate`, and `dependsOn[].versionMin`. The human directed the AI to integrate the feedback across all documents. No new schema fields were needed — only deprecations and the corresponding updates to the full example, risk analysis, pitches, and roadmap.

17. **Two-tier aggregation model proposed by human.** The human refined the `.well-known` architecture from a single organizational file to a two-tier model: individual deployments (both internal and public-facing) each publish their own per-deployment `/.well-known/publiccode-usage.json`, and an organizational aggregation tool crawls these to assemble the public-facing file. This directly addresses the stale data risk — data is maintained at the source by deployment processes rather than manually curated at the organizational level. The AI had not considered this pattern; the human's insight came from practical knowledge of how large organizations manage distributed deployments.

18. **Public procurement policy framing directed by human.** The human identified that the proposal lacked explicit connection to the "Public Money, Public Code" principle, procurement criteria frameworks, and legislative models. The AI had treated procurement as one of several use cases but had not positioned the extensions as infrastructure for implementing procurement policy. The human provided four URLs covering the PMPC campaign, contribution-based procurement, Switzerland's EMBAG law, and OSBA procurement criteria. The AI mapped each extension to specific procurement needs and OSBA criteria, and identified "Policy Maker / Legislator" as a distinct actor from "Federal Authority / Funder" — legislators draft laws and procurement regulations, while funders allocate money. The human also corrected the AI's initial attempt to combine APELL and EuroStack into a single ally entry, noting that they are separate initiatives with separate acquaintances.

19. **Credit system details explicitly scoped out by human.** The human provided details about Drupal's `contribution_records` module (open-source, ticket-system-agnostic, API-ready, reusable as SaaS) and credit weight system (issue credits scaled by project usage, case studies, event sponsorships, organizational membership), as well as the critical credit stickiness property (credits remain with a vendor after a contributor changes employer). The human directed these to be distilled into the proposal as reference architecture context, while explicitly noting that specifying the details of a credit system is beyond the scope of this proposal. The AI added this as a spectrum — from Drupal's sophisticated weighted system to simple GitHub Sponsors — to make clear that the Credit Registry API is intentionally agnostic to credit methodology.

20. **Human editorial pass to reduce prescriptiveness.** The human performed a comprehensive manual edit across PROPOSAL.md, ROADMAP.md, and RISK_ANALYSIS.md — without AI involvement — to shorten documents, reduce specific proposed terminology, and leave more room for stakeholders to shape the proposal. The motivation: the documents had become too detailed and prescriptive, which risks discouraging collaboration by making it feel like a top-down, already-decided plan. The final shape will depend on which stakeholders sign on and what they bring in terms of expertise, influence, and time.

21. **Methodology restructured based on external feedback.** Andrew Nesbitt reviewed this methodology document and noted that web search query strings are not useful for reproducibility (the search engine and results are opaque), but the investigated URLs are valuable as a reproducible research log and reference document. The AI restructured accordingly: query strings removed, URLs retained, summary lines added for web-search-discovered context.

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
2. **Log investigated sources** — the URLs actually fetched and analyzed form the reproducible research trail. Web search query strings are less useful since the search engine and results are opaque.
3. **Separate human decisions from AI outputs.** The "Editorial Decisions" section is critical for understanding what the AI would not have done on its own.
4. **Version the tools.** AI model versions matter for reproducibility. Record the exact model ID.
5. **Acknowledge limitations** including the inherent non-reproducibility of LLM outputs (same prompt may yield different results).

A future standard for this kind of documentation might combine:
- **PROV-O** vocabulary (Entity/Activity/Agent) for formal provenance chains
- **CRediT** roles for human-AI attribution
- **PRISMA-style** flow diagrams for the research process
- **Publisher AI disclosure** conventions for prompt/tool documentation

Until such a standard exists, a structured markdown document like this one is a pragmatic alternative.
