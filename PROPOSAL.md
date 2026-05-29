# Proposal: An Integrated Ecosystem for Decentralized Open Source Registry Infrastructure

A concrete proposal for evolving publiccode.yml with several backward-compatible improvements, plus companion standards (APIs, registries, discovery mechanisms) and supporting policy frameworks to address the gaps identified in the [evaluation](RESEARCH.md). The improvements are backward-compatible — existing v0.x files remain valid — while the companion standards enable decentralized registries, standardized data formats, and verifiable trust networks for procurement and adoption decisions.

## Contents

- [Design Principles](#design-principles)
- [Policy Context: Public Procurement and Open Source](#policy-context-public-procurement-and-open-source)
- [Actors and Relationships](#actors-and-relationships)
- [Improvement 1: Faceted Classification](#improvement-1-faceted-classification-replacing-flat-categories)
- [Improvement 2: Supply Chain References](#improvement-2-supply-chain-references)
- [Improvement 3: Vendor Contribution Credit System Discovery](#improvement-3-vendor-contribution-credit-system-discovery)
- [Improvement 4: Deprecate `usedBy`](#improvement-4-deprecate-usedby)
- [Improvement 5: Deprecate Temporal Fields](#improvement-5-deprecate-temporal-fields)
- [Improvement 6: Package Distribution References](#improvement-6-package-distribution-references)
- [Improvement 7: Sanctioned Mirror Declarations](#improvement-7-sanctioned-mirror-declarations)
- [Full Example](#full-example)
- **Companion specifications:**
  - [Contribution Credit Registry API](#contribution-credit-registry-api-rough-outline)
  - [Registry Discovery Standard](#registry-discovery-standard-rough-outline) (includes [Usage Registry API](#usage-registry-api), [Organization-Level Usage Declarations](#organization-level-usage-declarations), and [Project-Level Funding Declarations](#project-level-funding-declarations))
  - [Assessment Registry API](#assessment-registry-api-proposed)
- **Improvements awaiting consensus:**
  - [Improvement 8: CRA Steward Declaration](#improvement-8-cra-steward-declaration-deferred) (deferred pending CRA regulatory guidance)
  - [Improvement 9: Platform Lock-in Declaration](#improvement-9-platform-lock-in-declaration-proposed) (proposed, seeking community input)

---

## Design Principles

1. **publiccode.yml is the anchor point.** It lives in the repository and is maintained by the project team. It describes the project itself and points to external systems where appropriate; it doesn't duplicate dynamic data.

2. **Keep only what changes rarely.** Store information that changes with every release (version numbers, release dates) elsewhere—in GitHub releases, package registries, and Software Bill of Materials (SBOMs). Keep only data that humans maintain and that doesn't age between releases: classification, contacts, legal information, and pointers to external registries.

3. **Only include data the project controls.** A project can endorse which credit registries track its contributors (the project knows who contributes). A project cannot endorse usage registries (the project doesn't control who uses it). Usage data belongs entirely outside the file.

4. **All publiccode.yml metadata are project assertions.** Fields in publiccode.yml are statements made by the project or its maintainers. Validation, verification, and trust scoring may be added by catalogs, registries, or other consumers, but those layers sit on top of the published metadata rather than changing its meaning in the file itself.

5. **Decentralized discovery: registries find projects, not vice versa.** Usage registries discover projects by reading their publiccode.yml `url` field. The Registry Discovery Standard (described below) makes registries themselves discoverable and crawlable.

6. **All external systems use standardized APIs.** This allows catalogs (EU OSS Catalogue, Developers Italia) to aggregate data from any registry that conforms to the standard, rather than building custom integrations.

7. **Classification uses multiple dimensions.** Replace flat category lists with faceted classification (domain, function, role, layer, audience). This enables richer discovery: "show me healthcare CRMs that run as standalone web applications."

8. **Companion standards inherit publiccode.yml's vocabulary.** All companion specifications — usage declarations, registry APIs, discovery manifests — reuse publiccode.yml's existing field names and controlled vocabularies wherever the concepts overlap. When a companion standard addresses the same concept from a different perspective (for example, a deploying organization declaring its use case rather than a project author declaring capabilities), we reuse the same field name with an explicit note about who is making the assertion. This avoids creating new names for the same concept. publiccode.yml is the established standard; everything else is built on top of it.

9. **No new legal obligations.** This proposal provides infrastructure to make existing legal obligations — under the CRA, NIS2, EMBAG, and similar frameworks — easier to demonstrate and verify. It does not add new requirements beyond what those laws already impose. Every field and mechanism described here is either optional or a technical way to meet obligations that already exist in law. Participation is voluntary at every level; higher trust levels unlock richer catalog filters but carry no legal requirements.

10. **Duplicate/conflicting metadata.** Catalogs and aggregators should detect and clearly present duplicate or conflicting publiccode.yml files for the same open source project, notify users, and provide a process for maintainers to resolve such conflicts.

11. **Future: linked data representation (deferred).** The linked-data ecosystem (CodeMeta, schema.org, Software Heritage) could work with publiccode.yml through [YAML-LD](https://www.w3.org/community/reports/json-ld/CG-FINAL-yaml-ld-20231206/). Crawlers could create linked data from plain YAML by applying a standard context. However, this feature is deferred to reduce complexity. The `url` field and any sanctioned mirrors (see [Improvement 7](#improvement-7-sanctioned-mirror-declarations)) are already dereferenceable identifiers suitable as `schema:sameAs` targets in news article entity markup — enabling any project with a publiccode.yml to be referenced from external sources without requiring a Wikidata entry. Wikidata QIDs, where they exist, are complementary aliases the catalog aggregates; they are enrichments, not prerequisites for visibility.

---

## Global Applicability

While this proposal is motivated by EU policy contexts (CRA, NIS2, CAIDA), the underlying principles are universal and region-agnostic:

- **Decentralized discovery**: Registries do not require centralized authority. Any country, organization, or coalition can operate registries using the Registry Discovery Standard.
- **Standardized APIs**: A country implementing its own procurement requirements (US lock-in mitigation, Japan data residency rules, India DPI validation) can use the same API contracts to aggregate data from independent registries.
- **Reusable methodologies**: The Contribution Credit Registry trust model, faceted classification schema, and supply chain reference taxonomy are blueprints that can be adapted for any jurisdiction's risk profile.
- **No central gatekeeper**: publiccode.yml remains decentralized. Each region can define its own classification facets, trust models, and policy mappings while using the same underlying YAML schema.

---

## Policy Context: Public Procurement and Open Source (Global and Regional)

### 1. The Infrastructure Gap

Across the EU and globally, a consistent gap exists between open source policy ambitions and practical implementation. Laws and mandates (e.g., "Public Money, Public Code", EMBAG, CRA, NIS2) require or encourage open source adoption, but **lack the technical infrastructure** to make these requirements actionable. This gap is not just about missing software—it is about missing, standardized, machine-readable metadata and discovery mechanisms that enable:

- Procurement offices to find, evaluate, and compare open source solutions
- Auditable evidence for compliance with legal and policy requirements
- Visibility into supply chain health, security, and sustainability
- Data-driven funding and policy decisions

Catalogs, registries, and standards like publiccode.yml, SBOM, and contribution credit/usage registries are the essential infrastructure that operationalizes these laws. Without them, policy remains aspirational and difficult to enforce or measure.

This is also the mechanism for changing procurement behavior in practice: a data-driven evidence layer makes it easier and safer for procurement officers to deviate from incumbent proprietary defaults, because decisions can be justified through auditable criteria rather than relationship-based precedent.

The improvements proposed here are not merely technical changes to a metadata standard—they are infrastructure for implementing the **"Public Money, Public Code"** principle at scale. The [FSFE's Public Money? Public Code! campaign](https://publiccode.eu/) demands that publicly financed software be released under open source licenses. Over 200 civil society organizations and more than 31,000 individuals have signed their open letter. But the principle creates a follow-on problem: once governments commit to open source, how do procurement offices actually **find, evaluate, and responsibly adopt** it?

This proposal answers that question. Each improvement directly serves a procurement need that existing standards leave unmet.

### 2. Measurement and Impact

The standards and infrastructure proposed—catalogs, registries, publiccode.yml, SBOMs, credit and usage registries—are precisely what enable measurement and impact assessment. They provide the machine-verifiable data needed for procurement scoring, compliance audits, and policy evaluation. The proposal's improvements are designed to make these processes reliable, scalable, and defensible.

In short, this proposal does not just improve metadata quality; it lowers the institutional and legal risk of open source procurement by making qualitative selection criteria evidence-backed and challenge-resistant.

### 3. Policy Gaps and Integration

While this proposal is designed to serve as the technical implementation layer for many existing and emerging laws (see Legislative Models below), there remain **policy gaps** that require attention. For example, most procurement frameworks still structurally favor price over qualitative criteria such as open source preference, security posture, or vendor expertise—even when such criteria are legally permitted. The proposal provides the evidence base to support these criteria, but policy changes may still be needed to ensure they are weighted appropriately in practice. Other gaps include the lack of explicit mandates for open source preference in some jurisdictions, and insufficient incentives for maintaining high-quality metadata.

---

### How the Improvements Serve Procurement

---

## Stakeholder Engagement and Governance

The success of this proposal depends on broad stakeholder engagement and robust governance. The following process is recommended:

1. **Spec Maintainer Buy-in:** Secure the involvement of publiccode.yml maintainers and other core specification stewards early. Determine the process for proposing and reviewing improvements, and ensure capacity for governance.
2. **Working Group Formation:** Establish a working group with representation from academic, vendor, procurement, registry operator, and policy communities, plus at least one publiccode.yml maintainer as liaison.
3. **Open Consultation:** Conduct open consultations with affected communities (e.g., through public calls, RFCs, or workshops) to gather feedback and build consensus.
4. **Transparent Decision-Making:** Publish a working group charter, meeting notes, and decision records. Use open platforms (e.g., GitHub, mailing lists) for proposals and discussions.
5. **Iterative Piloting:** Pilot improvements with real projects, registries, and procurement offices. Use feedback to refine the standards and processes before broad rollout.
6. **Ongoing Governance:** Define a clear process for future changes, dispute resolution, and onboarding of new stakeholders. Ensure governance remains transparent, inclusive, and adaptable as the ecosystem evolves.

This process is further detailed in the ROADMAP.md Phase 0 section and is critical for legitimacy, adoption, and long-term sustainability.

| Procurement need                | Improvement                                                                                                                  | What it enables                                                                                                     |
| ------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| **Finding software that fits**  | [Improvement 1: Faceted Classification](#improvement-1-faceted-classification-replacing-flat-categories)                     | Multi-dimensional discovery: "CRM for healthcare, web-based" instead of keyword guessing                            |
| **Assessing security posture**  | [Improvement 2: Supply Chain References](#improvement-2-supply-chain-references)                                             | One-click access to SBOMs, OpenSSF Scorecards, REUSE compliance, vulnerability disclosure policies                  |
| **Evaluating vendor expertise** | [Improvement 3: Vendor Credit System Discovery](#improvement-3-vendor-credit-system-discovery)                               | Verifiable contribution data — who actually maintains the software, not just who claims to                          |
| **Checking peer adoption**      | [Improvement 4](#improvement-4-deprecate-usedby) + [Registry Discovery Standard](#registry-discovery-standard-rough-outline) | Which peer organizations already deploy this software, verified by domain control                                   |
| **Trusting the metadata**       | [Improvement 5: Deprecate Temporal Fields](#improvement-5-deprecate-temporal-fields)                                         | Only slow-changing, human-authored data — eliminating the stale version numbers that erode trust in the entire file |

### Alignment with Procurement Criteria Frameworks

The [OSBA's selection criteria for sustainable procurement of open source software](https://osb-alliance.de/publikationen/veroeffentlichungen/selection-criteria-for-the-sustainable-procurement-of-open-source-software) proposes key criteria that procurement offices should evaluate. The improvements provide the data infrastructure to make each criterion machine-verifiable:

1. **Relationship with software community.** The `contributionCreditRegistries` field (Improvement 3) provides verifiable evidence of a vendor's upstream contributions — not self-reported claims, but project-endorsed credit data.
2. **Upstream publication of modifications.** Credit registries track contribution types (code, security, documentation). Procurement offices can verify that a vendor pushes patches upstream rather than maintaining proprietary forks.
3. **High-quality Level 3 support.** The `maintenance` section (already in publiccode.yml) combined with credit data shows whether a vendor employs core developers or has contractual relationships with maintainers.
4. **Supply chain security.** The `supplyChain` section (Improvement 2) provides direct links to SBOMs, security policies, and compliance status — exactly what the Cyber Resilience Act and SBOM documentation requirements demand.

### Why Verifiable Data Matters More Than Legal Permission

The Public Procurement Directives already allow contracting authorities to include qualitative criteria in award decisions. In practice, procurement remains price-driven — not because qualitative criteria are illegal, but because they are expensive to defend. Subjective vendor assessments create legal exposure: a disappointed bidder can challenge the basis of a qualitative judgment. The result is a structural bias toward price, where the evidentiary bar is clear and uncontestable.

Machine-verifiable registry data shifts this calculus. A procurement decision backed by an auditable credit registry query, a linked SBOM, and a domain-verified peer adoption count is defensible in a way that a narrative vendor evaluation is not. This proposal does not merely provide data for criteria that authorities already use — it lowers the legal and administrative cost of criteria they currently avoid. The limiting factor for qualitative criteria is institutional, not legal; standardized evidence removes the institutional barrier.

### Digital Commons & Digital Sovereignty: Government as Steward

This data infrastructure is not just about procurement efficiency—it is critical for government engagement as a steward of the digital commons and builder of digital sovereignty.

Governments creating open source policies and defending digital independence need visibility into three dimensions that this proposal addresses:

1. **Evidence of policy enforcement.** "Public Money, Public Code" mandates are aspirational without auditable evidence. Usage and credit registries turn policy into practice by making reuse and upstream contribution visible.

2. **Supply chain health and independence.** Resilient digital sovereignty requires knowing which software is widely deployed, which has fragile vendor ecosystems, and which critical infrastructure lacks sufficient upstream support. The metadata infrastructure proposed here makes this visible.

3. **Evidence-based investment.** Funding allocation can shift from anecdote-driven ("project X is important") to data-driven ("X is deployed across multiple public agencies"). Usage registries and credit registries enable this transparency.

This is a distinct role from the CRA stewards discussed in the Legislative Models section below. While the Cyber Resilience Act assigns stewardship responsibilities to _commercial entities_ maintaining individual OSS projects, governments stewarding shared infrastructure need ecosystem-wide visibility—not just individual project compliance, but cross-cutting health, adoption patterns, and sustainability.

### Legislative Models

Government OSS preference and procurement policies have been documented across 80+ countries for over 15 years [(CSIS, 2010)](https://www.csis.org/analysis/government-open-source-policies); the EU legislative wave of the 2020s is the most binding expression of a longstanding global policy direction. Several legislative initiatives create immediate demand for the infrastructure proposed here:

**Mandate-driven policy:** Switzerland's [EMBAG law](https://www.fedlex.admin.ch/eli/cc/2023/682/en) (2023) mandates open source release of publicly financed software. The [APELL initiative](https://apell.info/) and [EuroStack coalition](https://eurostack.eu/) advance similar EU-level mandates. **The problem:** these mandates are unenforceable without metadata infrastructure. Procurement offices cannot operationalize "prefer open source" without discovery and evaluation tools. **The infrastructure this proposal provides:** faceted classification and supply chain references make the mandate actionable.

**Compliance-driven obligation: Cyber Resilience Act (CRA).** The [CRA](https://digital-strategy.ec.europa.eu/en/policies/cra-open-source) introduces the _open-source software steward_ — a legal entity that must demonstrate cybersecurity policy, vulnerability handling, and incident reporting to regulators on request. The `supplyChain` fields (`sbom`, `securityPolicy`, `scorecard`) are the natural machine-discoverable evidence layer; a project-endorsed credit registry record provides empirical evidence of "providing sustained support." This is discussed further in [Improvement 8: CRA Steward Declaration](#improvement-8-cra-steward-declaration-deferred).

**Supply chain risk mandate: NIS2 Directive.** The [NIS2 Directive](https://digital-strategy.ec.europa.eu/en/policies/nis2-directive) requires covered organizations to assess the supply chain security of software they adopt. Without standardized metadata, that assessment is expensive or impossible. The `supplyChain` references (SBOM, security policy, Scorecard) provide structured evidence from a single entry point; the `.well-known/publiccode-usage.json` declaration gives organizations a natural audit trail for their software inventory obligations.

For a detailed treatment of each initiative — including the forthcoming Public Procurement Act and the Interoperable Europe Act already in force — see [EU_POLICY_CONTEXT.md](EU_POLICY_CONTEXT.md).

### Sustainable Business Model Alignment

Open source infrastructure has no direct business model through license fees — it survives through donations and corporate sponsorship rather than revenue tied to the value it delivers or indirect funding through support and development contracts allows companies to direct some funds to maintenance. [Analysis of this structural problem](https://dri.es/open-source-infrastructure-deserves-a-business-model) points to a clear solution: connect infrastructure funding to usage and organizational benefit. This is not possible without visibility into _who_ uses what and at what scale.

The proposed infrastructure provides the necessary foundation:

- **Usage registries** make organizational consumption visible — enabling data-driven procurement decisions and, critically, enabling **ecosystem funds** like the [Open Source Endowment (OSE)](https://endowment.dev/) and the Sovereign Tech Fund to allocate resources based on measurable adoption and ecosystem impact rather than anecdote.
- **Contribution credit registries** create a traceable chain from organizational benefit to ecosystem stewardship: vendors demonstrate value through auditable upstream contribution, giving them an ROI case for maintenance investment.
- **Supply chain references** (SBOMs, Scorecard, security policies) reduce compliance verification costs for organizations with formal audit obligations.
- **Faceted classification** enables efficient filtering across the available open source landscape.

Without this infrastructure, open source procurement remains inefficient and funding allocation remains subjective. Short term price-driven procurement favors large proprietary vendors - further entrenching lock-in and foreign influence and control, where open source would allow for sovereignty.

---

## Actors and Relationships

The ecosystem this proposal addresses brings together different actors, each with distinct authority and information needs. Here's who needs what, and why:

| Actor                                 | Role                                                                                                                                                | Example                                                                             |
| ------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| **Open Source Project**               | Publishes code and metadata. Decides project classification and which contributions it recognizes.                                                  | Drupal, Nextcloud, OpenDesk                                                         |
| **Maintainer**                        | Day-to-day steward who authors and commits the publiccode.yml file. May work for a vendor, agency, or as an independent contributor.                | An individual or core team                                                          |
| **Vendor**                            | Contributes code to projects and sells related services. Wants to be findable to procurement professionals evaluating their expertise.              | Consulting firms specializing in specific platforms                                 |
| **Procurement Office**                | Searches for suitable software, evaluates vendor expertise and security, makes purchasing decisions.                                                | Municipal IT departments, federal agencies                                          |
| **Deploying Organization**            | Actually runs the software in production. Wants to declare what it uses.                                                                            | Cities, universities, government agencies                                           |
| **Federal Authority / Funder**        | Allocates money, influences policy, identifies ecosystem gaps. Makes data-driven funding decisions based on usage and impact metrics.               | Sovereign Tech Fund, CISA, digital sovereignty offices, Open Source Endowment (OSE) |
| **Ecosystem Fund / Allocation Agent** | Uses usage registries and credit data to allocate resources to underfunded but critical projects based on measurable adoption and ecosystem impact. | Open Source Endowment fund distribution model, Sovereign Tech Agency Fund           |
| **Policy Maker / Legislator**         | Writes laws and regulations that mandate or encourage open source. Needs metadata to make compliance verifiable.                                    | National parliaments, EU Commission                                                 |
| **Contribution Credit Registry**      | Tracks contributions to projects and creates vendor reputation data. Endorsed by projects.                                                          | Drupal.org Marketplace, ecosyste.ms funding platforms                               |
| **Usage Registry**                    | Tracks which organizations deploy which software. Independent from projects.                                                                        | Developers Italia, EU OSS Catalogue                                                 |
| **Software Catalog / Crawler**        | Aggregates metadata into searchable indexes for procurement and policy.                                                                             | EU OSS Catalogue, Developers Italia                                                 |

### Who Asserts What

The architecture depends on each piece of data being asserted by the actor who actually has authority over it:

```
 ┌─────────────────────────────────────────────────────────────┐
 │                    publiccode.yml                           │
 │                  (in the repository)                        │
 │                                                             │
 │  Asserted by: Project / Maintainer                         │
 │  Contains:                                                  │
 │    - What the software is (classification, description)     │
 │    - How it's maintained (contacts, contractors)            │
 │    - Where to find supply chain data (SBOM, scorecard)      │
 │    - Which contribution credit registries the project endorses │
 └──────────────────────────┬──────────────────────────────────┘
                            │
                            │ contributionCreditRegistries
                            ▼
              ┌────────────────────────┐        ┌────────────────────────────┐
              │ Contribution Credit     │        │    Usage Registries        │
              │ Registries             │        │                            │
              │  Asserted by:          │        │  Asserted by:              │
              │   Registry, endorsed   │        │   Deploying organizations, │
              │   by project           │        │   verified by registry     │
              │                        │        │                            │
              │  Contains:             │        │  Contains:                 │
              │   - Who contributes    │        │   - Who uses what          │
              │   - Contribution type  │        │   - Since when             │
              │   - Vendor rankings    │        │   - At what scale          │
              └────────────┬───────────┘        └──────────────┬─────────────┘
                           │                                   │
                           │     No link from publiccode.yml   │
                           │     Discovered via Registry       │
                           │     Discovery Standard ──────────►│
                           │                                   │
                           └──────────────┬────────────────────┘
                                          ▼
                           ┌──────────────────────────────┐
                           │   Software Catalog / Crawler │
                           │                              │
                           │  Aggregates:                 │
                           │   - publiccode.yml data      │
                           │   - Contribution credit data │
                           │   - Usage/adoption data      │
                           │                              │
                           │  Serves:                     │
                           │   - Procurement officers     │
                           │   - Federal authorities      │
                           │   - Funders                  │
                           └──────────────────────────────┘
```

### Why Contribution Credits and Usage Are Architecturally Different

A critical distinction: **who has authority over the data**.

|                                  | Contribution credit registries                                               | Usage registries                                                        |
| -------------------------------- | ---------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| **What's tracked**               | Who contributes to projects and how much                                     | Which organizations deploy projects                                     |
| **Who knows this?**              | The project maintainers                                                      | The deploying organizations                                             |
| **Listed in publiccode.yml?**    | Yes — as endorsements                                                        | No — the project doesn't control this data                              |
| **How crawlers find registries** | From publiccode.yml `contributionCreditRegistries` field and Registry Discovery Standard | From Registry Discovery Standard only                                   |
| **Data entry mechanism**         | Central registry maintains the data                                          | Direct declarations from deploying organizations and automated crawling |

| **Example** | Drupal.org contribution credits showing which vendors employ core contributors | Developers Italia showing which Italian municipalities use Nextcloud |

This separation keeps each actor in control of the claims they can actually back up.

---

## Improvement 1: Faceted Classification (replacing flat `categories`)

The current `categories` field is a flat list of ~100 values (like `content-management`, `crm`, `website`). This makes it impossible to express specific combinations. You can't say "a healthcare CRM that's a plugin library for backend use" — you'd pick `crm`, lose the other dimensions, and leave procurement officers guessing.

**Proposal:** Replace `categories` with a `classification` section organized by the six dimensions from the OSS Taxonomy. Keep `categories` as a deprecated fallback during transition.

### Schema

```yaml
# New faceted classification (replaces `categories`)
classification:
  # WHAT industry or field does this software serve?
  # Controlled vocabulary, multiple allowed.
  # Examples: blockchain, cloud-computing, content-management, data-science,
  # database, devops, embedded-systems, game-development, machine-learning, web-development
  domain:
    - content-management
    - web-development

  # WHAT role does this software play architecturally?
  # Controlled vocabulary, multiple allowed.
  # Examples: application, build-tool, cli-tool, framework, library,
  # orchestrator, package-manager, plugin, platform
  role:
    - framework
    - platform
    - library

  # WHAT specific capabilities or tasks does it enable?
  # Controlled vocabulary, multiple allowed.
  # Examples: api-development, authentication, caching, ci-cd,
  # containerization, database-management, deployment, logging
  function:
    - authentication
    - caching
    - database-management

  # WHERE in the technology stack does it operate?
  # Controlled vocabulary, multiple allowed.
  # Examples: backend, data-layer, frontend, full-stack, infrastructure,
  # middleware, operating-system, platform
  layer:
    - backend
    - frontend
    - full-stack

  # WHO is the intended audience?
  # Controlled vocabulary, multiple allowed.
  # Examples: content-creator, data-scientist, designer, developer,
  # educator, end-user, enterprise, researcher, student
  audience:
    - developer
    - enterprise

  # WHAT technologies does this project use or support?
  # Controlled vocabulary, multiple allowed.
  # Examples: python, javascript, rust, c, cpp, aws, docker, kubernetes, postgresql
  technology:
    - php
    - javascript
    - postgresql

  # Free-form tags for anything not covered by the controlled
  # vocabularies above. Uses the namespace:value convention
  # from OSS Taxonomy for forward compatibility.
  tags:
    - "compatibility:drupal"
    - "compliance:gdpr"
```

### Migration Path from Current Schema

| Current field              | Maps to                   | Notes                                                                                     |
| -------------------------- | ------------------------- | ----------------------------------------------------------------------------------------- |
| `categories`               | `classification.function` | Old values like "content-management", "authentication" become function vocabulary.        |
| `softwareType`             | `classification.role`     | Map standalone/web → standalone-web, etc.                                                 |
| `intendedAudience.scope`   | `classification.audience` | Existing scope values map to audience (e.g., "enterprise" → audience: enterprise).        |
| New mapping (no old field) | `classification.domain`   | Industry/field the software serves. Determined from README, description, or OSS Taxonomy. |

### Why This Matters for Procurement

Procurement officers need to answer questions like:

- "Show me all software in the content-management domain that runs as a full-stack platform for developers."
- "Which authentication libraries have active vendor ecosystems supporting them?"
- "What backend infrastructure tools are available for cloud-computing deployments?"

With faceted classification, an officer can query across multiple dimensions simultaneously. With flat categories, they can only search one dimension at a time and manually filter the rest. The OSS Taxonomy's six dimensions (domain, role, function, layer, audience, technology) enable rich combinatorial discovery.

### Vocabulary Governance

The controlled vocabularies for `domain`, `function`, `role`, `layer`, and `audience` should be maintained as a separate, versioned artifact (like the current [categories list](https://yml.publiccode.tools/categories-list.html)) with:

- A community contribution process via pull requests (as [OSS Taxonomy](https://nesbitt.io/2025/11/29/oss-taxonomy.html) does)
- Each term having: `name`, `description`, `examples`, `related`, `aliases`
- Versioned releases so that publiccode.yml files can reference a vocabulary version

The `tags` field is intentionally free-form to allow experimentation. Tags that gain traction get promoted into controlled vocabularies in future versions.

### Reducing the Maintainer Classification Burden

Faceted classification asks more of maintainers than the current flat `categories` list. Two mechanisms can reduce this friction:

1. **AI-assisted tooling.** The [publiccode.yml editor](https://github.com/italia/publiccode-editor) could offer an AI-powered classification assistant that reads the repository README and description, proposes classification values, and lets the maintainer confirm or adjust. This lowers the burden from "fill in six fields from scratch" to "review and accept suggestions."
2. **Crawler-assisted enrichment.** Catalog crawlers (the EU OSS Catalogue, Developers Italia) can derive classification from existing metadata — README keywords, package registry tags, GitHub topics — and store it at the catalog layer without requiring the maintainer to maintain it in the file. This keeps the file minimal while making faceted search available to catalog users.

Both approaches respect the principle that maintainers should only be asked to author data that genuinely requires their judgment.

---

## Improvement 2: Supply Chain References

New top-level section pointing to supply chain artifacts. These are URLs to externally-hosted resources, not inline data.

### Schema

```yaml
supplyChain:
  # SBOM attestation(s) — one entry per SBOM format you publish.
  # Fields mirror the Security Insights #Attestation type so a project
  # publishing both files can use consistent vocabulary:
  #
  #   name:         Human-readable label (e.g. "Release SBOM (CycloneDX)")
  #   predicate-uri: URI identifying the attestation type:
  #                   CycloneDX → https://cyclonedx.org/bom
  #                   SPDX      → https://spdx.dev/Document
  #   location:     URL template for the artifact. Use {version} as the
  #                 placeholder; consumers substitute the tag they are
  #                 evaluating. Works identically on GitHub, GitLab,
  #                 Codeberg/Forgejo, and any forge using standard
  #                 release attachments. Document the latest-release
  #                 stable URL in the comment field.
  #   comment:      Optional notes — document the `latest` stable URL here.
  #
  # Omit the block entirely if you do not publish SBOMs.
  sbom:
    - name: Release SBOM (CycloneDX)
      predicate-uri: https://cyclonedx.org/bom
      location: https://forge.example.org/owner/project/releases/{version}/sbom.cdx.json
      comment: Replace {version} with the release tag. Latest always at releases/latest/sbom.cdx.json
    - name: Release SBOM (SPDX)
      predicate-uri: https://spdx.dev/Document
      location: https://forge.example.org/owner/project/releases/{version}/sbom.spdx.json
      comment: Replace {version} with the release tag. Latest always at releases/latest/sbom.spdx.json

  # OpenSSF Scorecard — machine-readable API URL to the project's scorecard
  # results. Scorecard supports GitHub and GitLab hosted projects:
  #   https://api.scorecard.dev/projects/{forge}/{owner}/{repo}
  # Human-readable viewer: https://scorecard.dev/viewer/?uri={forge}/{owner}/{repo}
  scorecard: https://api.scorecard.dev/projects/forge.example.org/owner/project

  # Security policy — URL to the project's vulnerability disclosure policy.
  # Prefer the RFC 9116 well-known endpoint (machine-parseable, forge-agnostic):
  #   https://example.org/.well-known/security.txt
  # Acceptable fallbacks: a forge security page, a SECURITY.md, or any
  # dedicated security page — but RFC 9116 is the only standardized,
  # machine-readable format.
  securityPolicy: https://example.org/.well-known/security.txt

  # REUSE compliance — URL to the machine-readable REUSE lint API result.
  # Pattern: https://api.reuse.software/status/{forge}/{owner}/{repo}
  reuse: https://api.reuse.software/status/forge.example.org/owner/project
```

### Design Rationale

- **SBOMs are release artifacts**, not source-tree files. They change with every release. The `sbom` field points to where the latest SBOM can always be found. The per-entry structure (`name`, `predicate-uri`, `location`, `comment`) mirrors the in-toto `#Attestation` type used in [Security Insights](https://github.com/ossf/security-insights) `repository.release.attestations`, so a project that publishes both files can express SBOM attestations consistently across both schemas. Standard predicate URIs: `https://cyclonedx.org/bom` for CycloneDX, `https://spdx.dev/Document` for SPDX. The `location` field is a plain URL; SBOMs stored in OCI registries and discovered via the [manifest referrers API](https://github.com/opencontainers/distribution-spec/blob/main/spec.md#listing-referrers) are out of scope — their discovery requires OCI-native tooling rather than a simple HTTP fetch.
- **Scorecard results are computed externally** by the OpenSSF infrastructure. The field points directly to the machine-readable [Scorecard API](https://api.scorecard.dev/), which returns structured JSON. The human-readable viewer is documented in the comment for maintainers who want to verify their score manually.
- **`securityPolicy` is intentionally format-agnostic.** `SECURITY.md` is a widely-adopted convention for documenting vulnerability disclosure procedures, but it has no formal specification — its content is free-form prose. The more rigorous alternative is [RFC 9116 `security.txt`](https://www.rfc-editor.org/rfc/rfc9116), an IETF standard that defines a machine-parseable file at `/.well-known/security.txt` for declaring vulnerability disclosure contacts, preferred languages, and policy URLs for a domain. The `securityPolicy` field accepts any URL — a forge security page, a `/.well-known/security.txt` endpoint, or a project's dedicated security page — because prescribing the format would exclude projects that follow RFC 9116 rather than the `SECURITY.md` convention. Crawlers that want to parse disclosure metadata programmatically should prefer projects that publish an RFC 9116-compliant endpoint. Projects that also publish a Security Insights file should use the same URL for both `supplyChain.securityPolicy` here and `project.vulnerability-reporting.policy` there.
- **REUSE compliance** is already checked by Developers Italia badges. Making it an explicit field in publiccode.yml formalizes what's already practiced.
- **Relationship to the upcoming `supports` key.** The publiccode.yml maintainers are considering a generic `supports` key for declaring policy compliance and security frameworks in a future spec version. The `supplyChain` fields proposed here are intentionally URL-based and reference external standards (SBOMs, Scorecard, REUSE) rather than defining new inline vocabulary — making them compatible with whatever form `supports` takes when it is proposed. The `supplyChain` section is intentionally scoped to supply-chain and compliance artifacts; sustainability and accessibility policy declarations (emerging conventions such as `SUSTAINABILITY.md` and `ACCESSIBILITY.md`) belong in `supports` rather than as additional `supplyChain` subfields.

### Accessibility Declaration Profile (for `supports`)

The accessibility use case raised by public-sector catalogs is a strong fit for the future `supports` key. As with all publiccode.yml fields, accessibility metadata is a maintainer assertion by default.

**OSS as a baseline, not a finished product.** Open source software is rarely deployed without customization: agencies configure, theme, and extend it before going live. A CMS adopted by a ministry is not the same product as the upstream OSS project. This distinction matters for procurement: a "Product Accessibility Baseline" should capture what the project delivers out of the box, what it enables implementers to build on, and what evidence exists from real deployments — not just a summary claim. This profile is designed to express all three layers, so that procurement offices can assess risk and residual implementation effort before custom development begins.

The design goal is **progressive detail**: a simple summary for most projects, with optional fields for the additional information procurement offices need when assessing complex, multi-surface software.

```yaml
supports:
  accessibility:
    # Summary for quick catalog filtering.
    # Example values: full | partial
    coverage: partial

    # VPAT/ACR — link to a Voluntary Product Accessibility Template or
    # Accessibility Conformance Report. The gold standard for procurement offices.
    # May be a repository-relative path or an absolute URL.
    vpat: docs/a11y/VPAT-2026-03.md

    # Direct link to the project's issue tracker filtered to accessibility issues.
    # Signals transparency; lets procurement offices assess the known-bug backlog.
    issueQueue: https://github.com/example/project/issues?label=accessibility

    # Who procurement offices can contact for accessibility questions.
    # May be a role address, a named maintainer, or a team alias.
    maintainerContact: accessibility@example.org

    # Defaults apply unless overridden in assertions[].
    defaults:
      # Example values: EN-301-549, WCAG-2.1, WCAG-2.2
      standard: EN-301-549
      # Example values: A, AA, AAA, equivalent-national-profile
      level: AA
      # Example values: self-assessment, third-party-audit, public-authority-audit
      reviewerType: self-assessment

    # Optional per-surface overrides for mixed-standard or mixed-level reality.
    # If omitted, catalogs and consumers use defaults.
    # Surface vocabulary: web-ui, mobile-ios, mobile-android, desktop-ui, docs,
    # pdf-outputs, email-templates, starter-theme, admin-ui
    assertions:
      - surface: web-ui
        standard: WCAG-2.2
        level: AA
        reviewerType: third-party-audit
        evidence:
          - type: test-report
            reference: docs/a11y/web-audit-2026-03.md

      - surface: docs
        standard: WCAG-2.1
        level: A

      # starter-theme captures the "out-of-the-box" state a new deployment inherits.
      - surface: starter-theme
        standard: WCAG-2.2
        level: AA
        reviewerType: self-assessment
        evidence:
          - type: test-report
            reference: docs/a11y/starter-theme-audit-2026.md

    # Human-readable statement location, relative or absolute.
    # A project may point to ACCESSIBILITY.md or an equivalent statement.
    statement: ACCESSIBILITY.md

    # Date and software version of the latest assertion update.
    # lastReviewedVersion anchors the audit to a specific release, preventing
    # stale claims from being applied to later versions.
    lastReviewed: 2026-03-01
    lastReviewedVersion: "10.2.0"

    # Automated testing — whether the CI/CD pipeline runs accessibility checks.
    # Presence of this block signals that regressions are actively prevented.
    automatedTesting:
      # Example values: axe-core, pa11y, playwright-axe, deque-cli
      tool: axe-core
      # Path or URL to the CI configuration running the checks.
      ciReference: .github/workflows/accessibility.yml

    # Implementation enablement — how the project supports implementers in
    # building accessible deployments. Relevant when the OSS is a "base" that
    # agencies configure, theme, or extend before going live.
    implementation:
      # Does the admin/authoring interface enforce or guide accessible content
      # creation? (e.g., requiring alt text on images, validating heading structure)
      # Example values: yes | partial | no
      authoringToolSupport: partial
      # Component library or design system accessibility documentation URL.
      componentLibraryUrl: https://design.example.org/accessibility

    # URL to a milestone, roadmap issue, or equivalent live tracking artifact
    # for known critical accessibility issues.
    remediationRoadmap: https://github.com/example/project/milestone/42
```

The declaration model should remain simple and deterministic:

- If a surface is missing in `assertions[]`, `defaults` applies.
- If a surface appears in `assertions[]`, that entry overrides `defaults` for that surface only.
- If `assertions[]` is omitted entirely, the declaration is still valid and interpreted from `defaults`.
- All fields beyond `coverage` and `defaults` are optional; their presence signals maturity and transparency, not compliance.

This keeps the standard compact while still allowing catalogs and procurement tools to build simplified views on top of it.

### Design Rationale (Accessibility)

- **`vpat` carries more weight than structured fields for procurement.** ACRs/VPATs are the document type legal teams know how to evaluate; a pointer to one — even if partial — is a stronger signal than any set of machine-readable fields.
- **`issueQueue` signals transparency, not failure.** A project with a labeled issue queue is demonstrably tracking bugs. A project without one may have zero issues or no process — the absence is ambiguous; catalogs should surface the distinction.
- **`lastReviewedVersion` prevents stale reliance.** A `lastReviewed` date without a version anchor is meaningless when applied to a later release.
- **`automatedTesting` is a process signal; result artifacts belong in `assertions[].evidence`.** The block indicates that CI prevents regressions — not that all issues are resolved. Reports produced by automated runs can be linked as `type: automated-test-report` under the relevant surface assertion.
- **`remediationRoadmap` is a URL, not a date.** A target date in a static file goes stale the moment the roadmap slips; the milestone URL always reflects current status.

### What This Enables

1. **Procurement security assessment.** Before adopting software, a procurement officer can check its OpenSSF Scorecard, review the vulnerability disclosure policy, and verify SBOM availability — all from a single metadata file, without hunting across multiple external sources.
2. **Compliance verification.** The `reuse` field lets procurement offices confirm FSFE REUSE compliance (per-file licensing) as part of their legal due diligence, and the `securityPolicy` field confirms the project has a responsible disclosure process.
3. **Automated trust signals.** Crawlers (EU OSS Catalogue, Developers Italia) can fetch scorecard scores and REUSE status via the referenced URLs, enabling badges and filters like "show me only projects with an OpenSSF score above 7" or "only REUSE-compliant projects."
4. **CRA and NIS2 compliance evidence.** For projects operating under the [Cyber Resilience Act](https://digital-strategy.ec.europa.eu/en/policies/cra-open-source) as an open-source software steward, the `supplyChain` fields make the required security artifacts — cybersecurity policy, SBOM, vulnerability disclosure process — machine-discoverable without additional reporting overhead. For [NIS2](https://digital-strategy.ec.europa.eu/en/policies/nis2-directive)-covered deploying organizations, the same references satisfy supply chain risk assessment obligations for OSS components in their stack.

   The [OpenSSF OSPS Baseline](https://baseline.openssf.org/) makes this mapping concrete: it defines tiered security controls (maturity levels 1–3) that explicitly cross-reference CRA, NIS2, DORA, SLSA, SSDF, NIST 800-161, and BSI TR-03185-2. The [OSPS Baseline Scanner](https://github.com/ossf/security-baseline) reads `security-insights.yml` to assess compliance. The `supplyChain` fields map directly to specific OSPS controls:

   | `supplyChain` field   | OSPS control                                                     | Regulatory mapping   |
   | --------------------- | ---------------------------------------------------------------- | -------------------- |
   | `securityPolicy`      | OSPS-VM-01 — Publish Coordinated Vulnerability Disclosure Policy | CRA Art. 2.1–2.8     |
   | `scorecard`           | OSPS-SA — Security Assessment                                    | CRA Annex I, SSDF RV |
   | `sbom` (attestations) | OSPS-BR-06 — Include Signatures and Hashes With Release          | CRA Annex I §2, SLSA |

   For projects that publish a `security-insights.yml`, that file is the authoritative source for all of this data and catalog crawlers should read it directly. The `supplyChain` section here serves two more limited purposes: it is a lower-friction first step for projects that have not yet adopted `security-insights.yml` (four lines in an existing file, no new schema to learn), and a fallback for catalog crawlers that support publiccode.yml but are not yet capable of ingesting `security-insights.yml`. Maintaining the same URLs in both files is low effort given the aligned attestation shape (`name`, `predicate-uri`, `location`, `comment` — identical to the `#Attestation` type in Security Insights `repository.release.attestations`), and may be worthwhile.

5. **Accessibility pre-screening and procurement risk assessment.** The `supports.accessibility` declaration gives procurement offices a structured "Product Accessibility Baseline" — covering the project's out-of-the-box conformance (`coverage`, `defaults`, `vpat`), what it enables implementers to build on (`implementation`, `starter-theme` surface), and what testing prevents regressions (`automatedTesting`). Catalogs can present this as a comparable, filterable signal, reducing the risk of adopting software that fails barrier-free requirements and clarifying what residual work falls to the implementer.
6. **Vulnerability advisory routing.** Vulnerability databases ([EUVD](https://euvd.enisa.europa.eu), [CVE](https://www.cve.org), [OSV](https://osv.dev)) increasingly identify affected software by PURL. The package distribution references field is the link that closes the loop: advisory PURL → catalog entry → usage registry → deploying organizations. An NIS2-covered organization that has published `.well-known/publiccode-usage.json` can receive targeted advisory notifications for the exact packages it runs, without manual SBOM matching. This chain is currently impossible at scale; the proposal's infrastructure makes it a straightforward crawl query.

---

## Improvement 3: Vendor Contribution Credit System Discovery

The vendor/contributor contribution credit data itself is too dynamic to live in a git-committed file. Instead, publiccode.yml points to one or more **contribution credit registries** — external databases that track who contributes to the project and in what capacity.

Projects vary widely in how they track contributions. Drupal operates the most mature example: a [weighted contribution credit system](https://www.drupal.org/drupalorg/docs/marketplace/contribution-credit-weight-and-impact-on-ranking) backed by an [open-source, ticket-system-agnostic module](https://git.drupalcode.org/project/contribution_records) with an API — infrastructure that could in principle be adopted by other projects or operated as a SaaS. Other projects may prefer simpler approaches: GitHub Sponsors lists, or mapping code contributions to vendors through contributor employer declarations — but these capture only funding or commits, missing the contributors who sustain a project through testing, UX design, documentation, community organization, event work, or issue triage. The most complete contribution credit registries recognize this full spectrum; a registry that awards contribution credits only to financial sponsors or commit authors gives a systematically incomplete picture of who actually keeps a project alive. This proposal standardizes only the **read interface** for contribution credit data (the [Contribution Credit Registry API](#contribution-credit-registry-api-rough-outline)), not the methodology for earning or weighting credits — that is a per-project decision beyond the scope of this proposal.

### Who Operates a Contribution Credit Registry?

A contribution credit registry can be operated by the project itself, by a generic third-party platform, or by a SaaS provider. What matters is that the OSS project **endorses** it — by listing it in `contributionCreditRegistries`, the project signals "we consider this data authoritative for our contributors."

| Operator                         | Example                                                                                                                      | Sign-off requirement                                                                                 |
| -------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| **The project itself**           | Drupal.org Marketplace, a self-hosted `contribution_records` instance                                                        | Project defines its own process for reviewing and approving contribution credits                     |
| **Generic third-party platform** | GitHub Sponsors                                                                                                              | Pointing at the GitHub Sponsors URL is sufficient; no additional setup required                      |
| **SaaS / shared installation**   | A hosted `contribution_records` instance configured per-project, or a common credit platform that multiple projects opt into | Depends on what the platform offers; may range from fully automated to requiring maintainer sign-off |

The required depth of involvement scales with the registry's sophistication, but the spectrum is not flat — project sign-off is the goal, not just one option among equals. A project that points at a GitHub Sponsors page is endorsing a third-party platform's data without reviewing the underlying attribution: it is a low-friction starting point, not the intended end state. The ideal is for the project to take active responsibility for contribution credits: deciding what contribution types count (code, testing, design, organization, documentation, etc.), reviewing whether the data accurately reflects who sustains the project, and defining a process for sign-off. Projects that invest in this — whether through a custom registry, a SaaS platform with maintainer review, or a formal contribution credit policy — produce data that is meaningfully more trustworthy for procurement decisions and fairer to the full range of contributors.

### Schema

```yaml
# Pointers to external systems that track vendor/contributor
# contribution credits for this project. Each entry identifies a
# registry and this project's identifier within that registry.
contributionCreditRegistries:
  - # Human-readable name of the registry
    name: Drupal.org Marketplace
    # URL to this project's contribution credit page in the registry
    url: https://www.drupal.org/project/drupal/credit
    # API endpoint conforming to the Contribution Credit Registry API spec
    # (see "Contribution Credit Registry API" section below)
    apiUrl: https://www.drupal.org/api/v1/credits/project/drupal
    # Type of registry — helps crawlers understand what data
    # to expect
    type: contribution-credits # contribution-credits | vendor-directory | funding

  - name: ecosyste.ms
    url: https://ecosyste.ms/projects/lookup?url=https://forge.example.org/owner/project
    apiUrl: https://funds.ecosyste.ms/api/v1/projects/forge.example.org/owner/project
    type: funding

  - name: Open Source Pledge
    url: https://opensourcepledge.com/projects/example
    apiUrl: https://opensourcepledge.com/api/v1/projects/example
    type: funding
```

### What This Enables

1. **Procurement officers** can follow the `contributionCreditRegistries` links to see which vendors actively contribute to the software they're evaluating — similar to checking the [Drupal Marketplace](https://www.drupal.org/drupalorg/docs/marketplace) before hiring an agency.
2. **Crawlers** (EU OSS Catalogue, Developers Italia) can aggregate credit data across registries to build composite vendor profiles.
3. **Projects** control which registries they recognize by listing them in their publiccode.yml — this is a form of endorsement by the project maintainers.

---

## Improvement 4: Deprecate `usedBy`

The current `usedBy` field is a simple list of organization names maintained by the project. This has fundamental problems:

- **It's unverifiable** — anyone can claim usage
- **It's high-friction** — organizations must ask projects to update their file
- **It's always stale** — organizations adopt and retire software without notifying projects

As described in [Actors and Relationships](#actors-and-relationships), usage data is asserted by deploying organizations, not by projects. The project has no authority over who uses it. Usage tracking therefore lives entirely outside publiccode.yml, in decentralized [usage registries](#registry-discovery-standard) that index projects by their `url` field. This aligns with [publiccodeyml PR #187](https://github.com/publiccodeyml/publiccode.yml/pull/187), which proposes deprecating `usedBy` in the spec itself, with usage data moving to the catalog-api layer.

### Schema Change

```yaml
# Deprecated — retained for backward compatibility only.
# Usage data comes from external usage registries that
# independently index projects by their publiccode.yml url.
# See: Registry Discovery Standard
usedBy:
  - Comune di Roma
  - Stadt München
```

---

## Improvement 5: Deprecate Temporal Fields

Across the thousands of publiccode.yml files indexed by ecosyste.ms, most are out of date because nobody remembers to update a metadata file between releases. The fields most responsible for staleness are those that track information already available from authoritative sources — forge APIs, package registries, and release artifacts.

### Fields to Deprecate

| Field                             | Why it goes stale                                                                         | Authoritative source                                                                                                                                                                    |
| --------------------------------- | ----------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `softwareVersion`                 | Changes with every release; rarely updated in publiccode.yml                              | Forge API (GitHub releases, GitLab tags), package registry                                                                                                                              |
| `releaseDate`                     | Same as above — coupled to `softwareVersion`                                              | Forge API, package registry                                                                                                                                                             |
| `dependsOn[].versionMin`          | Dependency minimum versions evolve with each release                                      | Package manager lockfiles, SBOM (already referenced in `supplyChain`)                                                                                                                   |
| `maintenance.contractors[].until` | Contract expiry dates are rarely updated; create a false impression of active maintenance | Internal contract management systems; out of scope for a metadata file. For deployment-level contractor tracking, use `software[].contractors[]` in `.well-known/publiccode-usage.json` |

### Schema Change

```yaml
# Deprecated — version and release data should be consumed from
# the project's forge API or package registry, not maintained
# manually in a metadata file.
# softwareVersion: "3.2.1"
# releaseDate: "2026-01-15"

# Deprecated — dependency version constraints are better expressed
# in package manager files and SBOMs (see supplyChain.sbom).
# dependsOn:
#   open:
#     - name: PostgreSQL
#       versionMin: "14"

# contractors[].until deprecated — contract expiry dates are rarely
# maintained and create false impressions of active support.
# contractors[].identifier added (optional) — links inline contractor
# declarations to credit registry profiles.
maintenance:
  contractors:
    - name: Acme GmbH
      website: https://acme.example.org
      identifier: "LEI:XXXXXXXXXXXXXXXXXXXX" # new optional field
      # until: "2027-12-31"                   # deprecated
```

### Design Rationale

**Keep only what humans must author.** The remaining publiccode.yml content falls into four categories of data that cannot be reliably extracted from other sources:

1. **Classification** — project categorization unavailable from forges or registries (the `classification` section)
2. **Contacts and maintenance** — support contacts, maintenance arrangements, contractor information
3. **Legal** — license and copyright declarations (beyond what REUSE/SPDX cover at the file level)
4. **Registry pointers** — links to credit registries, supply chain artifacts, and security policies

These are slow-changing, human-maintained fields. They don't become stale between releases because they don't change with releases. Removing temporal fields reduces the maintenance burden on maintainers (addressing risk A3) and eliminates the most common source of inaccurate data in the ecosystem.

**Note:** The `dependsOn` field retains value as a human-readable indicator of major runtime dependencies (e.g., "this software needs PostgreSQL") even without version constraints. Projects may choose to keep `dependsOn` with dependency names but without `versionMin`/`versionMax` — the version detail belongs in the SBOM referenced by `supplyChain.sbom`.

**`contractors[]` as the minimal inline credit registry.** The `usedBy` field (deprecated in Improvement 4) and `contractors[]` might look similar — both list external entities inline. But `usedBy` listed something externally observable (which organizations deploy the software), making the inline declaration redundant. `contractors[]` lists a support relationship that only the project can declare.

Structurally, `contractors[]` is the inline, project-asserted minimal form of a contribution credit registry entry. A contractor is simply an entity with a `support-contract` relationship to the project — equivalent to a contribution credit registry entry with `roles: ["support-contract"]`. For projects that do not operate or endorse a contribution credit registry, `contractors[]` is the appropriate lightweight mechanism. For projects that do have a contribution credit registry, support contractors should be listed there (with `roles: ["support-contract"]`) and `contractors[]` may be omitted or used as a convenience mirror. The Contribution Credit Registry API includes `support-contract` as a valid role type alongside `code`, `maintainer`, `funding`, and others (see [Contribution Credit Registry API](#contribution-credit-registry-api-rough-outline)).

Only `contractors[].until` is deprecated because contract expiry dates create a false impression of maintenance status while being essentially unmaintained in practice. For organizations that need to document which vendor holds their support contract for a specific deployment — for example, to satisfy DORA Article 28 register requirements — that information belongs in the deploying organization's `software[].contractors[]` entry in `.well-known/publiccode-usage.json`, not in the project's metadata file.

**Adding optional contractor identifiers.** For consistency with `maintenance.steward.identifier` and credit registry entity records, this improvement adds an optional `identifier` field to contractor entries. This allows a contractor's inline declaration to be cross-referenced with their credit registry profile — useful when both exist. The field follows the same `SCHEME:VALUE` convention used elsewhere:

```yaml
maintenance:
  contractors:
    - name: Acme GmbH
      website: https://acme.example.org
      identifier: "LEI:529900T8BM49AURSDO55" # optional; LEI preferred for legal entities
      # until: "2027-12-31" — deprecated, see Fields to Deprecate above
```

This is a non-breaking addition; existing files that omit `identifier` remain valid.

---

## Improvement 6: Package Distribution References

An open source project is not the same thing as a package. Nextcloud Server is a project; `pkg:deb/debian/nextcloud-server`, `pkg:docker/nextcloud/nextcloud`, and `pkg:github/nextcloud/server` are three distributions of that project across different delivery channels. Anyone can publish a package using a well-known project's name — including malicious actors creating typosquat packages or unofficial forks. Today, there is no machine-readable way to distinguish an official distribution from an unauthorized one.

At the same time, the catalog and SBOM worlds use incompatible identifiers for the same underlying software:

- **Catalogs and usage registries** identify projects by their source repository URL (the `url` field in publiccode.yml).
- **SBOMs** (CycloneDX, SPDX) and **dependency graphs** identify components by [PURL (Package URL, ECMA-424)](https://github.com/package-url/purl-spec) — a standard notation encoding the ecosystem, package name, and optionally version: `pkg:type/namespace/name@version`.

Without a bridge between these two identifier spaces, it is impossible to automatically determine whether the `pkg:deb/debian/nextcloud-server` component in an NIS2 supply chain audit is the same software as the Nextcloud entry in a procurement catalog. A vulnerability advisory for a npm package cannot be traced to which catalogued projects are affected. An SBOM consumer cannot cross-reference a component to its procurement or vendor metadata.

**Proposal:** Add an optional `package_repositories` field that lists the project's own, officially endorsed package repositories and distributions. Because this list lives in the project's source repository — alongside the code it describes — it carries the same authorial authority as the rest of publiccode.yml. The syntax is progressive: maintainers can start with minimal PURLs and add optional detail (distro versions, repository URLs, architecture support) as needed.

### Schema

```yaml
package_repositories:
  # Required: PURL identifying the official distribution.
  # Optional: distros (list of release versions), repository (non-default repo URL),
  # status (active | deprecated).
  - purl: pkg:npm/nextcloud
  - purl: pkg:docker/nextcloud/nextcloud

  # With optional distro list for Debian/OS packages:
  - purl: pkg:deb/debian/nextcloud-server
    distros: [bullseye, bookworm, trixie]

  # With optional custom repository URL:
  - purl: pkg:deb/ubuntu/nextcloud
    repository: https://ppa.launchpad.net/nextcloud-team/releases/ubuntu
    distros: [focal, jammy, noble]

  # With detailed distro support matrix (optional):
  - purl: pkg:deb/debian/postgresql
    distros:
      - bullseye
      - name: bookworm
        architectures: [amd64, arm64]
        version_constraint: ">=13.0"

  # Optional status field for deprecated distributions:
  - purl: pkg:npm/nextcloud-legacy
    status: deprecated
```

Catalogs should surface deprecated entries as a warning rather than silently dropping them, since consuming organisations may still be using the package and need to know support has ended.

### Design Rationale

**Authority via source control.** Because `package_repositories` is asserted by the project in its own repository, it becomes the canonical list of official distributions — exactly the data that auto-discovery services cannot provide. ecosyste.ms and deps.dev can tell you that a package named `nextcloud-server` exists on Debian and which distros it covers; they cannot tell you whether the project team endorses it or which distros the project _itself_ tests. The `package_repositories` field answers that question.

**Relationship to Security Insights `distribution-points`.** [Security Insights](https://github.com/ossf/security-insights) `repository.release.distribution-points` uses the same concept: a list of `{uri, comment}` entries where `uri` may be a PURL (e.g., `pkg:npm/foobar`). `package_repositories` is a PURL-typed, semantically richer extension of that idea: the `purl` field plays the role of `uri`, and the additional fields (`distros`, `architectures`, `status`) carry detail that security-insights leaves to prose comments. Projects publishing both files can derive their `distribution-points` list directly from their `package_repositories` entries.

**Relationship to PGP artifact signing.** PGP signatures (f.e. required by Maven Central) verify that a specific artifact binary and its POM were not modified after signing — useful, but distinct from project endorsement of a distribution channel. Two gaps remain: the web-of-trust problem means a valid signature only proves the artifact was signed with a particular key, not that the key belongs to the claimed entity; and PGP covers only compiled artifacts and POM metadata, not scripts, config templates, or install hooks that ship in a distribution. The `package_repositories` field uses source repository control as its trust anchor, which does not fully eliminate the web-of-trust problem either — forge accounts can be hijacked — but the repository URL is more corroborated: it is already cross-referenced by package registries, SBOMs, documentation, and news articles across independent systems, making forgery significantly harder than uploading a fake key to a key server.

Catalogs should treat the four categories of distribution evidence differently:

| Source                                                       | Status                     | How to present                                         |
| ------------------------------------------------------------ | -------------------------- | ------------------------------------------------------ |
| Listed in `package_repositories`, `status: active` (default) | Project-endorsed           | Shown as official                                      |
| Listed in `package_repositories`, `status: deprecated`       | Officially retired         | Warning: project no longer maintains this distribution |
| Auto-discovered via ecosyste.ms/deps.dev, matches `url`      | Probable match, unendorsed | Shown with caveat                                      |
| Auto-discovered, does not match any `url` in catalog         | Unknown provenance         | Not linked to catalog entry                            |

This distinction matters for security: an unofficial Debian package or a lookalike npm package that is not in the `package_repositories` list can be flagged for manual review rather than silently linked to the project's trust record.

**PURL as the cross-ecosystem glue.** PURL is already the component identity standard in the SBOM ecosystem — CycloneDX and SPDX both support it, and the CRA's technical standards reference CycloneDX. By publishing PURLs in publiccode.yml, a project creates machine-readable links between:

1. Its **procurement identity** — catalog entry indexed by `url`
2. Its **supply chain identity** — SBOM component identified by PURL
3. Its **package ecosystem presence** — npm, PyPI, Debian, Homebrew, Docker Hub, and others
4. Its **distro support matrix** — which releases of which OSes the project officially supports (via optional `distros` field)

These links are also the natural foundation for linked data representations: two PURLs for the same project can be related with `owl:sameAs` or `schema:sameAs`, enabling full interoperability with the broader knowledge graph. Wikidata already carries cross-ecosystem package links for well-known projects; the `package_repositories` field makes those links explicit and crawlable for any project, without requiring a Wikidata entry.

**Auto-discovery complements the declared list.** Catalog crawlers should query [ecosyste.ms](https://packages.ecosyste.ms) (100+ registries, OS packages, funding data) and [deps.dev](https://deps.dev) (6 language ecosystems, full transitive graphs, CVE integration) using the `url` field as the lookup key. Auto-discovered packages that match a `package_repositories` entry confirm the endorsed link; those with no match are presented as unendorsed. Maintainers only need to declare what auto-discovery will miss.

**Why the field still matters even with auto-discovery.** Several distribution types are systematically under-represented in both services:

- Container images (Docker Hub, GHCR, Quay.io) — not all image registries expose structured metadata
- OS packages where the package name diverges from the repository name
- Projects too new or niche to be indexed yet
- Privately hosted packages in enterprise artifact registries
- Non-standard distro versions or custom repository URLs that require explicit endorsement

### What This Enables

1. **Official vs. unofficial distribution signal.** Procurement officers and security teams can distinguish project-endorsed packages from community-packaged or potentially malicious distributions — a distinction that is invisible to SBOM scanners without this field.
2. **SBOM-to-catalog bridging.** Given a CycloneDX or SPDX SBOM, a tool can look up each component's PURL against the catalog to retrieve procurement metadata, usage data, vendor credit information, and official distro support — connecting the security compliance world to the procurement world in a single step.
3. **Cross-ecosystem vulnerability tracing.** A CVE against `pkg:deb/debian/nextcloud-server@13.0` resolves to the publiccode.yml entry for Nextcloud, confirms which Debian releases are affected (via the `distros` list), and from there identifies all deploying organizations that declared usage of those specific versions.
4. **Richer NIS2 supply chain assessment.** Organizations with supply chain risk assessment obligations can map SBOM components to catalog entries, usage registries, credit registries, and official distro support matrices through a single PURL lookup.
5. **Catalog completeness without maintainer burden.** A minimal list of PURLs is friction-free; documenting distro versions is optional and can grow with project maturity.
6. **Distro support matrix queries.** Organizations can ask: "Which Debian releases does this project officially support?" and get a definitive answer from the project itself, rather than inferring from package metadata or forums.

---

## Full Example

Putting all improvements together:

```yaml
publiccodeYmlVersion: "1.0"

name: MedusaCMS
applicationSuite: MegaProductivitySuite
url: https://forge.example.org/owner/medusa-cms
# ===== NEW: Sanctioned mirrors and migration history =====
previousUrls:
  - https://old-forge.example.org/owner/medusa-cms
mirrors:
  - url: https://mirror.example.org/owner/medusa-cms
  - url: https://legacy-mirror.example.org/owner/medusa-cms
    status: deprecated
landingURL: https://medusa-cms.example.org
# softwareVersion and releaseDate removed — consumed from
# forge API / package registry (see Improvement 5)
logo: img/logo.svg
platforms:
  - web
  - linux

# ===== NEW: Faceted classification =====
classification:
  domain:
    - public-administration
    - healthcare
  function:
    - content-management
    - crm
    - document-management
  role:
    - standalone-web
    - platform
  layer:
    - full-stack
  audience:
    - public-administration
    - enterprise
  tags:
    - "technology:python"
    - "technology:django"
    - "compliance:gdpr"
    - "compliance:wcag-2.1"

# Deprecated, mapped from classification.function
categories:
  - content-management
  - crm
  - document-management

developmentStatus: stable

description:
  en:
    shortDescription: A CMS with integrated CRM for public sector organizations
    longDescription: |
      MedusaCMS is a content management system designed for public
      administrations that also provides integrated citizen relationship
      management (CRM) capabilities...
    features:
      - Multi-site management
      - Integrated CRM with citizen case tracking
      - WCAG 2.1 AA compliant
      - GDPR-ready data handling
    documentation: https://docs.medusa-cms.example.org

organisation:
  uri: https://ror.org/example
  name: Example Digital Agency

intendedAudience:
  countries:
    - de
    - at
    - ch

legal:
  license: EUPL-1.2
  mainCopyrightOwner: Example Digital Agency

maintenance:
  type: contract
  contractors:
    - name: Acme GmbH
      website: https://acme.example.org
      identifier: "LEI:529900T8BM49AURSDO55" # optional; see Improvement 5
      # until: "2027-12-31" — deprecated; see Improvement 5
  contacts:
    - name: Jane Maintainer
      email: jane@medusa-cms.example.org
      affiliation: Acme GmbH

localisation:
  localisationReady: true
  availableLanguages:
    - en
    - de
    - fr

# dependsOn retained for human-readable dependency names,
# but versionMin/versionMax removed (see Improvement 5).
# Version constraints belong in the SBOM.
dependsOn:
  open:
    - name: PostgreSQL

# ===== NEW: Package repository references =====
# Project-endorsed official distributions. Catalogs distinguish
# these from auto-discovered (unendorsed) packages.
package_repositories:
  # Simple: just PURL
  - purl: pkg:npm/medusa-cms
  - purl: pkg:docker/example/medusa-cms
  - purl: pkg:github/example/medusa-cms

  # Enhanced: add distro versions
  - purl: pkg:deb/debian/medusa-cms
    distros: [bookworm, trixie] # official Debian support

  # Deprecated: project no longer maintains this distribution
  - purl: pkg:npm/medusa-cms-legacy
    status: deprecated

# ===== NEW: Supply chain references =====
supplyChain:
  sbom:
    - name: Release SBOM (CycloneDX)
      predicate-uri: https://cyclonedx.org/bom
      location: https://forge.example.org/owner/medusa-cms/releases/{version}/sbom.cdx.json
      comment: Replace {version} with the release tag. Latest at releases/latest/sbom.cdx.json
  scorecard: https://api.scorecard.dev/projects/forge.example.org/owner/medusa-cms
  securityPolicy: https://medusa-cms.example.org/.well-known/security.txt
  reuse: https://api.reuse.software/status/forge.example.org/owner/medusa-cms

# ===== NEW: Accessibility declaration =====
supports:
  accessibility:
    coverage: partial
    vpat: docs/a11y/VPAT-2026-03.md
    issueQueue: https://forge.example.org/owner/medusa-cms/issues?label=accessibility
    maintainerContact: accessibility@medusa-cms.example.org
    defaults:
      standard: EN-301-549
      level: AA
      reviewerType: third-party-audit
    assertions:
      - surface: web-ui
        standard: WCAG-2.2
        level: AA
        evidence:
          - type: test-report
            reference: docs/a11y/web-audit-2026-03.md
      - surface: starter-theme
        standard: WCAG-2.2
        level: AA
        reviewerType: self-assessment
        evidence:
          - type: test-report
            reference: docs/a11y/starter-theme-audit-2026.md
    statement: ACCESSIBILITY.md
    lastReviewed: 2026-03-01
    lastReviewedVersion: "10.2.0"
    automatedTesting:
      tool: axe-core
      ciReference: .github/workflows/accessibility.yml
    implementation:
      authoringToolSupport: partial
      componentLibraryUrl: https://design.medusa-cms.example.org/accessibility
    remediationRoadmap: https://forge.example.org/owner/medusa-cms/milestone/42

# ===== NEW: Credit registry discovery =====
contributionCreditRegistries:
  - name: MedusaCMS Vendor Directory
    url: https://medusa-cms.example.org/vendors
    apiUrl: https://medusa-cms.example.org/api/v1/credits
    type: contribution-credits
  - name: ecosyste.ms
    url: https://ecosyste.ms/projects/lookup?url=https://forge.example.org/owner/medusa-cms
    apiUrl: https://packages.ecosyste.ms/api/v1/packages/lookup?repository_url=https://forge.example.org/owner/medusa-cms
    type: funding

# Deprecated — usage data comes from external registries that
# independently index projects by their url field.
# See: Registry Discovery Standard
usedBy:
  - Stadt München
  - Comune di Roma
```

---

## Contribution Credit Registry API (Rough Outline)

This is a **separate specification** from publiccode.yml. It defines the API that contribution credit registries (referenced from `contributionCreditRegistries[].apiUrl`) must implement so that crawlers can aggregate vendor/contributor data across registries.

### Design Principles

1. **Read-only public API.** The write side (how contribution credits are recorded) is registry-specific. Only the read side is standardized.
2. **Project-centric queries.** Given a project identifier (repository URL), return all credited organizations and individuals.
3. **Time-windowed.** Contribution credits are meaningful in context of recency. A company that contributed heavily 5 years ago but not since is different from one actively contributing.
4. **Inspired by Drupal's Contribution Records system.** Drupal tracks contributions per-issue, links them to users and organizations, and exposes them via [JSON:API endpoints](https://www.drupal.org/drupalorg/docs/apis/rest-and-other-apis) (`contribution-records-by-organization`, `contribution-records-by-user`). The underlying [`contribution_records` module](https://git.drupalcode.org/project/contribution_records) is open source and architecturally ticket-system-agnostic — it could be adopted by other projects or operated as a SaaS for projects that want a proven contribution credit system without building one from scratch.

### Endpoints

#### `GET /projects/{project-url}/credits`

Returns all credited entities for a project. Supports a `since` parameter for time-windowed queries.

```
GET /projects/forge.example.org%2Fowner%2Fmedusa-cms/credits?since=2025-01-01
```

The response identifies the project and registry, then lists credited entities with the following key fields per entity:

| Field                   | Purpose                                                                                                                                                                                      |
| ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `entity.type`           | `organization` or `individual`                                                                                                                                                               |
| `entity.identifier`     | Stable identifier — URL, ROR ID, Wikidata QID, LEI, etc. See [Entity Identity](#entity-identity) for recommended schemes and how verification trust should be carried through API responses. |
| `summary.totalCredits`  | All-time contribution credit count (registry-defined unit)                                                                                                                                   |
| `summary.periodCredits` | Contribution credits in the requested time window                                                                                                                                            |
| `summary.roles`         | Types of contributions: `code`, `documentation`, `security`, `translation`, `maintainer`, `funding`, `triage`, `design`, `support-contract`                                                  |
| `ranking.position`      | Rank among all credited entities for this project                                                                                                                                            |
| `ranking.tier`          | Registry-defined tier (e.g., `platinum`, `gold`, `silver` — or Drupal's `premium`, `certified`, `contributing`)                                                                              |

#### `GET /organizations/{org-identifier}/credits`

Inverse query: given an organization, return all projects they have contribution credits in. This lets procurement officers look up a vendor and see their full portfolio of contributions.

### What This Doesn't Standardize (Intentionally)

- **How contribution credits are earned.** Each project/registry defines its own contribution tracking. Drupal's system weights issue credits by project usage (more users = more credit per issue), adds credits for case studies, event sponsorships, organizational membership, and sponsored contributor roles. Other projects might simply count commits, reviews, or funding — or use GitHub Sponsors as a proxy for organizational investment. The API reports the _result_, not the methodology.
- **Attribution policies.** Registries differ in how contribution credits follow people and organizations over time. In Drupal's system, contribution credits attributed to a vendor through a contributor remain with that vendor even after the contributor changes employers — a critical property for procurement stability, since it means a vendor's track record reflects cumulative investment rather than current headcount. Other registries may re-attribute credits when contributors move. The API exposes credit totals and time windows; the attribution policy is registry-specific.
- **The trust model.** Some registries verify contributions via code review sign-off (high trust). Others use self-reporting (low trust). The `trustModel` field lets consumers assess credibility.
- **Tier definitions.** What "platinum" means varies by registry. This is like credit rating agencies — the scale is per-agency, but the API format is standard.

---

## Registry Discovery Standard (Rough Outline)

This is a **companion specification** to publiccode.yml. It solves a chicken-and-egg problem: if usage registries are decentralized and not listed in project files, how do crawlers find them?

The same standard also covers credit registries — even though projects _can_ list those in publiccode.yml, a global crawler shouldn't have to read every project file to discover that a registry exists. Registries should be independently discoverable.

### The Problem

```
                    ┌──────────────┐
                    │  Crawler     │
                    │  (EU OSS     │
                    │   Catalogue) │
                    └──────┬───────┘
                           │
              How does it find these?
                           │
           ┌───────────────┼───────────────┐
           ▼               ▼               ▼
    ┌──────────┐    ┌──────────┐    ┌──────────┐
    │nat. reg. │    │Developers│    │ Future   │
    │(DE)      │    │Italia    │    │ registry │
    │(usage)   │    │(usage)   │    │          │
    └──────────┘    └──────────┘    └──────────┘
```

Projects publish publiccode.yml. Usage registries track which organizations use which projects. But nothing connects crawlers to registries. Today this is solved by hardcoding registry URLs into each crawler — which doesn't scale.

### Design: A Registry of Registries

A lightweight manifest format that registries publish to make themselves discoverable.

#### Registry Manifest

Each registry publishes a manifest at a well-known URL:

```
https://{registry-domain}/.well-known/publiccode-registry.json
```

The manifest describes the registry's identity, operator, capabilities, API location, and scope. Key fields:

| Field                 | Purpose                                                                                                                                               |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `capabilities`        | What the registry tracks: `usage` (who uses what), `contribution-credits` (who contributes what), or both                                             |
| `trustModel`          | How the registry verifies claims: `verified-domain`, `signed-attestation`, `self-reported`                                                            |
| `api.conformsTo`      | Which standardized API specs the registry implements (see below)                                                                                      |
| `scope.jurisdictions` | Which countries/regions the registry covers (ISO 3166-1)                                                                                              |
| `scope.sectors`       | Which sectors the registry covers, using publiccode.yml's `intendedAudience.scope` vocabulary (e.g., `government`, `health`, `education`, `research`) |

#### Discovery Mechanisms

Crawlers discover registries through multiple complementary channels:

1. **Well-known URL probing.** Crawlers can probe known domains for `/.well-known/publiccode-registry.json`. Useful for discovering registries at domains already known to the ecosystem (opencode.de, developers.italia.it, etc.).

2. **Central directory (starter list).** A community-maintained list of known registry manifest URLs, similar to how browsers ship a root certificate store. Maintained via pull requests (like a browser's CA inclusion process) and published at a stable URL. Any registry operator can request inclusion.

3. **DNS TXT records (future).** A registry could publish a TXT record at `_publiccode-registry.opencode.de` pointing to its manifest URL. This enables fully decentralized discovery without any central directory.

### Usage Registry API

Registries with `"capabilities": ["usage"]` must implement these endpoints.

#### `GET /software/{project-url}/adopters`

Given a project URL (the `url` field from publiccode.yml, URL-encoded), return all organizations that have declared they use it. Each adopter entry includes the organization's identity (name, domain, jurisdiction), adoption status (`production`, `pilot`, `evaluation`, `retired`), and when they adopted.

```
GET /v1/software/forge.example.org%2Fowner%2Fmedusa-cms/adopters
```

#### `GET /organizations/{org-identifier}/software`

Inverse query: what software does this organization use? This enables the "software declaration" use case — a procurement office can publish its entire stack, and crawlers can aggregate this into the reuse badge system.

#### `GET /software` (bulk index)

Returns all projects the registry tracks, paginated. Enables crawlers to do a full sync rather than querying project-by-project.

### Organization-Level Usage Declarations

Usage registries are the canonical aggregation point for adoption data, but the data has to get into registries somehow. The most scalable intake mechanism is a standardized `.well-known` file that deploying organizations publish on their own domains (e.g., `https://muenchen.de/.well-known/publiccode-usage.json`).

#### Key Fields

| Field                         | Purpose                                                                                                                                                                                                                                                                                                                                                     |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `organization.url`            | The declaring organization's domain — also serves as its verified identity (domain control = proof of authority)                                                                                                                                                                                                                                            |
| `organization.sector`         | The organization's sector, using publiccode.yml's `intendedAudience.scope` vocabulary (e.g., `government`, `health`, `education`). Optional.                                                                                                                                                                                                                |
| `software[].url`              | The project's canonical URL (matches the `url` field in publiccode.yml)                                                                                                                                                                                                                                                                                     |
| `software[].status`           | Deployment status: `production`, `pilot`, `evaluation`, or `retired`                                                                                                                                                                                                                                                                                        |
| `software[].until`            | When the software was retired (only for `retired` status) — provides an explicit deprecation signal                                                                                                                                                                                                                                                         |
| `software[].classification`   | Which classification facets apply to this organization's specific use, using publiccode.yml's `classification` vocabulary. The `domain` and `function` facets are most meaningful here; `role`, `layer`, `technology`, and `audience` are not recommended in this context. Optional.                                                                        |
| `software[].contractors[]`    | Contracted support providers for this specific deployment — each entry carries `name`, `website`, and an optional `identifier` (same `SCHEME:VALUE` pattern as `maintenance.contractors[]` in publiccode.yml). Optional; intended for organizations with supply chain contract documentation obligations (e.g., DORA Article 28 register).                  |
| `software[].certifications[]` | Certifications and audits obtained by the deploying organization for this specific deployment — each entry carries `type` (`accessibility`, `security`, `privacy`, `quality`), `standard` (e.g., `WCAG 2.1`, `ISO 27001`), optional `level` (e.g., `AA`, `High`), `issued` date, optional `expires` date, `issuer`, and optional `reference` URL. Optional. |
| `lastUpdated`                 | When this file was last modified — crawlers flag files older than a configurable threshold (e.g., 12 months) as potentially stale                                                                                                                                                                                                                           |

The schema deliberately excludes software version numbers and infrastructure details — the declaration answers "do you use this project, and for what purpose?" not "how is it deployed?" Technology detection services like BuiltWith already expose comparable detail for public-facing web applications.

#### Classification in Usage Declarations

The `software[].classification` field reuses publiccode.yml's `classification` vocabulary but carries a different assertion authority: **publiccode.yml's `classification` is asserted by the project author** ("this software supports content management and CRM"); **the usage declaration's `classification` is asserted by the deploying organization** ("we use it specifically for content management"). Same controlled vocabulary, different perspective — consistent with the broader pattern in this ecosystem where each actor declares only what they can directly verify.

This distinction is valuable for procurement. A project listing `classification.domain: [content-management, crm]` tells potential adopters what the software _can_ do. A municipality listing `classification.domain: [content-management]` in their usage declaration tells peers what a comparable organization _actually uses it for_ — a more reliable signal for adoption decisions.

#### Deployment Certifications

The `software[].certifications[]` field records certifications the **deploying organization** has obtained for their specific deployment — distinct from any claims the project makes in `publiccode.yml`'s `supports.accessibility` or `supplyChain` fields. Each entry carries `type` (`accessibility`, `security`, `privacy`, `compliance`), `standard`, optional `level`, `issued`, optional `expires`, `issuer`, and optional `reference` URL. Common standards: EN 301 549 / WCAG (accessibility); ISO 27001, BSI C5, EUCS, Common Criteria (security); ISO 27701, EuroPriSe (privacy); NIS2 audit attestations (compliance).

```json
{
  "organization": {
    "url": "https://muenchen.de",
    "sector": "government"
  },
  "software": [
    {
      "url": "https://forge.example.org/drupal/drupal",
      "status": "production",
      "classification": {
        "domain": ["content-management"],
        "function": ["authentication", "caching"]
      },
      "certifications": [
        {
          "type": "accessibility",
          "standard": "EN 301 549",
          "level": "AA",
          "issued": "2025-03-10",
          "expires": "2026-03-10",
          "issuer": "Acme Accessibility Auditors GmbH",
          "reference": "https://muenchen.de/docs/en301549-audit-2025.pdf"
        },
        {
          "type": "security",
          "standard": "BSI C5",
          "level": "Basic",
          "issued": "2024-11-01",
          "issuer": "TÜV Rheinland",
          "reference": "https://muenchen.de/docs/bsi-c5-2024.pdf"
        },
        {
          "type": "privacy",
          "standard": "ISO 27701",
          "issued": "2024-06-15",
          "issuer": "DQS GmbH"
        }
      ]
    },
    {
      "url": "https://forge.example.org/nextcloud/server",
      "status": "production",
      "classification": {
        "domain": ["file-management", "collaboration"]
      },
      "contractors": [
        {
          "name": "Acme GmbH",
          "website": "https://acme.example.org",
          "identifier": "LEI:XXXXXXXXXXXXXXXXXXXX"
        }
      ]
    }
  ],
  "lastUpdated": "2025-09-01"
}
```

#### Design Principles

The file should be maintained as part of software deployment processes, not as a separate bureaucratic exercise. Projects like Drupal or Nextcloud can integrate the generation of this file into their documented deployment procedures — deploying adds an entry, decommissioning marks it `retired`.

For organizations with many deployments across internal and public-facing domains, a two-tier model applies: each deployment publishes its own per-deployment declaration, and an organizational aggregation tool assembles the combined public-facing file on the organization's primary domain. This minimizes staleness and allows both internal and public-facing usage to be declared.

Usage registries aggregate declaration data from multiple complementary sources: `.well-known` crawling (domain control = identity verification), direct declaration via registry UI/API, per-deployment aggregation, and bulk import from internal scanning tools.

### How Contribution Credit Registries Relate

Contribution credit registries (listed in publiccode.yml's `contributionCreditRegistries`) **can also** publish a registry manifest and appear in the central directory. This means:

- A crawler building a **vendor intelligence dashboard** can discover all contribution credit registries, then for each project, check both the project's endorsed registries (from publiccode.yml) and any additional registries found via the directory.
- The project's `contributionCreditRegistries` listing serves as an **endorsement signal** — "we consider this data authoritative" — but the registry is still independently discoverable by crawlers who want a broader view.

### Trust Models

| Model                | Verification                                                                            | Example                              |
| -------------------- | --------------------------------------------------------------------------------------- | ------------------------------------ |
| `verified-domain`    | Organization controls the declared domain (DNS validation or institutional account)     | Developers Italia, EU OSS Catalogue  |
| `signed-attestation` | Organization signs a machine-readable statement (e.g., with a code-signing certificate) | Future: EU eIDAS-backed attestations |
| `self-reported`      | No verification, organization self-declares                                             | Low-trust community registries       |

### Entity Identity

Both credit registries and usage registries track entities — vendors contributing to projects, and organizations deploying them. Without stable, shared identifiers, cross-registry aggregation breaks down: "Acme GmbH", "ACME GmbH", and "Acme" in three different registries may be the same legal entity or three different ones. There is no way to know without a canonical reference.

Three classes of entities benefit from resolvable identifiers across the ecosystem:

| Entity class                             | Where it appears                                                                           | Problem without stable IDs                                                                                                                                           |
| ---------------------------------------- | ------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Vendors / contributors / contractors** | Credit registry `entity.identifier`; publiccode.yml `maintenance.contractors[].identifier` | Contribution records cannot be aggregated across registries; inline contractor declarations cannot be linked to credit registry profiles without a stable identifier |
| **Deploying organizations**              | Usage registry adopter entries; `.well-known` declarations                                 | The same municipality appears under different names in different registries; sector classification cannot be verified                                                |
| **CRA stewards**                         | publiccode.yml `maintenance.steward.identifier`                                            | A legal entity assuming CRA steward obligations must be unambiguously identifiable by regulators and market surveillance authorities                                 |

The CRA steward already has an `identifier` field in the proposed schema. The `organisation` field (the project-owning organization) already has a `uri` field serving the same purpose. Improvement 5 adds an optional `identifier` field to `maintenance.contractors[]` following the same pattern. Contractors and credit registry vendors are the same entity class viewed through different lenses — `contractors[]` is the inline, project-asserted form; a credit registry entry with `roles: ["support-contract"]` is the registry-asserted form. The `identifier` field makes them cross-referenceable, completing the entity identity picture across the ecosystem.

The fact that LEI is the preferred identifier across stewards, credit registry entities, and deploying organizations is intentional: a single legal entity identifier scheme spans the whole ecosystem.

#### Recommended Identifier Schemes

Rather than defining a new identifier, the ecosystem composes existing stable schemes. All are optional enrichments on top of the baseline domain URL — no law requires an organization to obtain or publish any of them:

| Scheme                                    | Coverage                                                | Best for                                                              |
| ----------------------------------------- | ------------------------------------------------------- | --------------------------------------------------------------------- |
| [LEI](https://www.gleif.org/) (ISO 17442) | Global legal entities, maintained by GLEIF              | Companies, foundations; already referenced in the CRA steward section |
| [ROR](https://ror.org/)                   | Research and academic organizations                     | Universities, research institutes                                     |
| [Wikidata QID](https://www.wikidata.org/) | Broad; includes municipalities, government bodies, NGOs | Public sector organizations and any entity with a Wikidata entry      |
| Domain URL                                | Any entity with a web presence                          | Baseline; already the identity anchor in `.well-known` declarations   |

The domain URL is the lowest-bar identifier and the starting point for all deploying organizations (domain control = proof of authority). Stable identifiers enrich that baseline voluntarily: a municipality that also publishes a Wikidata QID gives crawlers enough to resolve it unambiguously across registries, but this is an improvement in data quality, not a requirement.

Where an organization does provide a stable identifier, registry APIs should include the identifier scheme alongside the value — `LEI:XXXX`, `ROR:XXXX`, `wikidata:QXXXX`, or a plain URL — so that crawlers can perform cross-registry entity resolution without guessing the scheme.

#### Organization Classification and Verification

Self-declared sector classification (via `organization.sector` in the usage declaration, or `scope.sectors` in the registry manifest) provides useful signal, but its value depends on whether it has been verified. Email domain matching against a known public sector domain list is one implementation pattern for registry-level verification. The exact mechanism is an implementation detail; what matters for the ecosystem is that registries expose _how_ a sector claim was verified, not just the claim itself.

This maps onto the existing trust model:

| Verification level   | Sector claim reliability                                               | Example                                                      |
| -------------------- | ---------------------------------------------------------------------- | ------------------------------------------------------------ |
| `self-reported`      | Low — organization asserts its own sector                              | A company declaring itself `government` without verification |
| `verified-domain`    | Medium — domain control confirmed, sector inferred from domain list    | Email domain matching against known public sector list       |
| `signed-attestation` | High — formal attestation, e.g., eIDAS-backed institutional credential | Future EU digital identity infrastructure                    |

#### Trust Chain to Catalog Filters

The trust information established at each layer — entity identity, sector verification, domain control — is only useful if it reaches the procurement decision-maker. Catalog UIs that surface this data can offer trust-aware filters; catalogs that discard it cannot. This is a capability question, not a compliance obligation: no law requires a catalog to offer these filters, but catalogs that do give procurement officers a meaningfully richer decision environment.

Concretely, the Usage Registry API's `/software/{url}/adopters` response and the Contribution Credit Registry API's `/projects/{url}/credits` response may include, per entity:

- The organization's stable identifier (and scheme), if one was provided
- The trust model used to verify their identity (`verified-domain`, `signed-attestation`, `self-reported`)
- The verification method used to confirm their sector classification, if the registry performed one

This enables catalog UIs to offer filters such as "show only government-verified deployers", "show only contributions from LEI-identified organizations", or "exclude self-reported data" — filters that are only possible if the underlying trust data flows through. Registries that expose this data produce more useful catalog results; those that don't still participate in the ecosystem at a lower trust level.

### What This Enables

1. **Any organization can start a usage registry** for its jurisdiction or sector without needing buy-in from every project.
2. **Crawlers discover registries, not projects.** The EU OSS Catalogue crawler checks the central directory, fetches all registry manifests, then queries each registry's API. No per-project configuration needed.
3. **Projects remain in control of contribution credit endorsement** via `contributionCreditRegistries` in publiccode.yml, while usage data flows independently.
4. **The Developers Italia adoption model scales globally.** Developers Italia is one registry among many. A German equivalent, a Brazilian equivalent, or a sector-specific registry (e.g., healthcare) can all join the ecosystem by publishing a manifest and conforming to the API.
5. **Deploying organizations can declare usage without intermediaries.** By publishing `/.well-known/publiccode-usage.json` on their domain, organizations assert usage with their domain as proof of identity — no registry account required. Registries crawl these files as one intake mechanism alongside direct declarations.
6. **Deployment processes become the source of truth.** When open source projects integrate usage declaration updates into their deployment documentation, the `.well-known` file stays current with actual deployments. Retirement is explicitly signaled, giving the ecosystem a deprecation mechanism that manual registries lack.

### Project-Level Funding Declarations

Following the same pattern, OSS projects can publish a standardized `.well-known` file declaring their funding sources, making that information discoverable and aggregable by crawlers without requiring centralized registry maintenance.

#### Design & Schema

Each OSS project publishes an optional funding declaration at:

```
https://{project-domain}/.well-known/publiccode-funding.json
```

This enables procurement offices and funding allocators to assess project sustainability — knowing whether a project is grant-funded, commercially sponsored, community-supported, or a mix. The format is intentionally flexible, allowing projects to declare whatever funding relationships they wish to make public.

```json
{
  "project": {
    "url": "https://forge.example.org/owner/medusa-cms",
    "name": "Medusa CMS"
  },
  "funding": [
    {
      "source": "European Commission",
      "program": "Horizon Europe / NGI Forward",
      "type": "public-grant",
      "url": "https://ngi.eu/projects/medusa-cms/",
      "period": {
        "from": "2024-01-01",
        "to": "2025-12-31"
      },
      "description": "Core maintenance and feature development"
    },
    {
      "source": "Acme Corporation",
      "type": "corporate-sponsorship",
      "url": "https://acme-corp.org/sponsorships",
      "ongoing": true,
      "description": "Sustaining member supporting full-time core developer"
    },
    {
      "source": "Open Source Collective",
      "type": "crowdfunding",
      "url": "https://opencollective.com/medusa-cms",
      "description": "Community donations processed through Open Collective"
    }
  ],
  "lastUpdated": "2025-09-15"
}
```

#### Key Fields

| Field                   | Purpose                                                                                                              | Required |
| ----------------------- | -------------------------------------------------------------------------------------------------------------------- | -------- |
| `project.url`           | The canonical project URL from publiccode.yml — enables crawlers to link funding data to project metadata            | Yes      |
| `project.name`          | Human-readable project name for convenience                                                                          | No       |
| `funding[].source`      | Name of the funding source or organization                                                                           | Yes      |
| `funding[].type`        | Category: `public-grant`, `corporate-sponsorship`, `endowment`, `crowdfunding`, `service-revenue`, `other`           | Yes      |
| `funding[].url`         | URL to the funder's own page describing the project or funding relationship                                          | No       |
| `funding[].period`      | Time-bounded grants: `from` and `to` dates (ISO 8601). Omitted for ongoing support.                                  | No       |
| `funding[].ongoing`     | Boolean; set to `true` for recurring or indefinite sponsorship                                                       | No       |
| `funding[].amount`      | Monetary amount for transparency (optional, rarely disclosed by recipients)                                          | No       |
| `funding[].description` | Human-readable purpose or scope of funding — e.g., "core maintainer salary", "security audit", "feature development" | No       |
| `lastUpdated`           | ISO 8601 date of the last update — helps crawlers detect stale declarations                                          | Yes      |

#### Design Rationale

- **Projects own their funding narrative.** Funding declarations come from the project (or its primary operator), not from an external registry, ensuring accuracy and up-to-date information.
- **Published outside source control.** Unlike `fundingjson.org` (which lives in the repository), `.well-known/publiccode-funding.json` is published on the project's domain. Funding changes (sponsorships end, grants begin) can be updated independently without repository commits, keeping frequently-changing data out of git history — consistent with the principle that publiccode.yml should "keep only what changes rarely."
- **Procurement risk assessment.** Procurement offices can evaluate whether a project has diversified funding, is dependent on a single funder, or is at risk of abandonment due to funding gaps.
- **Conflict of interest transparency.** When a government procurement office evaluates a project and also evaluates bids for implementation services, the funding declaration signals whether the bidder/vendor is also the primary funder — a structural conflict that scoring systems should flag.
- **Data-driven allocation.** Ecosystem funds (e.g., Sovereign Tech Fund, Open Source Endowment) can query funded projects to identify gaps — projects with usage but no recorded funding, or projects from specific regions/sectors that are under-resourced.
- **Voluntary disclosure.** Projects are not obligated to publish this file. Its absence does not imply zero funding; it means the project has chosen not to declare its funding publicly. Catalogs should not penalize projects for non-disclosure.
- **Temporal accuracy.** Time-bounded grants include explicit period ranges; ongoing sponsorships are marked as such. The `lastUpdated` field prevents stale claims from being attributed to current project status.

#### Discovery & Aggregation

Crawlers can discover funding declarations through:

1. **Direct .well-known probing** (opportunistic) — if a project's publiccode.yml URL is known, a crawler can check the same domain for `/.well-known/publiccode-funding.json`.
2. **Registry Discovery Standard** — a funding registry manifest can declare itself in the central registry of registries, allowing crawlers to discover and query funding aggregators that collect project funding data.
3. **Explicit pointers** (optional) — projects that want to ensure their funding declaration is found can include a `fundingUrl` field in publiccode.yml (a light-weight field pointing to the .well-known location or an aggregator URL).

#### Relationship to fundingjson.org

Both standards can describe **received funding**, but at different depths:

- **fundingjson.org** — comprehensive financial accounting (fundraising channels, payment plans, detailed income/expense/tax history by year). Best for full financial transparency and attracting donors.
- **publiccode-funding.json** — lightweight snapshot of current funding relationships with optional time-bounded periods. Best for procurement/funder assessment of project sustainability.

Projects can publish both: fundingjson.org for donors and accountability; `publiccode-funding.json` for catalog visibility and procurement risk assessment. Catalogs can ingest and surface both standards, presenting detailed financial history or streamlined funding summaries depending on the consumer's needs.

#### What This Enables

1. **Procurement sustainability checks.** Before adopting software, an office can quickly assess whether the project is funded and by whom — answering "Will this project still be maintained in 3 years?"
2. **Funding gap analysis.** A funder can query all projects in a domain (e.g., healthcare, climate), identify those with significant adoption but minimal funding, and target grants to high-impact but under-resourced projects.
3. **Vendor transparency.** When a vendor bids on a project implementation, their role as a primary funder (if disclosed in the funding declaration) becomes visible to procurement officers — enabling proper conflict-of-interest assessment.
4. **Ecosystem health dashboards.** Catalogs and policy dashboards can display funding concentration (e.g., "What % of our critical digital infrastructure is dependent on a single funder?"), identifying systemic risks.
5. **Evidence for policy mandates.** The [EMBAG](https://www.fedlex.admin.ch/eli/cc/2023/682/en) and related mandates encourage public investment in open source. Funding declarations provide auditable evidence that public money is reaching intended projects.

---

## Assessment Registry API _(Proposed)_

**Status**: Identified as gap for operationalizing Sovereignty Checks and regional compliance assessments globally; proposed for OpenSSF Supply Chain Integrity Working Group governance.

**Governance recommendation:** This specification aligns with the OpenSSF Supply Chain Integrity WG's mission ("help individuals and organizations assess and improve the security of end-to-end supply chains"). The WG already maintains GUAC (supply chain analysis), SLSA (provenance), and gittuf (integrity)—all complementary to assessment registry discovery and aggregation. Moving this to OpenSSF would benefit from their existing infrastructure, community, and alignment with GUAC/SLSA tooling.

**Scope:** This is a **separate specification** from publiccode.yml. It defines the API that assessment registries (regional Sovereignty Checks, compliance assessments, etc.) publish so that catalogs can aggregate assessment findings alongside publiccode.yml metadata.

### Design Rationale

Regional authorities conducting Sovereignty Checks (ZenDiS Sovereignty Check in Germany, DPI validation frameworks in India, data residency assessments in Japan, etc.) need a standardized way to publish their findings so that:

1. Projects discover assessments relevant to their jurisdiction
2. Procurement offices see all applicable assessments in one place
3. Assessment results are decentralized—each region operates its own registry without centralized gatekeeping
4. Catalogs aggregate assessment data without custom integration per assessment framework

### Schema

Assessment registries publish standardized assessment results:

```json
{
  "assessor": "zendis",
  "assessorURL": "https://zendis.de",
  "assessorJurisdiction": "de",
  "projectURL": "https://github.com/example/project",
  "framework": "ZenDiS Sovereignty Check v1",
  "frameworkURL": "https://zendis.de/sovereignty-check-criteria",
  "timestamp": "2026-05-21",
  "validUntil": "2027-05-21",
  "criteria": {
    "supportsOnPremiseDeployment": {
      "status": "PASS",
      "finding": "Can be deployed on any Linux server"
    },
    "platformLockIn": {
      "status": "DISPUTE",
      "projectClaim": "No vendor lock-in; can run on any cloud",
      "assessmentFinding": "Project requires AWS RDS Aurora features not available in standard PostgreSQL",
      "evidence": "main.tf lines 245-250 show hardcoded RDS Aurora parameters",
      "recommendation": "Update publiccode.yml requiredPlatforms field to declare AWS RDS dependency",
      "confidence": 0.95
    },
    "dataResidency": {
      "status": "PASS",
      "finding": "Supports on-premise deployment with all data stored locally"
    },
    "auditability": {
      "status": "PASS",
      "finding": "Source code is public; no obfuscation"
    }
  }
}
```

### Key Features

**1. Dispute Resolution**: Assessment results can indicate `DISPUTE` status when assessment findings conflict with project claims in publiccode.yml. This maintains the separation of concerns—the project's claim remains in publiccode.yml, but the assessment registry flags the disagreement with evidence.

**2. Trust Levels**: Assessments include a trust model (`verified-domain`, `signed-attestation`, `self-reported`) so catalogs can weight findings based on verification rigor:

- **`verified-domain`:** Assessor domain control verified via DNS. Organization sector verified via domain-suffix matching against known public sector list (.gov, .edu, .ac.uk). **MEDIUM trust.**
- **`signed-attestation`:** Organization signs assessment result with a key in a public trust store (eIDAS, institutional CA, etc.). **HIGH trust.**
- **`self-reported`:** No verification. **LOW trust.**
- **Catalog behavior:** Display trust breakdown to procurement officers (e.g., "2/5 assessors rate FAIL: 1 verified-domain + 4 self-reported"). Allow filtering by minimum trust threshold.

**3. Confidence Scoring**: Assessments can include confidence levels (0-1) so catalogs can highlight high-confidence findings vs. preliminary assessments.

**4. Recommendation Loop**: Assessment findings can include recommendations (e.g., "Project should update publiccode.yml to declare AWS RDS dependency"), creating a feedback loop for project improvement.

**5. Status Vocabulary**: Standardized status values:

- `PASS` — Project meets the assessment criterion
- `FAIL` — Project does not meet the criterion
- `DISPUTE` — Assessment finding contradicts project claim; requires resolution
- `INCONCLUSIVE` — Assessor cannot determine status with current evidence
- `NOT_APPLICABLE` — Criterion does not apply to this project

### API Endpoints

#### `GET /assessments/{project-url}`

Returns all assessments published for a given project URL.

```json
[
  {
    "assessor": "zendis",
    "assessorJurisdiction": "de",
    "timestamp": "2026-05-21",
    "criteria": { ... }
  },
  {
    "assessor": "india-dpi-validation",
    "assessorJurisdiction": "in",
    "timestamp": "2026-04-15",
    "criteria": { ... }
  }
]
```

#### `GET /assessors`

Returns metadata about all assessment authorities publishing to this registry.

```json
[
  {
    "assessor": "zendis",
    "name": "Zentrum für Digitale Souveränität der Öffentlichen Verwaltung",
    "country": "de",
    "framework": "ZenDiS Sovereignty Check",
    "frameworkURL": "https://zendis.de/sovereignty-criteria",
    "criteria": ["supportsOnPremiseDeployment", "platformLockIn", "dataResidency", ...]
  }
]
```

#### `GET /frameworks`

Returns all assessment frameworks known to catalogs for filtering and discovery.

```json
[
  {
    "id": "zendis-sovereignty-check-v1",
    "name": "ZenDiS Sovereignty Check",
    "description": "German assessment for digital sovereignty in public administration",
    "country": "de",
    "criteria": [ "supportsOnPremiseDeployment", "platformLockIn", "dataResidency", ... ]
  },
  {
    "id": "india-dpi-validation-v1",
    "name": "India Digital Public Infrastructure Validation",
    "description": "Assessment for suitability in Indian DPI deployments",
    "country": "in",
    "criteria": [ "openStandards", "interoperability", "scalability", ... ]
  }
]
```

### What This Enables

1. **Decentralized Sovereignty Checks**: Each region (Germany, Japan, India, etc.) operates its own Assessment Registry publishing to a standard API. Catalogs aggregate findings without fragmentation.
2. **Procurement Filtering**: Procurement offices filter "Show me projects that PASS the India DPI validation" or "Which projects have DISPUTES in the ZenDiS assessment?"
3. **Transparent Disagreement**: Projects and assessors can address disputes through the registry rather than silently conflicting.
4. **Trust Building**: Over time, catalogs track which assessment frameworks are reliable, which projects have high dispute rates, and which assessors have high confidence scores.
5. **Cross-Regional Learning**: Procurement offices across jurisdictions see which assessment criteria matter, enabling harmonization or informed divergence based on local policy.

### Design Questions for Community

1. Should Assessment Registries be centralized (one global registry of all assessments) or federated (each assessor publishes independently and catalogs aggregate)?
2. How do catalogs discover Assessment Registry URLs? Via Registry Discovery Standard manifest, or separate mechanism?
3. Should projects be able to embed assessment results in publiccode.yml (like `supplyChain` links to SBOMs), or should catalogs discover them only via Assessment Registry queries?
4. How should disputes be resolved? Should Assessment Registries support comment/response mechanisms, or should projects respond separately in their own publiccode.yml?
5. What operational metrics should Assessment Registries track to enable audit trails and feed back into trust model weighting (e.g., historical accuracy of assessor findings)?

---

## Improvement 7: Sanctioned Mirror Declarations

The `url` field is publiccode.yml's canonical project identity anchor — the key used by catalogs, usage registries, credit registries, and SBOM resolvers to identify a project unambiguously. A single forge URL is however brittle as a long-term identifier: projects may maintain official mirrors on multiple forges in preparation for a migration, or use internal infrastructure for development while publishing to a public-facing forge for end-user access.

Without a machine-readable way to declare official mirrors, catalogs cannot distinguish a project's own sanctioned mirrors from unofficial forks, and a forge URL change silently breaks all existing references — usage declarations, news article entity links, SBOM-to-catalog lookups — until each consuming system is manually updated.

**Proposal:** Add an optional `mirrors` field listing officially sanctioned alternative repository locations for the same project.

### Schema

```yaml
# Canonical identity anchor — used as the primary key across all
# ecosystem components (usage registries, contribution credit registries, SBOMs).
url: https://forge.example.org/owner/project

# Previous canonical URLs, in chronological order (oldest first).
# Populated when the project migrates forges. Allows catalogs to
# merge stale references using the old URL with the current entry,
# and lets usage declarations written against the old URL remain valid.
previousUrls:
  - https://old-forge.example.org/owner/project

# Project-endorsed alternative repository locations.
# Each entry is an object with a required url and an optional status.
# status: active (default) | deprecated
# Catalogs treat active mirror URLs as equivalent to the canonical url
# for identity resolution. Deprecated mirrors are retained so catalogs
# can warn consumers still referencing the old location.
mirrors:
  - url: https://mirror1.example.org/owner/project
  - url: https://mirror2.example.org/owner/project
  - url: https://decommissioned.example.org/owner/project
    status: deprecated
```

### Design Rationale

**Authority via source control.** The mirrors list and `previousUrls` are committed to the repository alongside the rest of publiccode.yml — they carry the same authorial trust. An unofficial fork cannot add itself to a project's mirrors list; only the maintainers who control the canonical repository can endorse alternative locations or declare migration history.

**Lifecycle coverage.** Three distinct scenarios require explicit signalling rather than silent deletion:

- _Mirror decommissioned._ Mark the entry `status: deprecated` rather than removing it. Removing it silently breaks any consumer that cached the URL; a deprecated entry lets catalogs surface a warning to those consumers instead.
- _Forge migration._ Add the destination as a mirror before migrating, then after completing the move update `url` to the new location, move the old URL to `previousUrls`, and remove it from `mirrors`. The `previousUrls` chain means catalogs can merge stale references from usage declarations, news article entity links, and SBOM resolvers written against the old URL — without requiring every downstream system to be updated manually.
- _Old forge goes dark before migration is complete._ If the canonical repository becomes inaccessible, the new publiccode.yml at the new location should carry `previousUrls` so crawlers discovering the new entry can identify and retire the orphaned old catalog entry rather than leaving both live as duplicates.

**Forge migration readiness.** The recommended migration sequence is: (1) add destination as a mirror, (2) complete the migration, (3) update `url`, move old URL to `previousUrls`, clear mirrors. Usage declarations and other downstream references to the old URL remain resolvable throughout, since the catalog normalises all known aliases to the current canonical `url`.

**Development vs. public access.** Projects using internal forge infrastructure for development but publishing to a public forge for end-user access can declare the public-facing URL as canonical while listing the internal location in mirrors — without ambiguity about which URL external systems should use.

**Entity anchor for news article linking.** News outlets embedding Schema.org `mentions` with a `sameAs` value can reference any of a project's sanctioned URLs — canonical, mirror, or previous — and a catalog lookup resolves them to the same entry. This avoids the Wikidata chicken-and-egg problem: any project with a publiccode.yml has a resolvable identity anchor from day one. Wikidata QIDs, where they exist, are aggregated as additional aliases rather than acting as a prerequisite for visibility.

**Duplicate detection.** Catalogs that encounter publiccode.yml files at multiple URLs should check `url`, `mirrors`, and `previousUrls` before flagging a duplicate. If a known mirror or previous URL appears as the `url` in a separately crawled file, the catalog can surface a conflict notice to the maintainer rather than silently creating a duplicate entry.

---

## Improvement 8: CRA Steward Declaration _(Deferred)_

> **Status: deferred pending CRA guidance.** The Cyber Resilience Act's open-source software steward obligations are still being clarified through guidance documents (see risk [P5](RISK_ANALYSIS.md)). This improvement is described here as a planned extension so that the design space is reserved and the intent is clear, but it should not be implemented until the regulatory semantics have settled.

The [Cyber Resilience Act](https://digital-strategy.ec.europa.eu/en/policies/cra-open-source) introduces the _open-source software steward_ — a legal entity that provides sustained support to an OSS product intended for commercial use and assumes formal obligations for cybersecurity policy, vulnerability handling, and incident reporting. The existing `maintenance.contacts` and `maintenance.contractors` fields describe operational support arrangements but do not identify a legal entity accepting CRA steward responsibilities. This improvement adds a dedicated `steward` field under `maintenance` to fill that gap.

### Design Rationale

The field follows the pointer pattern established by `supplyChain`: it identifies and links to an external declaration rather than asserting a legal claim inline. A YAML file in a git repository is not an appropriate vehicle for a binding legal declaration — it can be wrong, stale, or set by someone who is not the steward. The `url` field should point to the steward entity's own published declaration (on their website, a regulatory register, or equivalent), with the publiccode.yml entry serving as a discovery mechanism.

### Schema

```yaml
maintenance:
  # Existing fields (contacts, contractors) unchanged.

  # CRA open-source software steward — the legal entity that has
  # assumed steward responsibilities under the Cyber Resilience Act.
  # Points to an authoritative steward declaration rather than
  # asserting the legal claim inline.
  # DEFERRED: only add once CRA guidance has settled on required
  # identifiers and declaration formats.
  steward:
    # Legal name of the steward entity.
    name: Acme Foundation
    # URL to the steward's own published CRA steward declaration
    # or organizational page confirming steward responsibilities.
    url: https://acme-foundation.org/cra-steward
    # Stable legal identifier for the entity.
    # Preferred: LEI (Legal Entity Identifier, ISO 17442).
    # Alternatives: EU VAT number, national business register ID.
    identifier: LEI:XXXXXXXXXXXXXXXXXXXX
    # URL to the steward's documented cybersecurity policy per
    # CRA Article 24(1): the organizational policy covering secure
    # development practices, vulnerability handling, and community
    # disclosure processes. Distinct from supplyChain.securityPolicy,
    # which is the project-level vulnerability disclosure policy.
    # DEFERRED: field semantics depend on regulatory guidance on what
    # constitutes a "verifiable" cybersecurity policy document.
    cybersecurityPolicy: https://acme-foundation.org/cra-cybersecurity-policy
```

### Relationship to Existing Fields

The `maintenance.contractors` field already lists organizations providing contracted support. A CRA steward will often be one of those contractors, or the project's own foundation. The `steward` field is not a replacement — it makes the specific legal accountability claim explicit and machine-readable, separate from the operational listing. A project may have multiple contractors but only one (or a small number of) formally registered stewards.

The `cybersecurityPolicy` field is distinct from `supplyChain.securityPolicy`: the latter is a project-level vulnerability disclosure policy ("how to report a vulnerability in this software"), while `cybersecurityPolicy` is the steward's organizational policy document — broader in scope (covering secure development practices, internal processes, community coordination) and owned by the steward entity rather than the project. A project without a formal steward still benefits from `supplyChain.securityPolicy`; `cybersecurityPolicy` only applies when a steward exists.

### What This Enables

1. **Procurement officers** can identify which legal entity is accountable for CRA compliance — not inferred from contributor lists, but explicitly declared.
2. **Regulators and market surveillance authorities** can discover both the steward identity and their cybersecurity policy document from a single metadata file, directly supporting Article 24(2)'s requirement that stewards provide documentation on request.
3. **Crawlers** can surface steward identity, cybersecurity policy, and `supplyChain` artifacts together, giving procurement offices a complete CRA compliance picture in one place.

---

## Improvement 9: Platform Lock-in Declaration _(Proposed)_

**Status**: Identified as gap for Sovereignty Checks and procurement evaluation; needs community input on design.

Projects should be able to declare which platforms they require and assess vendor lock-in risk. This enables Sovereignty Checks (like ZenDiS Sovereignty Check) to evaluate whether a project can be deployed independently or has hard dependencies on proprietary platforms.

### Design Rationale

While Software Bill of Materials (SBOM) documents code-level dependencies, it cannot capture architectural platform requirements. A project may declare that it uses only open-source libraries, but still require deployment on AWS RDS with RDS-specific extensions, or require Azure Cosmos DB proprietary features. This architectural lock-in is invisible to procurement offices and Sovereignty Checks unless explicitly declared.

### Schema

```yaml
requiredPlatforms:
  - name: "PostgreSQL"
    vendor: "Any (open source compatible)"
    lockIn: false
  - name: "AWS RDS with Aurora-specific features"
    vendor: "AWS"
    lockIn: true
    comment: "Requires RDS Aurora extensions not available in standard PostgreSQL"
    featuresMissingInOpenSource:
      ["Auto-scaling groups", "Multi-AZ automatic failover"]
  - name: "On-premise Linux"
    vendor: "Any"
    lockIn: false
    optional: true
    comment: "Can be deployed on any Linux distribution; not required"
```

### What This Enables

1. **Sovereignty assessment**: Sovereignty Checks can programmatically identify whether a project has hard dependencies on proprietary platforms, and which features cause that dependency.
2. **Procurement filtering**: Procurement offices can filter "Show me projects that don't require AWS-specific features" or "Which projects can run entirely on-premise?"
3. **Alternative evaluation**: Vendors bidding on public sector work can quickly identify which open-source projects are genuinely deployable in their jurisdiction's infrastructure constraints.
4. **Vendor transparency**: When a vendor recommends a tool, the explicit platform lock-in declaration prevents hidden architectural dependencies from emerging after procurement.

### Design Questions for Community

1. Should this field live in `supplyChain` (where platform vendor information semantically belongs) or as a separate `platformDependencies` section at the top level?
2. How specific should "vendor" names be? (e.g., "AWS", "Amazon Web Services", "AWS RDS", "Amazon Aurora")
3. Should there be a controlled vocabulary of common platform vendors and lock-in features, or is free-text acceptable?
