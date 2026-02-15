# Evaluation of Software Metadata Standards for a Global Open Source Index

An analysis of emerging standards for describing open source software in source repositories, plus a comprehensive proposal for a decentralized ecosystem of metadata registries and procurement policies to make evidence-based open source adoption at scale.

Our goal: create an integrated infrastructure enabling procurement offices, security researchers, and funders to discover, evaluate, and confidently adopt open source projects at scale—particularly within the public sector, where public money should support maintainable, transparent software.

---

## **Quick Navigation**

**New to this project?** Start with [HOW_TO_READ.md](HOW_TO_READ.md) for guided reading paths by role.

**Encounter unfamiliar terms?** Check [GLOSSARY.md](GLOSSARY.md) for explanations of technical terms and acronyms.

**Want the executive summary?** Jump to [PITCH.md](PITCH.md) to see benefits for your role.

---

## Goals

We seek an ecosystem of metadata standards, decentralized registries, and policy frameworks with broad adoption across the open source community and public sector that enables:

1. **Procurement discovery** — Help procurement professionals find software that meets their specific needs (e.g., a content management system that also offers customer relationship management) through faceted classification and standardized metadata.
2. **Vendor and contributor credit visibility** — Make visible which organizations contribute to projects through decentralized credit registries with verifiable trust models, creating transparent pathways to vendor expertise and shared investment in open source.
3. **Supply chain visibility** — Enable funding agencies to identify critical infrastructure gaps and direct resources where they're most needed by linking projects to SBOMs, security assessments, and compliance data.
4. **Usage transparency** — Allow procurement offices and deploying organizations to declare which software they use through standardized organization-level declarations, building a shared map of what's actually in use across organizations.
5. **Security and compliance assessment** — Provide procurement professionals with verified information about security practices, dependencies, and license compliance before adoption.
6. **Evidence-based procurement policy** — Support procurement regulations and digital sovereignty mandates with standardized data infrastructure that makes vendor expertise, supply chain security, and community health transparently measurable.

## Candidates Evaluated

We examined five metadata standards and specifications that could potentially serve as infrastructure for the procurement and discovery needs described above.

