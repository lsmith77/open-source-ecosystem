# Evaluation of Software Metadata Standards for a Global Open Source Index

An analysis of emerging standards for describing open source software in source repositories, with the goal of enabling a crawlable software index for procurement, supply chain security, and ecosystem transparency — particularly for the public sector.

## Goal

Create a single format with a good chance of wide adoption from the open source community and the public sector that enables:

1. **Procurement discovery** — help procurement people find software relevant to their goals (e.g., a CMS that is also a CRM)
2. **Vendor/contributor credit metadata** — attach additional metadata (e.g., Drupal-style credit system) to provide procurement people insights about vendors knowledgeable with the software
3. **Supply chain steering** — enable federal authorities to steer money to secure the supply chain or identify gaps
4. **Usage declarations** — enable procurement offices to declare which software they use (inspired by [openCode.de badges](https://badges.opencode.de/en/))
5. **Security and compliance assessment** — enable procurement offices to evaluate the security posture, dependency transparency, and licensing compliance of software before adoption (e.g., OpenSSF Scorecard, SBOM availability, REUSE compliance)

## Candidates Evaluated

| Standard | Format | Status | Scope |
|---|---|---|---|
| [publiccode.yml](#1-publiccodeyml) | YAML | Active, EU-adopted | Public sector software metadata |
| [OSS Taxonomy](#2-oss-taxonomy-andrew-nesbitt--ecosystems) | YAML vocabulary | Early stage | Faceted classification for OSS |
| [CodeMeta](#3-codemeta) | JSON-LD | Active, academic adoption | Research software citation/metadata |
| [gov-codejson](#4-gov-codejson-dsacms--codegov-lineage) | JSON | Active, U.S.-specific | U.S. federal agency software inventory |
| [contribute.json](#5-contributejson-mozilla) | JSON | Decommissioned (2024) | Contribution onboarding metadata |

---

## Detailed Analysis

### 1. publiccode.yml

**What it is:** A YAML metadata file placed in the repository root, originally developed by Italy's Digital Transformation Team. Defines software name, description, categories, software type, intended audience, maintenance status/contacts, legal/licensing info, dependencies, and localized descriptions.

- Spec: https://yml.publiccode.tools/schema.core.html
- GitHub: https://github.com/publiccodeyml/publiccode.yml

**Strengths:**

- **Strongest real-world traction by far.** Mandatory in Italy since 2020 (powering [Developers Italia](https://developers.italia.it/en/reuse/publication.html) catalog), adopted by Germany's [openCode.de](https://opencode.de/en/sharing-and-developing-code), and since March 2025, **a prerequisite for the [EU Open Source Solutions Catalogue](https://interoperable-europe.ec.europa.eu/eu-oss-catalogue)** (640+ solutions across 30+ public sector domains).
- **Purpose-built for procurement.** Has fields for `intendedAudience` (countries, scope), `categories` (controlled vocabulary like "content-management", "crm"), `softwareType` (standalone/web, library, addon, etc.), `maintenance` (internal/contract/community/none with contractor details and expiry dates), and `usedBy` (list of public administrations using the software).
- **Federated crawling architecture.** National catalogs feed into the EU-wide catalog via public APIs — proven at scale.
- **Human-readable YAML.** Low barrier for maintainers. Tooling exists: [publiccode.yml editor](https://editor.opencode.de/), validators, crawlers.
- **Country-specific extensions.** The schema has a modular design where each country can define additional fields without breaking the core.
- **Badge ecosystem proven.** openCode.de already runs [automated badges](https://badges.opencode.de/en/) for maintenance, reuse (Level 1–3 based on confirmed adopters), security, and open source compliance — directly relevant to procurement trust signals.

**Weaknesses:**

- **Public-sector-centric naming and framing.** "publiccode" signals government software; open source projects outside government may not feel it applies to them. Fields like `usedBy` assume public administration context.
- **Temporal fields cause widespread staleness.** Fields like `softwareVersion`, `releaseDate`, and `dependsOn[].versionMin` change with every release but are rarely updated in the metadata file. Across the thousands of publiccode.yml files indexed by ecosyste.ms, most are out of date because nobody remembers to update a metadata file between releases. This data already exists in authoritative sources (forge APIs, package registries) — duplicating it in a committed file creates a staleness problem that undermines trust in the entire file.
- **No vendor/contributor credit system.** No fields for tracking which companies contribute, their expertise level, or anything resembling Drupal's credit system. The `maintenance.contractors` field is rudimentary (name + expiry date).
- **Limited taxonomy depth.** The `categories` list is flat and coarse (e.g., "content-management") — no faceted classification like domain + function + audience.
- **No supply-chain / dependency-security fields.** No SBOM reference, no OpenSSF scorecard integration, no vulnerability policy metadata beyond what badges add externally.
- **Small governance community.** The spec is maintained by the [publiccodeyml](https://github.com/publiccodeyml/publiccode.yml) GitHub organization. The separately-governed [Foundation for Public Code](https://publiccode.net/) (a Dutch foundation) maintains [The Standard for Public Code](https://standard.publiccode.net/), a broader framework that recommends publiccode.yml as a tool — but does not control the spec itself. The spec's maintainer community is small relative to its institutional adoption.

**Chances of success:** **High for the public sector.** It is already the de facto EU standard. The question is whether it can expand beyond government contexts.

---

### 2. OSS Taxonomy (Andrew Nesbitt / ecosyste.ms)

**What it is:** A [faceted classification system](https://nesbitt.io/2025/11/29/oss-taxonomy.html) for categorizing open source projects across 6 dimensions: domain, role, technology, audience, layer, and function. Stored as YAML in a GitHub repo, designed to integrate with CodeMeta via namespaced keywords (e.g., `"domain:web-development"`).

See also: [The Dependency Layer in Digital Sovereignty](https://nesbitt.io/2026/01/28/the-dependency-layer-in-digital-sovereignty.html)

**Strengths:**

- **Solves the taxonomy problem well.** Multi-faceted classification is far more expressive than flat category lists. A project can be `domain:healthcare`, `function:authentication`, `layer:backend`, `role:library` simultaneously.
- **Designed for funding/gap analysis.** If "function:authentication" is widely depended on but has few maintained options, funders can identify this.
- **Integrates with existing standards.** Designed to work as a layer on top of CodeMeta without schema changes — pure additive.
- **Backed by ecosyste.ms infrastructure.** Andrew Nesbitt runs the [largest open dataset of OSS metadata](https://ecosyste.ms/), tracking packages, repos, and dependencies across ecosystems.

**Weaknesses:**

- **Not a metadata file standard.** It's a classification vocabulary, not a repo-level manifest. Projects don't place an "oss-taxonomy.yml" in their repo — classification happens externally or via CodeMeta keywords.
- **Early stage, single-person governance.** Community-contributed via GitHub PRs but no institutional backing yet.
- **No procurement, vendor, or maintenance metadata.** It classifies *what* software does, not *who* maintains it, *how* it's supported, or *who* has expertise.

**Chances of success:** **High as a vocabulary/taxonomy layer** that other standards adopt. Low as a standalone standard. Best outcome: publiccode.yml and CodeMeta adopt its classification dimensions.

---

### 3. CodeMeta

**What it is:** A [JSON-LD metadata schema](https://codemeta.github.io/) based on schema.org terms for describing software. A `codemeta.json` file in the repo root. Maintained by a research community consortium, affiliated with the [SciCodes Consortium](https://scicodes.net/).

**Strengths:**

- **Strong academic/research adoption.** Used by [Software Heritage](https://www.softwareheritage.org/), Zenodo, and FAIRCORE4EOSC. The primary standard for research software citation and discovery.
- **Semantically rich.** Built on JSON-LD and schema.org, enabling linked-data interoperability. Machine-readable in a way that integrates with the broader web of data.
- **Crosswalks exist.** Mappings between CodeMeta and other metadata formats (npm, PyPI, CRAN, Maven, etc.) enable automated extraction.
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

**What it is:** A [JSON schema](https://github.com/DSACMS/gov-codejson) for collecting metadata about U.S. federal agency software projects. Evolved from the code.gov `code.json` standard mandated by [M-16-21](https://obamawhitehouse.archives.gov/sites/default/files/omb/memoranda/2016/m-16-21.pdf) and the SHARE IT Act.

**Strengths:**

- **U.S. federal mandate.** Agencies are legally required to publish this metadata, creating a compliance-driven adoption floor.
- **Active development.** Schema v2.0.0 released September 2025, showing ongoing iteration.
- **Government-specific fields.** Designed for the compliance context of federal software inventory.

**Weaknesses:**

- **U.S.-specific scope.** Designed for U.S. federal compliance, not international use. No localization, no multi-country architecture.
- **Agency-centric, not project-centric.** Metadata describes what an agency has, not what a project is. It's an inventory format, not a discovery format.
- **Tiny community.** 5 stars, 2 forks — the team is "still building the core team and defining roles and responsibilities."
- **No ecosystem for open source broadly.** Not designed to be placed in open source repositories by their maintainers; it's for agency reporting.
- **Overlaps with publiccode.yml** but with less maturity, less tooling, and narrower geographic scope.

**Chances of success:** **Low as a global standard.** Will persist as a U.S. compliance artifact. The smart move would be for DSACMS to adopt publiccode.yml with a U.S. country extension, similar to how Italy and Germany did.

---

### 5. contribute.json (Mozilla)

**What it is:** A [JSON schema](https://github.com/mozilla/contribute.json) for describing how to contribute to an open source project — communication channels, bug tracking, deployment URLs, tech stack.

**Strengths:**

- None that are currently relevant.

**Weaknesses:**

- **Decommissioned and archived** (March 2024). The website (contributejson.org) is gone. The schema was never stabilized.
- **Narrow scope.** Only covered contribution onboarding metadata, not software classification, procurement, or supply chain.
- **Zero adoption** beyond Mozilla's own projects.

**Chances of success:** **Zero.** This is a dead project. Mentioned here only as a cautionary tale about standards that don't find a community.

---

## Other Relevant Standards and Initiatives

| Standard | Focus | Relevance |
|---|---|---|
| [CITATION.cff](https://citation-file-format.github.io/) | Software citation (YAML) | GitHub-native support. Covers authorship/citation only. Complementary, not competing. |
| [REUSE (FSFE)](https://reuse.software/spec-3.3/) | Per-file licensing compliance using SPDX | Adopted by KDE, SAP, Nextcloud, DLR. Already integrated as a badge on openCode.de. Complementary. |
| [OpenSSF Scorecard](https://scorecard.dev/) | Automated security health scoring | Scans 1M+ projects weekly. Security metadata that procurement officers need. Complementary. |
| [CycloneDX](https://cyclonedx.org/) / [SPDX](https://spdx.dev/) | SBOM (Software Bill of Materials) | Supply chain transparency, mandated by U.S. executive order. Answers "what are the dependencies" not "what is this software for." Complementary. |
| [schema.org SoftwareSourceCode](https://schema.org/SoftwareSourceCode) | Web-native linked data vocabulary | The semantic backbone that CodeMeta builds on. Not a standalone file format. |
| [Canada Open Resource Exchange](https://github.com/canada-ca/ore-ero) | Canadian government open source catalog | Uses its own schema, but has discussed adopting publiccode.yml. |
| [Drupal Credit System](https://www.drupal.org/drupalorg/contribution-credit) | Vendor contribution tracking and marketplace ranking | The most mature model for linking contributions to vendor credibility. Not a file format but a system design to learn from. |

### Digital Sovereignty Score Initiatives

A growing number of initiatives assess digital sovereignty at the organization, provider, or country level. These operate above the project metadata layer — they are **consumers** of the kind of data publiccode.yml provides, not competing standards. The EU Cloud Sovereignty Framework's SOV-5 (Supply Chain, 20% weight) and SOV-6 (Technology Sovereignty — requires open source components, open APIs, open protocols) directly need project-level metadata like SBOM references, license information, and open standard declarations.

| Initiative | Scope | What it assesses |
|---|---|---|
| [EU Cloud Sovereignty Framework](https://commission.europa.eu/document/download/09579818-64a6-4dd5-9577-446ab6219113_en) | Cloud providers (EU) | 8 sovereignty objectives (SOV-1–SOV-8), SEAL 0–4 rating. Basis for a €180M EU procurement tender. SOV-6 explicitly requires open source. |
| [Munich Digital Sovereignty Score](https://www.heise.de/en/news/Munich-makes-digital-sovereignty-measurable-with-its-own-score-11164230.html) | Municipal IT services | Nutri-Score-style rating of vendor lock-in, jurisdiction risk, open standards. Applied to 194 services; integrated into procurement. |
| [Bechtle Index für digitale Souveränität](https://www.bechtle.com/ueber-bechtle/presse/pressemeldungen/2025/bechtle-entwickelt-index-fuer-digitale-souveraenitaet) | Enterprise / public sector orgs | Data sovereignty, technological independence, design freedom. Software-based assessment launching Q1 2026. |
| [Nextcloud Digital Sovereignty Index](https://nextcloud.com/blog/digital-sovereignty-index-how-countries-compare-in-digital-independence/) | Countries | Self-hosted tool deployments per 100k citizens across ~60 countries. Measures adoption of 50 collaboration tools. |
| [SUSE CSF Assessment](https://www.suse.com/cloud-sovereignty-framework-assessment/) | Organizations | Free self-assessment tool scoring against the EU Cloud Sovereignty Framework. Produces SEAL rating and gap analysis. |

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
- **Missing:** A federated protocol for procurement offices to *publish* their software declarations. Usage data doesn't belong in the project's publiccode.yml (the project has no authority over who uses it) — it needs decentralized usage registries with a standardized discovery mechanism.

---

## Recommendation

**Build on publiccode.yml.** It has the strongest momentum:

- Legal mandate in Italy, adopted in Germany, prerequisite for the EU OSS Catalogue since 2025
- Proven federated crawler architecture
- Active tooling ecosystem (editors, validators, crawlers)
- Already designed for the public sector audience
- Aligned with the [Public Money, Public Code](https://publiccode.eu/) principle — as governments mandate open source (e.g., [Switzerland's EMBAG](https://www.fedlex.admin.ch/eli/cc/2023/682/en)), procurement offices need the discovery and evaluation infrastructure that publiccode.yml extensions can provide

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
