# Proposal: An Integrated Ecosystem for Decentralized Open Source Registry Infrastructure

A concrete proposal for evolving publiccode.yml with five backward-compatible improvements, plus four companion standards (APIs, registries, discovery mechanisms) and supporting policy frameworks to address the gaps identified in the [evaluation](RESEARCH.md). The improvements are backward-compatible — existing v0.x files remain valid — while the companion standards enable decentralized registries, standardized data formats, and verifiable trust networks for procurement and adoption decisions.

## Contents

- [Design Principles](#design-principles)
- [Policy Context: Public Procurement and Open Source](#policy-context-public-procurement-and-open-source)
- [Actors and Relationships](#actors-and-relationships)
- [Improvement 1: Faceted Classification](#improvement-1-faceted-classification-replacing-flat-categories)
- [Improvement 2: Supply Chain References](#improvement-2-supply-chain-references)
- [Improvement 3: Vendor Credit System Discovery](#improvement-3-vendor-credit-system-discovery)
- [Improvement 4: Deprecate `usedBy`](#improvement-4-deprecate-usedby)
- [Improvement 5: Deprecate Temporal Fields](#improvement-5-deprecate-temporal-fields)
- [Full Example](#full-example)
- **Companion specifications:**
  - [Credit Registry API](#credit-registry-api-rough-outline)
  - [Registry Discovery Standard](#registry-discovery-standard-rough-outline) (includes [Usage Registry API](#usage-registry-api) and [Organization-Level Usage Declarations](#organization-level-usage-declarations))
- **Deferred improvements (pending regulatory guidance):**
  - [Improvement 6: CRA Steward Declaration](#improvement-6-cra-steward-declaration-deferred)

---

## Design Principles

1. **publiccode.yml is the anchor point.** It lives in the repository and is maintained by the project team. It describes the project itself and points to external systems where appropriate; it doesn't duplicate dynamic data.

2. **Keep only what changes rarely.** Store information that changes with every release (version numbers, release dates) elsewhere—in GitHub releases, package registries, and Software Bill of Materials (SBOMs). Keep only data that humans maintain and that doesn't age between releases: classification, contacts, legal information, and pointers to external registries.

3. **Only include data the project controls.** A project can endorse which credit registries track its contributors (the project knows who contributes). A project cannot endorse usage registries (the project doesn't control who uses it). Usage data belongs entirely outside the file.

4. **Decentralized discovery: registries find projects, not vice versa.** Usage registries discover projects by reading their publiccode.yml `url` field. The Registry Discovery Standard (described below) makes registries themselves discoverable and crawlable.

5. **All external systems use standardized APIs.** This allows catalogs (openCode.de, EU OSS Catalogue, Developers Italia) to aggregate data from any registry that conforms to the standard, rather than building custom integrations.

6. **Classification uses multiple dimensions.** Replace flat category lists with faceted classification (domain, function, role, layer, audience). This enables richer discovery: "show me healthcare CRMs that run as standalone web applications."

7. **Companion standards inherit publiccode.yml's vocabulary.** All companion specifications — usage declarations, registry APIs, discovery manifests — reuse publiccode.yml's existing field names and controlled vocabularies wherever the concepts overlap. When a companion standard addresses the same concept from a different perspective (for example, a deploying organization declaring its use case rather than a project author declaring capabilities), we reuse the same field name with an explicit note about who is making the assertion. This avoids creating new names for the same concept. publiccode.yml is the established standard; everything else is built on top of it.

8. **No new legal obligations.** This proposal provides infrastructure to make existing legal obligations — under the CRA, NIS2, EMBAG, and similar frameworks — easier to demonstrate and verify. It does not add new requirements beyond what those laws already impose. Every field and mechanism described here is either optional or a technical way to meet obligations that already exist in law. Participation is voluntary at every level; higher trust levels unlock richer catalog filters but carry no legal requirements.

9. **Future: linked data representation (deferred).** The linked-data ecosystem (CodeMeta, schema.org, Software Heritage) could work with publiccode.yml through [YAML-LD](https://www.w3.org/community/reports/json-ld/CG-FINAL-yaml-ld-20231206/). Crawlers could create linked data from plain YAML by applying a standard context. However, this feature is deferred to reduce complexity.

---

## Policy Context: Public Procurement and Open Source

The improvements proposed here are not merely technical changes to a metadata standard — they are infrastructure for implementing the **"Public Money, Public Code"** principle at scale. The [FSFE's Public Money? Public Code! campaign](https://publiccode.eu/) demands that publicly financed software be released under free and open source licenses. Over 200 civil society organizations and more than 31,000 individuals have signed their open letter. But the principle creates a follow-on problem: once governments commit to open source, how do procurement offices actually **find, evaluate, and responsibly adopt** it?

This proposal answers that question. Each improvement directly serves a procurement need that existing standards leave unmet.

### How the Improvements Serve Procurement

| Procurement need                | Improvement                                                                                                                  | What it enables                                                                                                     |
| ------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| **Finding software that fits**  | [Improvement 1: Faceted Classification](#improvement-1-faceted-classification-replacing-flat-categories)                     | Multi-dimensional discovery: "CRM for healthcare, web-based" instead of keyword guessing                            |
| **Assessing security posture**  | [Improvement 2: Supply Chain References](#improvement-2-supply-chain-references)                                             | One-click access to SBOMs, OpenSSF Scorecards, REUSE compliance, vulnerability disclosure policies                  |
| **Evaluating vendor expertise** | [Improvement 3: Vendor Credit System Discovery](#improvement-3-vendor-credit-system-discovery)                               | Verifiable contribution data — who actually maintains the software, not just who claims to                          |
| **Checking peer adoption**      | [Improvement 4](#improvement-4-deprecate-usedby) + [Registry Discovery Standard](#registry-discovery-standard-rough-outline) | Which peer organizations already deploy this software, verified by domain control                                   |
| **Trusting the metadata**       | [Improvement 5: Deprecate Temporal Fields](#improvement-5-deprecate-temporal-fields)                                         | Only slow-changing, human-authored data — eliminating the stale version numbers that erode trust in the entire file |

### Alignment with Procurement Criteria Frameworks

The [OSBA's selection criteria for sustainable procurement of open source software](https://osb-alliance.de/publikationen/veroeffentlichungen/selection-criteria-for-the-sustainable-procurement-of-open-source-software) proposes four criteria that procurement offices should evaluate. The improvements provide the data infrastructure to make each criterion machine-verifiable:

1. **Relationship with software community.** The `creditRegistries` field (Improvement 3) provides verifiable evidence of a vendor's upstream contributions — not self-reported claims, but project-endorsed credit data.
2. **Upstream publication of modifications.** Credit registries track contribution types (code, security, documentation). Procurement offices can verify that a vendor pushes patches upstream rather than maintaining proprietary forks.
3. **High-quality Level 3 support.** The `maintenance` section (already in publiccode.yml) combined with credit data shows whether a vendor employs core developers or has contractual relationships with maintainers.
4. **Supply chain security.** The `supplyChain` section (Improvement 2) provides direct links to SBOMs, security policies, and compliance status — exactly what the Cyber Resilience Act and SBOM documentation requirements demand.

### Digital Commons & Digital Sovereignty: Government as Steward

This data infrastructure is not just about procurement efficiency—it is critical for government engagement as a steward of the digital commons and builder of digital sovereignty.

Governments creating open source policies and defending digital independence need visibility into three dimensions that this proposal addresses:

1. **Evidence of policy enforcement.** "Public Money, Public Code" mandates are aspirational without auditable evidence. Usage and credit registries turn policy into practice by making reuse and upstream contribution visible.

2. **Supply chain health and independence.** Resilient digital sovereignty requires knowing which software is widely deployed, which has fragile vendor ecosystems, and which critical infrastructure lacks sufficient upstream support. The metadata infrastructure proposed here makes this visible.

3. **Evidence-based investment.** Funding allocation can shift from anecdote-driven ("project X is important") to data-driven ("X is deployed by 47 public agencies"). Usage registries and credit registries enable this transparency.

This is a distinct role from the CRA stewards discussed in the Legislative Models section below. While the Cyber Resilience Act assigns stewardship responsibilities to _commercial entities_ maintaining individual OSS projects, governments stewarding shared infrastructure need ecosystem-wide visibility—not just individual project compliance, but cross-cutting health, adoption patterns, and sustainability.

### Legislative Models

Government OSS preference and procurement policies have been documented across 80+ countries for over 15 years [(CSIS, 2010)](https://www.csis.org/analysis/government-open-source-policies); the EU legislative wave of the 2020s is the most binding expression of a longstanding global policy direction. Several legislative initiatives create immediate demand for the infrastructure proposed here:

**Mandate-driven policy:** Switzerland's [EMBAG law](https://www.fedlex.admin.ch/eli/cc/2023/682/en) (2023) mandates open source release of publicly financed software. The [APELL initiative](https://apell.info/) and [EuroStack coalition](https://eurostack.eu/) advance similar EU-level mandates. **The problem:** these mandates are unenforceable without metadata infrastructure. Procurement offices cannot operationalize "prefer open source" without discovery and evaluation tools. **The infrastructure this proposal provides:** faceted classification and supply chain references make the mandate actionable.

**Compliance-driven obligation: Cyber Resilience Act (CRA).** The [CRA](https://digital-strategy.ec.europa.eu/en/policies/cra-open-source) applies to FOSS placed on the market for commercial use. It introduces the _open-source software steward_ — a legal entity that:

- Implements cybersecurity policies for secure development
- Operates a vulnerability handling and disclosure process
- Reports actively exploited vulnerabilities to authorities

A steward must demonstrate these commitments when requested by regulators. **The infrastructure this proposal provides:** the `supplyChain` fields (`sbom`, `securityPolicy`, `scorecard`) are the natural machine-discoverable evidence layer for these obligations. A vendor with a sustained, project-endorsed contribution record in a credit registry provides empirical evidence of "providing sustained support." This is discussed further in [Improvement 6: CRA Steward Declaration](#improvement-6-cra-steward-declaration-deferred).

**Supply chain risk mandate: NIS2 Directive.** The [NIS2 Directive](https://digital-strategy.ec.europa.eu/en/policies/nis2-directive) requires organizations in 18 critical sectors — energy, health, transport, public administration, and others — to implement supply chain security risk management. For these organizations, **the obligation is clear:** assess the supply chain security of software you adopt. **The problem:** without standardized metadata, assessment is expensive or impossible. **The infrastructure this proposal provides:** the `supplyChain` references allow a deploying organization to fetch SBOM, security policy, and maintenance evidence from a single entry point. The usage declaration mechanism (`.well-known/publiccode-usage.json`) also benefits internal compliance: organizations maintaining a declared software inventory have a natural audit trail for their supply chain risk management obligations.

### Sustainable Business Model Alignment

Open source infrastructure faces a critical funding problem—both _visibility_ of infrastructure costs and _disconnection_ of funding from usage. [Dries Buytaert's recent analysis (March 2026)](https://dri.es/open-source-infrastructure-deserves-a-business-model) shows the result: PyPI depends on donor sponsorships from Fastly, AWS, and Google Cloud; npm was acquired by Microsoft; WordPress infrastructure depends on Automattic's continued success; GNOME was forced to outsource bandwidth costs to GitHub to reduce spending.

**The core insight: Most open source infrastructure does not have a real business model. It survives through donations, corporate sponsorship, and community fundraising, rather than revenue tied to the value it delivers.**

This proposal's metadata infrastructure directly enables a sustainable alternative: **a value exchange model where infrastructure funding is connected to usage and organizational benefit**, not dependent on donor goodwill.

#### Metadata as the Foundation for Value Exchange

Dries proposes: "Keep core infrastructure free for individuals and small projects, while organizations using it at scale help pay for what they consume." This is not possible without visibility into _who_ uses what, _how much_ they rely on it, and _what value_ they extract.

The proposed infrastructure provides this foundation:

- **Usage registries** make consumption visible: which organizations deploy what software, at what scale. Infrastructure operators and funders can see which projects have broad organizational adoption and justify investment proportionally.
- **Credit registries** tie vendor benefit to visible contribution. Organizations that extract value through commercial offerings (consulting, managed hosting, SaaS) demonstrate that value through auditable upstream investment—creating a traceable chain from organizational benefit to ecosystem stewardship.
- **Supply chain references** (SBOMs, security policies, Scorecard) reduce the cost of compliance verification for large organizations. This compliance value can be monetized through tiered services: free metadata for individuals, auditable compliance attestation services for organizations above a threshold.
- **Faceted classification** enables usage-based metrics: "how many organizations deploy this healthcare software versus that one?" Data-driven funding allocation replaces guesswork.

#### Enabling Sustainable Funding Models

Once this infrastructure exists, sustainable funding models become possible:

1. **Infrastructure operators can charge tiered fees** linked to organizational consumption (downloads, API calls, registrations) — without centralizing power, because the standard APIs allow multiple registries to operate in parallel.
2. **Vendors can justify investment** in maintaining infrastructure by pointing to verified adoption and contribution data visible through credit registries — creating a clear ROI case to their leadership.
3. **Funders can allocate resources** based on usage data rather than anecdotes — improving effectiveness and accountability.
4. **Legislation can mandate infrastructure funding** by tying it to publicly visible metrics (e.g., "organizations deploying this software above this scale contribute to maintenance") rather than relying on moral persuasion.

Without standardized, decentralized metadata infrastructure, the funding problem persists: costs remain invisible, adoption remains anecdotal, and every project must reinvent its own fragile sponsorship arrangement.

With it, sustainable business models become possible—and open source infrastructure can transition from a patchwork of donations to a genuine ecosystem.

---

## Actors and Relationships

The ecosystem this proposal addresses brings together different actors, each with distinct authority and information needs. Here's who needs what, and why:

| Actor                          | Role                                                                                                                                   | Example                                                  |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------- |
| **Open Source Project**        | Publishes code and metadata. Decides project classification and which contributions it recognizes.                                     | Drupal, Nextcloud, OpenDesk                              |
| **Maintainer**                 | Day-to-day steward who authors and commits the publiccode.yml file. May work for a vendor, agency, or as an independent contributor.   | An individual or core team                               |
| **Vendor**                     | Contributes code to projects and sells related services. Wants to be findable to procurement professionals evaluating their expertise. | Consulting firms specializing in specific platforms      |
| **Procurement Office**         | Searches for suitable software, evaluates vendor expertise and security, makes purchasing decisions.                                   | Municipal IT departments, federal agencies               |
| **Deploying Organization**     | Actually runs the software in production. Wants to declare what it uses.                                                               | Cities, universities, government agencies                |
| **Federal Authority / Funder** | Allocates money, influences policy, identifies ecosystem gaps.                                                                         | Sovereign Tech Fund, CISA, digital sovereignty offices   |
| **Policy Maker / Legislator**  | Writes laws and regulations that mandate or encourage open source. Needs metadata to make compliance verifiable.                       | National parliaments, EU Commission                      |
| **Credit Registry**            | Tracks contributions to projects and creates vendor reputation data. Endorsed by projects.                                             | Drupal.org Marketplace, ecosyste.ms funding platforms    |
| **Usage Registry**             | Tracks which organizations deploy which software. Independent from projects.                                                           | openCode.de, Developers Italia catalog                   |
| **Software Catalog / Crawler** | Aggregates metadata into searchable indexes for procurement and policy.                                                                | EU Open Source Catalogue, openCode.de, Developers Italia |

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
 │    - Which credit registries the project endorses           │
 └──────────────────────────┬──────────────────────────────────┘
                            │
                            │ creditRegistries
                            ▼
              ┌────────────────────────┐        ┌────────────────────────────┐
              │   Credit Registries    │        │    Usage Registries        │
              │                        │        │                            │
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
                           │   - Credit data              │
                           │   - Usage/adoption data      │
                           │                              │
                           │  Serves:                     │
                           │   - Procurement officers     │
                           │   - Federal authorities      │
                           │   - Funders                  │
                           └──────────────────────────────┘
```

### Why Credit and Usage Are Architecturally Different

A critical distinction: **who has authority over the data**.

|                                  | Credit registries                                                            | Usage registries                                                        |
| -------------------------------- | ---------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| **What's tracked**               | Who contributes to projects and how much                                     | Which organizations deploy projects                                     |
| **Who knows this?**              | The project maintainers                                                      | The deploying organizations                                             |
| **Listed in publiccode.yml?**    | Yes — as endorsements                                                        | No — the project doesn't control this data                              |
| **How crawlers find registries** | From publiccode.yml `creditRegistries` field and Registry Discovery Standard | From Registry Discovery Standard only                                   |
| **Data entry mechanism**         | Central registry maintains the data                                          | Direct declarations from deploying organizations and automated crawling |
| **Example**                      | Drupal.org credits showing which vendors employ core contributors            | openCode.de showing which German towns use Nextcloud                    |

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
2. **Crawler-assisted enrichment.** Catalog crawlers (openCode.de, the EU OSS Catalogue) can derive classification from existing metadata — README keywords, package registry tags, GitHub topics — and store it at the catalog layer without requiring the maintainer to maintain it in the file. This keeps the file minimal while making faceted search available to catalog users.

Both approaches respect the principle that maintainers should only be asked to author data that genuinely requires their judgment.

---

## Improvement 2: Supply Chain References

New top-level section pointing to supply chain artifacts. These are URLs to externally-hosted resources, not inline data.

### Schema

```yaml
supplyChain:
  # SBOM location(s) — can be a URL to a published SBOM file or
  # an API endpoint that serves the current SBOM.
  sbom:
    - url: https://github.com/example/project/releases/latest/download/sbom.cdx.json
      format: CycloneDX # CycloneDX | SPDX
    - url: https://github.com/example/project/releases/latest/download/sbom.spdx.json
      format: SPDX

  # OpenSSF Scorecard — URL to the project's scorecard results.
  # The canonical pattern is:
  #   https://scorecard.dev/viewer/?uri=github.com/{owner}/{repo}
  # Alternatively, a direct API URL:
  #   https://api.scorecard.dev/projects/github.com/{owner}/{repo}
  scorecard: https://scorecard.dev/viewer/?uri=github.com/example/project

  # Security policy — URL to the project's vulnerability disclosure
  # policy. This may be a GitHub SECURITY.md (rendered at
  # /security/policy), a .well-known/security.txt file per RFC 9116,
  # a dedicated security page, or any equivalent resource.
  securityPolicy: https://github.com/example/project/security/policy

  # REUSE compliance — whether the project follows the FSFE REUSE
  # specification (https://reuse.software/). Boolean or URL to
  # the REUSE lint results.
  reuse: https://api.reuse.software/status/github.com/example/project
```

### Design Rationale

- **SBOMs are release artifacts**, not source-tree files. They change with every release. The `sbom` field points to where the latest SBOM can always be found.
- **Scorecard results are computed externally** by the OpenSSF infrastructure. The field simply links to the canonical viewer URL. Crawlers can follow this to fetch the score programmatically via the [Scorecard API](https://api.scorecard.dev/).
- **`securityPolicy` is intentionally format-agnostic.** `SECURITY.md` is a de facto convention popularized by GitHub (which renders it as a "Security policy" tab), but it has no formal specification — its content is free-form prose. The more rigorous alternative is [RFC 9116 `security.txt`](https://www.rfc-editor.org/rfc/rfc9116), an IETF standard that defines a machine-parseable file at `/.well-known/security.txt` for declaring vulnerability disclosure contacts, preferred languages, and policy URLs for a domain. The `securityPolicy` field accepts any URL — a GitHub SECURITY.md page, a `/.well-known/security.txt` endpoint, or a project's dedicated security page — because prescribing the format would exclude projects that follow RFC 9116 rather than the GitHub convention. Crawlers that want to parse disclosure metadata programmatically should prefer projects that publish an RFC 9116-compliant endpoint.
- **`SECURITY.md` is part of a broader emerging pattern of repository-level policy files.** Similar conventions are developing for other dimensions: [`SUSTAINABILITY.md`](https://github.com/mgifford/sustainability.md) (currently in draft) documents a project's environmental commitments and targets, following the [Web Sustainability Guidelines](https://w3c.github.io/sustyweb/); [`ACCESSIBILITY.md`](https://github.com/mgifford/ACCESSIBILITY.md) (currently in draft) documents Web Content Accessibility Guidelines (WCAG) conformance levels, known accessibility gaps, and contributor expectations. None of these have reached the status of formal standards yet, but they follow the same convention: a named file in the repository root that makes a policy dimension legible to humans and increasingly to tools. The `supplyChain` section is intentionally scoped to supply-chain and compliance artifacts. Sustainability and accessibility declarations belong in the future `supports` key (see below) rather than as additional `supplyChain` subfields.
- **REUSE compliance** is already checked by openCode.de badges. Making it a first-class field in publiccode.yml formalizes what's already practiced.
- **Relationship to the upcoming `supports` key.** The publiccode.yml maintainers are considering a generic `supports` key for declaring policy compliance and security frameworks in a future spec version. The `supplyChain` fields proposed here are intentionally URL-based and reference external standards (SBOMs, Scorecard, REUSE) rather than defining new inline vocabulary — making them compatible with whatever form `supports` takes when it is proposed. The `supports` key is also the natural home for sustainability and accessibility policy declarations as those conventions mature.

### What This Enables

1. **Procurement security assessment.** Before adopting software, a procurement officer can check its OpenSSF Scorecard, review the vulnerability disclosure policy, and verify SBOM availability — all from a single metadata file, without hunting across multiple external sources.
2. **Compliance verification.** The `reuse` field lets procurement offices confirm FSFE REUSE compliance (per-file licensing) as part of their legal due diligence, and the `securityPolicy` field confirms the project has a responsible disclosure process.
3. **Automated trust signals.** Crawlers (openCode.de, EU OSS Catalogue) can fetch scorecard scores and REUSE status via the referenced URLs, enabling badges and filters like "show me only projects with an OpenSSF score above 7" or "only REUSE-compliant projects."
4. **CRA and NIS2 compliance evidence.** For projects operating under the [Cyber Resilience Act](https://digital-strategy.ec.europa.eu/en/policies/cra-open-source) as an open-source software steward, the `supplyChain` fields make the required security artifacts — cybersecurity policy, SBOM, vulnerability disclosure process — machine-discoverable without additional reporting overhead. For [NIS2](https://digital-strategy.ec.europa.eu/en/policies/nis2-directive)-covered deploying organizations, the same references satisfy supply chain risk assessment obligations for OSS components in their stack.

---

## Improvement 3: Vendor Credit System Discovery

The vendor/contributor credit data itself is too dynamic to live in a git-committed file. Instead, publiccode.yml points to one or more **credit registries** — external databases that track who contributes to the project and in what capacity.

Projects vary widely in how they track contributions. Drupal operates the most mature example: a [weighted credit system](https://www.drupal.org/drupalorg/docs/marketplace/contribution-credit-weight-and-impact-on-ranking) backed by an [open-source, ticket-system-agnostic module](https://git.drupalcode.org/project/contribution_records) with an API — infrastructure that could in principle be adopted by other projects or operated as a SaaS. Other projects may prefer simpler approaches: GitHub Sponsors lists, or mapping code contributions to vendors through contributor employer declarations — but these capture only funding or commits, missing the contributors who sustain a project through testing, UX design, documentation, community organization, event work, or issue triage. The most complete credit registries recognize this full spectrum; a registry that credits only financial sponsors or commit authors gives a systematically incomplete picture of who actually keeps a project alive. This proposal standardizes only the **read interface** for credit data (the [Credit Registry API](#credit-registry-api-rough-outline)), not the methodology for earning or weighting credits — that is a per-project decision beyond the scope of this proposal.

### Who Operates a Credit Registry?

A credit registry can be operated by the project itself, by a generic third-party platform, or by a SaaS provider. What matters is that the OSS project **endorses** it — by listing it in `creditRegistries`, the project signals "we consider this data authoritative for our contributors."

| Operator                         | Example                                                                                                                      | Sign-off requirement                                                                                 |
| -------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| **The project itself**           | Drupal.org Marketplace, a self-hosted `contribution_records` instance                                                        | Project defines its own process for reviewing and approving credits                                  |
| **Generic third-party platform** | GitHub Sponsors                                                                                                              | Pointing at the GitHub Sponsors URL is sufficient; no additional setup required                      |
| **SaaS / shared installation**   | A hosted `contribution_records` instance configured per-project, or a common credit platform that multiple projects opt into | Depends on what the platform offers; may range from fully automated to requiring maintainer sign-off |

The required depth of involvement scales with the registry's sophistication, but the spectrum is not flat — project sign-off is the goal, not just one option among equals. A project that points at a GitHub Sponsors page is endorsing a third-party platform's data without reviewing the underlying attribution: it is a low-friction starting point, not the intended end state. The ideal is for the project to take active responsibility for credit: deciding what contribution types count (code, testing, design, organization, documentation, etc.), reviewing whether the data accurately reflects who sustains the project, and defining a process for sign-off. Projects that invest in this — whether through a custom registry, a SaaS platform with maintainer review, or a formal credit policy — produce data that is meaningfully more trustworthy for procurement decisions and fairer to the full range of contributors.

### Schema

```yaml
# Pointers to external systems that track vendor/contributor
# credits for this project. Each entry identifies a registry
# and this project's identifier within that registry.
creditRegistries:
  - # Human-readable name of the registry
    name: Drupal.org Marketplace
    # URL to this project's credit/contribution page in the registry
    url: https://www.drupal.org/project/drupal/credit
    # API endpoint conforming to the Credit Registry API spec
    # (see "Credit Registry API" section below)
    apiUrl: https://www.drupal.org/api/v1/credits/project/drupal
    # Type of registry — helps crawlers understand what data
    # to expect
    type: contribution-credits # contribution-credits | vendor-directory | funding

  - name: ecosyste.ms
    url: https://ecosyste.ms/projects/lookup?url=https://github.com/example/project
    apiUrl: https://funds.ecosyste.ms/api/v1/projects/github.com/example/project
    type: funding

  - name: Open Source Pledge
    url: https://opensourcepledge.com/projects/example
    apiUrl: https://opensourcepledge.com/api/v1/projects/example
    type: funding
```

### What This Enables

1. **Procurement officers** can follow the `creditRegistries` links to see which vendors actively contribute to the software they're evaluating — similar to checking the [Drupal Marketplace](https://www.drupal.org/drupalorg/docs/marketplace) before hiring an agency.
2. **Crawlers** (openCode.de, EU OSS Catalogue) can aggregate credit data across registries to build composite vendor profiles.
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

| Field                             | Why it goes stale                                                                         | Authoritative source                                                   |
| --------------------------------- | ----------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| `softwareVersion`                 | Changes with every release; rarely updated in publiccode.yml                              | Forge API (GitHub releases, GitLab tags), package registry             |
| `releaseDate`                     | Same as above — coupled to `softwareVersion`                                              | Forge API, package registry                                            |
| `dependsOn[].versionMin`          | Dependency minimum versions evolve with each release                                      | Package manager lockfiles, SBOM (already referenced in `supplyChain`)  |
| `maintenance.contractors[].until` | Contract expiry dates are rarely updated; create a false impression of active maintenance | Internal contract management systems; out of scope for a metadata file |

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

Structurally, `contractors[]` is the inline, project-asserted minimal form of a credit registry entry. A contractor is simply an entity with a `support-contract` relationship to the project — equivalent to a credit registry entry with `roles: ["support-contract"]`. For projects that do not operate or endorse a credit registry, `contractors[]` is the appropriate lightweight mechanism. For projects that do have a credit registry, support contractors should be listed there (with `roles: ["support-contract"]`) and `contractors[]` may be omitted or used as a convenience mirror. The Credit Registry API includes `support-contract` as a valid role type alongside `code`, `maintainer`, `funding`, and others (see [Credit Registry API](#credit-registry-api-rough-outline)).

Only `contractors[].until` is deprecated because contract expiry dates create a false impression of maintenance status while being essentially unmaintained in practice.

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

## Full Example

Putting all improvements together:

```yaml
publiccodeYmlVersion: "1.0"

name: MedusaCMS
applicationSuite: MegaProductivitySuite
url: https://github.com/example/medusa-cms
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

# ===== NEW: Supply chain references =====
supplyChain:
  sbom:
    - url: https://github.com/example/medusa-cms/releases/latest/download/sbom.cdx.json
      format: CycloneDX
  scorecard: https://scorecard.dev/viewer/?uri=github.com/example/medusa-cms
  securityPolicy: https://github.com/example/medusa-cms/security/policy
  reuse: https://api.reuse.software/status/github.com/example/medusa-cms

# ===== NEW: Credit registry discovery =====
creditRegistries:
  - name: MedusaCMS Vendor Directory
    url: https://medusa-cms.example.org/vendors
    apiUrl: https://medusa-cms.example.org/api/v1/credits
    type: contribution-credits
  - name: ecosyste.ms
    url: https://ecosyste.ms/projects/lookup?url=https://github.com/example/medusa-cms
    apiUrl: https://packages.ecosyste.ms/api/v1/packages/lookup?repository_url=https://github.com/example/medusa-cms
    type: funding

# Deprecated — usage data comes from external registries that
# independently index projects by their url field.
# See: Registry Discovery Standard
usedBy:
  - Stadt München
  - Comune di Roma
```

---

## Credit Registry API (Rough Outline)

This is a **separate specification** from publiccode.yml. It defines the API that credit registries (referenced from `creditRegistries[].apiUrl`) must implement so that crawlers can aggregate vendor/contributor data across registries.

### Design Principles

1. **Read-only public API.** The write side (how credits are recorded) is registry-specific. Only the read side is standardized.
2. **Project-centric queries.** Given a project identifier (repository URL), return all credited organizations and individuals.
3. **Time-windowed.** Credits are meaningful in context of recency. A company that contributed heavily 5 years ago but not since is different from one actively contributing.
4. **Inspired by Drupal's Contribution Records system.** Drupal tracks contributions per-issue, links them to users and organizations, and exposes them via [JSON:API endpoints](https://www.drupal.org/drupalorg/docs/apis/rest-and-other-apis) (`contribution-records-by-organization`, `contribution-records-by-user`). The underlying [`contribution_records` module](https://git.drupalcode.org/project/contribution_records) is open source and architecturally ticket-system-agnostic — it could be adopted by other projects or operated as a SaaS for projects that want a proven credit system without building one from scratch.

### Endpoints

#### `GET /projects/{project-url}/credits`

Returns all credited entities for a project. Supports a `since` parameter for time-windowed queries.

```
GET /projects/github.com%2Fexample%2Fmedusa-cms/credits?since=2025-01-01
```

The response identifies the project and registry, then lists credited entities with the following key fields per entity:

| Field                   | Purpose                                                                                                                                                                                      |
| ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `entity.type`           | `organization` or `individual`                                                                                                                                                               |
| `entity.identifier`     | Stable identifier — URL, ROR ID, Wikidata QID, LEI, etc. See [Entity Identity](#entity-identity) for recommended schemes and how verification trust should be carried through API responses. |
| `summary.totalCredits`  | All-time credit count (registry-defined unit)                                                                                                                                                |
| `summary.periodCredits` | Credits in the requested time window                                                                                                                                                         |
| `summary.roles`         | Types of contributions: `code`, `documentation`, `security`, `translation`, `maintainer`, `funding`, `triage`, `design`, `support-contract`                                                  |
| `ranking.position`      | Rank among all credited entities for this project                                                                                                                                            |
| `ranking.tier`          | Registry-defined tier (e.g., `platinum`, `gold`, `silver` — or Drupal's `premium`, `certified`, `contributing`)                                                                              |

#### `GET /organizations/{org-identifier}/credits`

Inverse query: given an organization, return all projects they have credits in. This lets procurement officers look up a vendor and see their full portfolio of contributions.

### What This Doesn't Standardize (Intentionally)

- **How credits are earned.** Each project/registry defines its own contribution tracking. Drupal's system weights issue credits by project usage (more users = more credit per issue), adds credits for case studies, event sponsorships, organizational membership, and sponsored contributor roles. Other projects might simply count commits, reviews, or funding — or use GitHub Sponsors as a proxy for organizational investment. The API reports the _result_, not the methodology.
- **Attribution policies.** Registries differ in how credits follow people and organizations over time. In Drupal's system, credits attributed to a vendor through a contributor remain with that vendor even after the contributor changes employers — a critical property for procurement stability, since it means a vendor's track record reflects cumulative investment rather than current headcount. Other registries may re-attribute credits when contributors move. The API exposes credit totals and time windows; the attribution policy is registry-specific.
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
    │openCode  │    │Developers│    │ Future   │
    │.de       │    │Italia    │    │ registry │
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
| `capabilities`        | What the registry tracks: `usage` (who uses what), `credits` (who contributes what), or both                                                          |
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
GET /v1/software/github.com%2Fexample%2Fmedusa-cms/adopters
```

#### `GET /organizations/{org-identifier}/software`

Inverse query: what software does this organization use? This enables the "software declaration" use case — a procurement office can publish its entire stack, and crawlers can aggregate this into the reuse badge system.

#### `GET /software` (bulk index)

Returns all projects the registry tracks, paginated. Enables crawlers to do a full sync rather than querying project-by-project.

### Organization-Level Usage Declarations

Usage registries are the canonical aggregation point for adoption data, but the data has to get into registries somehow. The most scalable intake mechanism is a standardized `.well-known` file that deploying organizations publish on their own domains (e.g., `https://muenchen.de/.well-known/publiccode-usage.json`).

#### Key Fields

| Field                       | Purpose                                                                                                                                                                                                                                                                              |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `organization.url`          | The declaring organization's domain — also serves as its verified identity (domain control = proof of authority)                                                                                                                                                                     |
| `organization.sector`       | The organization's sector, using publiccode.yml's `intendedAudience.scope` vocabulary (e.g., `government`, `health`, `education`). Optional.                                                                                                                                         |
| `software[].url`            | The project's canonical URL (matches the `url` field in publiccode.yml)                                                                                                                                                                                                              |
| `software[].status`         | Deployment status: `production`, `pilot`, `evaluation`, or `retired`                                                                                                                                                                                                                 |
| `software[].until`          | When the software was retired (only for `retired` status) — provides an explicit deprecation signal                                                                                                                                                                                  |
| `software[].classification` | Which classification facets apply to this organization's specific use, using publiccode.yml's `classification` vocabulary. The `domain` and `function` facets are most meaningful here; `role`, `layer`, `technology`, and `audience` are not recommended in this context. Optional. |
| `lastUpdated`               | When this file was last modified — crawlers flag files older than a configurable threshold (e.g., 12 months) as potentially stale                                                                                                                                                    |

The schema deliberately excludes software version numbers and infrastructure details — the declaration answers "do you use this project, and for what purpose?" not "how is it deployed?" Technology detection services like BuiltWith already expose comparable detail for public-facing web applications.

#### Classification in Usage Declarations

The `software[].classification` field reuses publiccode.yml's `classification` vocabulary but carries a different assertion authority: **publiccode.yml's `classification` is asserted by the project author** ("this software supports content management and CRM"); **the usage declaration's `classification` is asserted by the deploying organization** ("we use it specifically for content management"). Same controlled vocabulary, different perspective — consistent with the broader pattern in this ecosystem where each actor declares only what they can directly verify.

This distinction is valuable for procurement. A project listing `classification.domain: [content-management, crm]` tells potential adopters what the software _can_ do. A municipality listing `classification.domain: [content-management]` in their usage declaration tells peers what a comparable organization _actually uses it for_ — a more reliable signal for adoption decisions.

```json
{
  "organization": {
    "url": "https://muenchen.de",
    "sector": "government"
  },
  "software": [
    {
      "url": "https://github.com/drupal/drupal",
      "status": "production",
      "classification": {
        "domain": ["content-management"],
        "function": ["authentication", "caching"]
      }
    },
    {
      "url": "https://github.com/nextcloud/server",
      "status": "production",
      "classification": {
        "domain": ["file-management", "collaboration"]
      }
    }
  ],
  "lastUpdated": "2025-09-01"
}
```

#### Design Principles

The file should be maintained as part of software deployment processes, not as a separate bureaucratic exercise. Projects like Drupal or Nextcloud can integrate the generation of this file into their documented deployment procedures — deploying adds an entry, decommissioning marks it `retired`.

For organizations with many deployments across internal and public-facing domains, a two-tier model applies: each deployment publishes its own per-deployment declaration, and an organizational aggregation tool assembles the combined public-facing file on the organization's primary domain. This minimizes staleness and allows both internal and public-facing usage to be declared.

Usage registries aggregate declaration data from multiple complementary sources: `.well-known` crawling (domain control = identity verification), direct declaration via registry UI/API, per-deployment aggregation, and bulk import from internal scanning tools.

### How Credit Registries Relate

Credit registries (listed in publiccode.yml's `creditRegistries`) **can also** publish a registry manifest and appear in the central directory. This means:

- A crawler building a **vendor intelligence dashboard** can discover all credit registries, then for each project, check both the project's endorsed registries (from publiccode.yml) and any additional registries found via the directory.
- The project's `creditRegistries` listing serves as an **endorsement signal** — "we consider this data authoritative" — but the registry is still independently discoverable by crawlers who want a broader view.

### Trust Models

| Model                | Verification                                                                            | Example                              |
| -------------------- | --------------------------------------------------------------------------------------- | ------------------------------------ |
| `verified-domain`    | Organization controls the declared domain (DNS validation or institutional account)     | openCode.de, Developers Italia       |
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

Self-declared sector classification (via `organization.sector` in the usage declaration, or `scope.sectors` in the registry manifest) provides useful signal, but its value depends on whether it has been verified. openCode.de's practice of verifying public sector status via email domain matching against a known list is one implementation pattern for registry-level verification. The exact mechanism is an implementation detail; what matters for the ecosystem is that registries expose _how_ a sector claim was verified, not just the claim itself.

This maps onto the existing trust model:

| Verification level   | Sector claim reliability                                               | Example                                                      |
| -------------------- | ---------------------------------------------------------------------- | ------------------------------------------------------------ |
| `self-reported`      | Low — organization asserts its own sector                              | A company declaring itself `government` without verification |
| `verified-domain`    | Medium — domain control confirmed, sector inferred from domain list    | openCode.de's Keycloak email domain matching                 |
| `signed-attestation` | High — formal attestation, e.g., eIDAS-backed institutional credential | Future EU digital identity infrastructure                    |

#### Trust Chain to Catalog Filters

The trust information established at each layer — entity identity, sector verification, domain control — is only useful if it reaches the procurement decision-maker. Catalog UIs that surface this data can offer trust-aware filters; catalogs that discard it cannot. This is a capability question, not a compliance obligation: no law requires a catalog to offer these filters, but catalogs that do give procurement officers a meaningfully richer decision environment.

Concretely, the Usage Registry API's `/software/{url}/adopters` response and the Credit Registry API's `/projects/{url}/credits` response may include, per entity:

- The organization's stable identifier (and scheme), if one was provided
- The trust model used to verify their identity (`verified-domain`, `signed-attestation`, `self-reported`)
- The verification method used to confirm their sector classification, if the registry performed one

This enables catalog UIs to offer filters such as "show only government-verified deployers", "show only contributions from LEI-identified organizations", or "exclude self-reported data" — filters that are only possible if the underlying trust data flows through. Registries that expose this data produce more useful catalog results; those that don't still participate in the ecosystem at a lower trust level.

### What This Enables

1. **Any organization can start a usage registry** for its jurisdiction or sector without needing buy-in from every project.
2. **Crawlers discover registries, not projects.** The EU OSS Catalogue crawler checks the central directory, fetches all registry manifests, then queries each registry's API. No per-project configuration needed.
3. **Projects remain in control of credit endorsement** via `creditRegistries` in publiccode.yml, while usage data flows independently.
4. **The openCode.de reuse badge model scales globally.** openCode.de is one registry among many. A French equivalent, a Brazilian equivalent, or a sector-specific registry (e.g., healthcare) can all join the ecosystem by publishing a manifest and conforming to the API.
5. **Deploying organizations can declare usage without intermediaries.** By publishing `/.well-known/publiccode-usage.json` on their domain, organizations assert usage with their domain as proof of identity — no registry account required. Registries crawl these files as one intake mechanism alongside direct declarations.
6. **Deployment processes become the source of truth.** When open source projects integrate usage declaration updates into their deployment documentation, the `.well-known` file stays current with actual deployments. Retirement is explicitly signaled, giving the ecosystem a deprecation mechanism that manual registries lack.

---

## Improvement 6: CRA Steward Declaration _(Deferred)_

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