| Standard                                                   | Format          | Status                    | Scope                                  |
| ---------------------------------------------------------- | --------------- | ------------------------- | -------------------------------------- |
| [publiccode.yml](#1-publiccodeyml)                         | YAML            | Active, EU-adopted        | Public sector software metadata        |
| [OSS Taxonomy](#2-oss-taxonomy-andrew-nesbitt--ecosystems) | YAML vocabulary | Early stage               | Faceted classification for OSS         |
| [CodeMeta](#3-codemeta)                                    | JSON-LD         | Active, academic adoption | Research software citation/metadata    |
| [gov-codejson](#4-gov-codejson-dsacms--codegov-lineage)    | JSON            | Active, U.S.-specific     | U.S. federal agency software inventory |
| [contribute.json](#5-contributejson-mozilla)               | JSON            | Decommissioned (2024)     | Contribution onboarding metadata       |

---

## Detailed Analysis

### 1. publiccode.yml

**What it is:** A YAML metadata file placed in a repository's root directory, created by Italy's Digital Transformation Team. It describes a project's name, purpose, categories, type (standalone web app, library, etc.), intended audience, maintenance arrangements, legal information and licensing, dependencies, and translations.

- Spec: https://yml.publiccode.tools/schema.core.html
- GitHub: https://github.com/publiccodeyml/publiccode.yml

**Strengths:**

- **Real-world adoption at scale.** Mandatory in Italy since 2020, powering [Developers Italia](https://developers.italia.it/en/reuse/publication.html). Adopted by Germany's [openCode.de](https://opencode.de/en/sharing-and-developing-code). Since March 2025, required by the [EU Open Source Solutions Catalogue](https://interoperable-europe.ec.europa.eu/eu-oss-catalogue)—640+ projects across 30+ public sector domains.
- **Purpose-built for procurement.** Has fields for `intendedAudience` (countries, scope), `categories` (controlled vocabulary like "content-management", "crm"), `softwareType` (standalone/web, library, addon, etc.), `maintenance` (internal/contract/community/none with contractor details and expiry dates), and `usedBy` (list of public administrations using the software).
- **Federated crawling architecture.** National catalogs feed into the EU-wide catalog via public APIs — proven at scale.
- **Human-readable YAML.** Low barrier for maintainers. Tooling exists: [publiccode.yml editor](https://editor.opencode.de/), validators, crawlers.
- **Country-specific extensions.** The schema has a modular design where each country can define additional fields without breaking the core.
- **Badge ecosystem proven.** openCode.de already runs [automated badges](https://badges.opencode.de/en/) for maintenance, reuse (Level 1–3 based on confirmed adopters), security, and open source compliance — directly relevant to procurement trust signals.

**Weaknesses:**

- **Public-sector-centric naming and framing.** "publiccode" signals government software; open source projects outside government may not feel it applies to them. Fields like `usedBy` assume public administration context.
- **Version fields go stale quickly.** Fields like `softwareVersion` and `releaseDate` change with every release but are rarely updated once committed to the repository. Across thousands of publiccode.yml files indexed by [ecosyste.ms](https://ecosyste.ms/), most contain outdated version information—not because maintainers are negligent, but because it's impractical to update a committed file with every release. This data already exists in authoritative sources (GitHub releases, package registries). Duplicating it in a metadata file creates staleness, which erodes trust in the entire file.
- **No community credit system.** Fields like `maintenance.contractors` provide basic metadata (name and contract expiry) but don't connect the project to Drupal-style reputation systems where vendor expertise is visible to procurement officers.
- **Limited taxonomy depth.** The `categories` list is flat and coarse (e.g., "content-management") — no way to express combinations like "CMS with healthcare focus in the backend layer."
- **No supply chain / dependency visibility.** No references to SBOMs (bill of materials lists of all dependencies), OpenSSF Scorecards (security assessments), or vulnerability disclosure policies beyond what third-party badge systems add externally.
- **Small, distributed governance.** The spec is maintained by the [publiccodeyml GitHub organization](https://github.com/publiccodeyml/publiccode.yml) with limited maintainer capacity relative to the scale of institutional adoption. Scope increases require approval from a small community.

**Chances of success:** Very high for public sector adoption. It's already the de-facto EU standard. The key question is whether it can expand beyond government contexts to attract the broader open source community.

---

### 2. OSS Taxonomy (Andrew Nesbitt / ecosyste.ms)

**What it is:** A faceted classification system for categorizing open source projects across six dimensions:

- **Domain**: What field does the software serve? (e.g., healthcare, education)
- **Function**: What does it do? (e.g., authentication, reporting)
- **Technology**: What languages or frameworks does it use?
- **Audience**: Who uses it? (developers, enterprises, citizens)
- **Layer**: Where in the technology stack? (frontend, backend, infrastructure)
- **Role**: What architectural role? (library, plugin, framework, application)

The taxonomy is stored as YAML in a GitHub repository and designed to integrate with CodeMeta (another metadata standard listed below) through namespaced keywords like `"domain:web-development"`.

**Strengths:**

- **Solves the taxonomy problem well.** Multi-faceted classification is far more expressive than flat category lists. A project can be `domain:healthcare`, `function:authentication`, `layer:backend`, `role:library` simultaneously.
- **Designed for funding/gap analysis.** If "function:authentication" is widely depended on but has few maintained options, funders can identify this.
- **Integrates with existing standards.** Designed to work as a layer on top of CodeMeta without schema changes — pure additive.
- **Backed by ecosyste.ms infrastructure.** Andrew Nesbitt runs the [largest open dataset of OSS metadata](https://ecosyste.ms/), tracking packages, repos, and dependencies across ecosystems.

**Weaknesses:**

- **Not a metadata file standard.** It's a classification vocabulary, not a repo-level manifest. Projects don't place an "oss-taxonomy.yml" in their repo — classification happens externally or via CodeMeta keywords.
- **Early stage, single-person governance.** Community-contributed via GitHub PRs but no institutional backing yet.
- **No procurement, vendor, or maintenance metadata.** It classifies _what_ software does, not _who_ maintains it, _how_ it's supported, or _who_ has expertise.

**Chances of success:** **High as a vocabulary/taxonomy layer** that other standards adopt. Low as a standalone standard. Best outcome: publiccode.yml and CodeMeta adopt its classification dimensions.

---

### 3. CodeMeta

**What it is:** A schema for describing research software in JSON-LD format (a way to embed structured data in JSON that's machine-readable across the web). A `codemeta.json` file in the repository root. Maintained by a consortium of research institutions affiliated with the [SciCodes Consortium](https://scicodes.net/). Designed specifically for making research software findable and citable in academic contexts.

**Strengths:**

- **Strong academic/research adoption.** Used by [Software Heritage](https://www.softwareheritage.org/), Zenodo, and FAIRCORE4EOSC. The primary standard for research software citation and discovery.
- **Machine-readable with web standards.** Built on [JSON-LD](https://json-ld.org/), which embeds structured data into JSON, and [schema.org](https://schema.org/), a shared vocabulary used across the web. This enables interoperability: data from CodeMeta can be linked with other datasets.
- **Crosswalks to package ecosystems.** Mappings exist between CodeMeta and metadata from npm (JavaScript), PyPI (Python), CRAN (R), Maven (Java), and other package managers. This means CodeMeta can be auto-populated from package metadata rather than manually maintained.
- **Active effort to merge upstream.** Work is ongoing to get CodeMeta terms adopted directly into [schema.org](https://github.com/codemeta/codemeta/issues/232), which would make it the web-native standard.

**Weaknesses:**

- **Academic focus, not procurement focus.** Core use case is citation and reproducibility, not "help a procurement officer find a CMS with good vendor support."
- **JSON-LD complexity.** The format is powerful but intimidating for average maintainers compared to simple YAML. The `@context`, `@type` boilerplate adds friction.
- **No maintenance/vendor/support metadata.** No fields for contractor info, maintenance contracts, vendor expertise, or adopter declarations.
- **No controlled category vocabulary.** Software descriptions are free-text or use schema.org `applicationCategory`, which is not standardized for procurement domains.
- **Low adoption outside academia.** Most mainstream open source projects don't ship a `codemeta.json`.

**Chances of success:** **High in academia, low for procurement.** It's the right semantic foundation but lacks the procurement-specific fields. Best outcome: serve as the underlying linked-data vocabulary that publiccode.yml maps to.

---

### 4. gov-codejson (DSACMS / code.gov lineage)

### 4. gov-codejson (U.S. Federal Government)

A JSON schema for U.S. federal agency software inventory, evolved from the code.gov `code.json` standard mandated by U.S. policy. The approach is narrow (U.S. federal context), small community (5 stars, 2 forks on GitHub), and fundamentally agency-centric (an internal inventory format rather than a discovery format for external procurement).

It will persist as a U.S. federal compliance requirement but is not viable as a global standard. The natural path forward: U.S. agencies could adopt publiccode.yml with a U.S. country extension, as Italy and Germany have done.

---

### 5. contribute.json (Mozilla)

### 5. contribute.json (Mozilla) — Archived

[contribute.json](https://github.com/mozilla/contribute.json) was decommissioned in March 2024 with zero adoption outside Mozilla. It was a narrow contribution-onboarding schema that never found any community traction.

---

## Other Relevant Standards and Initiatives

| Standard                                                                     | Focus                                                | Relevance                                                                                                                                        |
| ---------------------------------------------------------------------------- | ---------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| [CITATION.cff](https://citation-file-format.github.io/)                      | Software citation (YAML)                             | GitHub-native support. Covers authorship/citation only. Complementary, not competing.                                                            |
| [REUSE (FSFE)](https://reuse.software/spec-3.3/)                             | Per-file licensing compliance using SPDX             | Adopted by KDE, SAP, Nextcloud, DLR. Already integrated as a badge on openCode.de. Complementary.                                                |
| [OpenSSF Scorecard](https://scorecard.dev/)                                  | Automated security health scoring                    | Scans 1M+ projects weekly. Security metadata that procurement officers need. Complementary.                                                      |
| [CycloneDX](https://cyclonedx.org/) / [SPDX](https://spdx.dev/)              | SBOM (Software Bill of Materials)                    | Supply chain transparency, mandated by U.S. executive order. Answers "what are the dependencies" not "what is this software for." Complementary. |
| [schema.org SoftwareSourceCode](https://schema.org/SoftwareSourceCode)       | Web-native linked data vocabulary                    | The semantic backbone that CodeMeta builds on. Not a standalone file format.                                                                     |
| [Canada Open Resource Exchange](https://github.com/canada-ca/ore-ero)        | Canadian government open source catalog              | Uses its own schema, but has discussed adopting publiccode.yml.                                                                                  |
| [Drupal Credit System](https://www.drupal.org/drupalorg/contribution-credit) | Vendor contribution tracking and marketplace ranking | The most mature model for linking contributions to vendor credibility. Not a file format but a system design to learn from.                      |

### Digital Sovereignty Score Initiatives

A growing number of initiatives assess digital sovereignty at the organization, provider, or country level. These operate above the project metadata layer — they are **consumers** of the kind of data publiccode.yml provides, not competing standards. The EU Cloud Sovereignty Framework's SOV-5 (Supply Chain, 20% weight) and SOV-6 (Technology Sovereignty — requires open source components, open APIs, open protocols) directly need project-level metadata like SBOM references, license information, and open standard declarations.

| Initiative                                                                                                               | What it assesses                           | Relevance to this proposal                                                                                                                                                         |
| ------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [EU Cloud Sovereignty Framework](https://commission.europa.eu/document/download/09579818-64a6-4dd5-9577-446ab6219113_en) | Cloud service providers (EU scope)         | Explicitly requires open source components (SOV-6). Project-level metadata about licensing, SBOMs, and open standards directly feeds this assessment.                              |
| Munich Digital Sovereignty Score                                                                                         | Municipal IT services                      | Uses a scoring system similar to nutritional labeling. Software metadata enables objective scoring.                                                                                |
| Bechtle Index für digitale Souveränität                                                                                  | Enterprise and public sector organizations | Software-independent assessment tool launching in 2026. Relies on availability of standardized project metadata.                                                                   |
| Nextcloud Digital Sovereignty Index                                                                                      | Countries                                  | Counts deployments of self-hosted tools (50 collaboration platforms) across countries. Usage declarations and adoption registries would make this data collection more verifiable. |
| SUSE Cloud Sovereignty Framework Assessment                                                                              | Organizations                              | Free self-assessment tool. Producing ratings and identifying gaps requires project-level supply chain and licensing data.                                                          |

These frameworks strengthen the case for publiccode.yml extensions: sovereignty assessors need machine-readable project metadata (licenses, SBOMs, open standard compliance, dependency transparency) to automate their evaluations. The proposed `supplyChain` section and faceted `classification` directly feed this need.

---

## Gap Analysis by Use Case

### Use Case 1: Procurement discovery ("find me a CMS that's also a CRM")

- **publiccode.yml** has `categories` but it's flat. Adopting the **OSS Taxonomy** faceted approach (domain + function + audience + layer) would make this dramatically better.
- **Missing:** No marketplace-style search UX exists beyond openCode.de and the EU OSS Catalogue.

### Use Case 2: Vendor/contributor credit metadata (Drupal-style)

- **Nobody covers this today.** This is the biggest gap. Drupal's system works because it's centralized — the [credit system](https://www.drupal.org/drupalorg/contribution-credit) tracks contributions per-issue and rolls up to marketplace rankings.
- **What's needed:** A way for projects to point to external credit registries that track vendor/contributor data. The ecosyste.ms [funds leaderboard proposal](https://github.com/ecosyste-ms/funds/issues/236) and the [Open Source Pledge integration discussion](https://github.com/opensourcepledge/opensourcepledge.com/issues/418) are working toward this from the funding side.
- **Possible approach:** Extend publiccode.yml with a `creditRegistries` section — project-endorsed pointers to external systems (Drupal-style credit APIs, ecosyste.ms, forge stats) where the dynamic credit data actually lives.

### Use Case 3: Supply chain steering and gap identification

- **OSS Taxonomy** is purpose-built for this (identify underinvested function areas).
- **OpenSSF Scorecard** provides the security dimension.
- **CycloneDX/SPDX SBOMs** provide the dependency graph.
- **Missing:** A way to link these together in a single metadata file. publiccode.yml could reference SBOM locations and scorecard URLs.

### Use Case 4: Procurement offices declaring what they use (openCode.de reuse badge)

- **publiccode.yml** has `usedBy` but it's manually maintained and unverified.
- **openCode.de's badge system** is the most advanced implementation: confirmed adopter declarations drive the [Reuse Badge](https://badges.opencode.de/en/) (Level 1–3).
- **Missing:** A federated protocol for procurement offices to _publish_ their software declarations. Usage data doesn't belong in the project's publiccode.yml (the project has no authority over who uses it) — it needs decentralized usage registries with a standardized discovery mechanism.

---

## Recommendation

**Recommendation:** Build on publiccode.yml because:

- **It has real institutional traction.** It's not a proposal or proof-of-concept — it's a legal requirement in Italy, activated in Germany, and mandatory for the EU Open Source Catalogue since March 2025 (640+ projects indexed). This is the only candidate to have crossed from "promising idea" to "operational infrastructure."
- **Federated architecture already works.** openCode.de and Developers Italia prove the crawler model scales. National catalogs feed into the EU-wide catalog. No theoretical research needed.
- **Proven tooling ecosystem.** Editors, validators, and crawlers already exist. Projects can adopt it today.
- **Aligned with policy direction.** As governments mandate open source (like Switzerland's EMBAG law), procurement offices urgently need the discovery infrastructure that publiccode.yml extensions provide.

**But extend it with:**

1. **Faceted taxonomy** (from OSS Taxonomy) replacing or supplementing the flat `categories` list — enabling "CMS that's also a CRM" queries
2. **Credit registry discovery** — a `creditRegistries` section pointing to external systems (Drupal-style credit APIs, ecosyste.ms) where vendor/contributor data lives, endorsed by the project
3. **Supply chain references** — fields pointing to SBOM locations, OpenSSF Scorecard results, and REUSE compliance status
4. **A companion Registry Discovery Standard** — decentralized usage registries where procurement offices declare what they deploy, discoverable by crawlers without per-project configuration, enabling the reuse badge system to scale beyond openCode.de
5. **Deprecation of temporal fields** — remove `softwareVersion`, `releaseDate`, and `dependsOn[].versionMin/versionMax` from the spec, keeping only slow-changing, human-authored data that cannot be reliably obtained from forge APIs or package registries

**Make publiccode.yml attractive to non-government open source projects** by:

- Reframing it (or creating a profile/alias) that doesn't signal "government only"
- Making the public-sector-specific fields optional while keeping the core useful for any project
- Integrating with package registries (npm, PyPI, crates.io) through CodeMeta crosswalks so that metadata can be auto-populated

> The worst outcome would be fragmentation — five competing standards each with 10% adoption. publiccode.yml is the only candidate that has crossed the threshold from "proposal" to "infrastructure with legal backing," and the EU's adoption makes it the gravitational center.

**See [PROPOSAL.md](PROPOSAL.md) for a concrete proposal** with schema extensions for faceted classification, supply chain references, credit registry discovery, and a Registry Discovery Standard for usage declarations.

**See [PITCH.md](PITCH.md) for stakeholder pitches** — what each actor (maintainers, vendors, procurement offices, funders, registries) gains economically and politically from this proposal.

**See [METHODOLOGY.md](METHODOLOGY.md) for research provenance** — full documentation of the AI-assisted research process, prompts, sources, editorial decisions, and limitations.

**See [ROADMAP.md](ROADMAP.md) for an illustrative phased implementation plan** - to work towards the proposal by building momentum with achievable milestones, and address critical risks.

**See [RISK_ANALYSIS.md](RISK_ANALYSIS.md) for a systematic examination of risks to the proposal** - each risk includes an assessment of likelihood and impact, plus potential mitigations.

---

## Related Discussions

- [Drupal: How credit is granted for core issues](https://www.drupal.org/about/core/policies/maintainers/how-credit-is-granted-for-drupal-core-issues)
- [Open Source Pledge: Contribution tracking discussion](https://github.com/opensourcepledge/opensourcepledge.com/issues/418)
- [ecosyste.ms funds: Cross-ecosystem funding](https://github.com/ecosyste-ms/funds/issues/290)
- [ecosyste.ms funds: Funder leaderboard](https://github.com/ecosyste-ms/funds/issues/236)
- [USDS DITAP Curriculum](https://github.com/usds/ditap-curriculum-update)
- [CivicActions Open Practice](https://github.com/CivicActions/open-practice)
- [The Dependency Layer in Digital Sovereignty (Nesbitt)](https://nesbitt.io/2026/01/28/the-dependency-layer-in-digital-sovereignty.html)
- [Funding Open Source for Digital Sovereignty (Buytaert)](https://dri.es/funding-open-source-for-digital-sovereignty)
- [Public Money? Public Code! (FSFE)](https://publiccode.eu/)
- [Switzerland's EMBAG open source law (OSOR)](https://interoperable-europe.ec.europa.eu/collection/open-source-observatory-osor/news/new-open-source-law-switzerland)
- [Selection Criteria for Sustainable Procurement of OSS (OSBA)](https://osb-alliance.de/publikationen/veroeffentlichungen/selection-criteria-for-the-sustainable-procurement-of-open-source-software)
