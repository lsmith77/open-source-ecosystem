# Proposal: publiccode.yml v1.0 Extensions

A concrete proposal for evolving publiccode.yml to address the gaps identified in the [evaluation](README.md). All proposed changes are backward-compatible additions — existing v0.x files remain valid.

## Contents

- [Design Principles](#design-principles)
- [Actors and Relationships](#actors-and-relationships)
- [Extension 1: Faceted Classification](#extension-1-faceted-classification-replacing-flat-categories)
- [Extension 2: Supply Chain References](#extension-2-supply-chain-references)
- [Extension 3: Vendor Credit System Discovery](#extension-3-vendor-credit-system-discovery)
- [Extension 4: Deprecate `usedBy`](#extension-4-deprecate-usedby)
- [Extension 5: Optional Linked Data Representation (YAML-LD)](#extension-5-optional-linked-data-representation-yaml-ld)
- [Full Example](#full-example)
- **Companion specifications:**
  - [Credit Registry API](#credit-registry-api-rough-outline)
  - [Registry Discovery Standard](#registry-discovery-standard-rough-outline) (includes [Usage Registry API](#usage-registry-api) and [Organization-Level Usage Declarations](#organization-level-usage-declarations))

---

## Design Principles

1. **The publiccode.yml file is the authoritative anchor.** It lives in the repository and is maintained by the project. It describes the project and points to external systems where the project has authority; it does not replicate dynamic data.
2. **Only project-authoritative data belongs in publiccode.yml.** The project can endorse credit registries (it knows who contributes). It cannot endorse usage registries (it doesn't control who uses it). Usage data flows independently.
3. **Decentralized registries discover projects, not the other way around.** Usage registries index projects by their publiccode.yml `url` field. A companion [Registry Discovery Standard](#registry-discovery-standard) makes registries themselves crawlable.
4. **External systems should implement standardized APIs.** So that crawlers (openCode.de, EU OSS Catalogue, Developers Italia) can aggregate data from any conforming provider, not just one.
5. **Faceted classification replaces flat categories.** Enabling multi-dimensional discovery for procurement and supply chain analysis.
6. **Optional linked data representation.** For interoperability with the linked-data ecosystem (CodeMeta, schema.org, Software Heritage) without forcing all maintainers into JSON-LD etc.

---

## Actors and Relationships

The ecosystem this proposal addresses has distinct actors with different authorities and information needs. Understanding who knows what — and who should assert what — drives the architecture. See [PITCH.md](PITCH.md) for what each actor gains economically and politically from this proposal.

### Actors

| Actor                          | Role                                                                                                                                     | Example                                     |
| ------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------- |
| **OSS Project**                | Publishes source code and publiccode.yml. Has authority over its own description, classification, and which contributions it recognizes. | Drupal, Nextcloud, OpenDesk                 |
| **Maintainer**                 | Maintains the project day-to-day. Authors and commits the publiccode.yml file. May be employed by a vendor or an agency.                 | An individual or team                       |
| **Vendor / Service Provider**  | Contributes to projects and sells expertise to procurement offices. Wants to be discoverable as knowledgeable for specific software.     | Acme GmbH, CivicActions                     |
| **Procurement Office**         | Searches for software that meets a need, evaluates vendor expertise, assesses security posture and licensing compliance, and makes buying decisions. | A municipal IT department                   |
| **Deploying Organization**     | Runs the software in production. May or may not be the same as the procurement office. Wants to declare what it uses.                    | Stadt München, Comune di Roma               |
| **Federal Authority / Funder** | Steers money toward supply chain security, identifies ecosystem gaps, sets policy.                                                       | Sovereign Tech Fund, CISA, ZenDiS           |
| **Credit Registry**            | Tracks who contributes to which projects. Endorsed by projects.                                                                          | Drupal.org Marketplace, ecosyste.ms         |
| **Usage Registry**             | Tracks which organizations use which software. Operates independently of projects.                                                       | openCode.de, Developers Italia              |
| **Software Catalog / Crawler** | Aggregates data from publiccode.yml files and registries into a searchable index.                                                        | EU OSS Catalogue, Developers Italia catalog |

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

|                                | Credit registries                                                                                     | Usage registries                                                  |
| ------------------------------ | ----------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| **Who has authority?**         | The project knows who contributes to it.                                                              | Deploying organizations know what they run. The project does not. |
| **Listed in publiccode.yml?**  | Yes — as an endorsement by the project.                                                               | No — the project has no role in tracking its own adoption.        |
| **Discovered by crawlers via** | publiccode.yml `creditRegistries` field + [Registry Discovery Standard](#registry-discovery-standard) | [Registry Discovery Standard](#registry-discovery-standard) only  |
| **How data enters**            | Via the registry's tracking mechanisms (issue credits, commit attribution, manual curation)            | Via direct registry declarations, [`.well-known/publiccode-usage.json`](#organization-level-usage-declarations) on org domains, or bulk imports from internal scans |

This separation keeps each actor in control of the claims they can actually back up.

---

## Extension 1: Faceted Classification (replacing flat `categories`)

The current `categories` field is a flat list of ~100 values (e.g., `content-management`, `crm`, `hr`). This makes it impossible to express that a project is a "healthcare CRM library for backend use" — you'd pick `crm` and lose the rest.

**Proposal:** Replace `categories` with a `classification` section using faceted dimensions inspired by the [OSS Taxonomy](https://nesbitt.io/2025/11/29/oss-taxonomy.html). Keep `categories` as a deprecated alias during transition.

### Schema

```yaml
# New faceted classification (replaces `categories`)
classification:
  # WHAT domain does this software serve?
  # Controlled vocabulary, multiple allowed.
  domain:
    - healthcare
    - public-administration

  # WHAT function does this software perform?
  # Controlled vocabulary, multiple allowed. This is the closest
  # successor to the old `categories` list.
  function:
    - content-management
    - crm
    - identity-management

  # WHAT role does this software play architecturally?
  # Controlled vocabulary from softwareType evolution.
  role:
    - standalone-web # was: standalone/web
    - library
    - addon
    - framework
    - platform

  # WHERE in the stack does it operate?
  layer:
    - backend
    - frontend
    - full-stack

  # WHO is the intended audience?
  # Replaces and extends intendedAudience.scope
  audience:
    - public-administration
    - enterprise
    - developer
    - citizen

  # Free-form tags for anything not covered by the controlled
  # vocabularies above. Uses the namespace:value convention
  # from OSS Taxonomy for forward compatibility.
  tags:
    - "technology:python"
    - "technology:django"
    - "compliance:gdpr"
```

### Migration Path from Current Schema

| Current field                | Maps to                                             | Notes                                                                           |
| ---------------------------- | --------------------------------------------------- | ------------------------------------------------------------------------------- |
| `categories`                 | `classification.function`                           | 1:1 mapping for most values. Old values become the initial function vocabulary. |
| `softwareType`               | `classification.role`                               | Slash syntax (`standalone/web`) becomes hyphenated (`standalone-web`).          |
| `intendedAudience.scope`     | `classification.audience` + `classification.domain` | Split scope values into the appropriate dimension.                              |
| `intendedAudience.countries` | Remains as `intendedAudience.countries`             | Geographic scope is not a classification dimension.                             |

### Why This Matters for Procurement

A procurement officer searching for "CRM for healthcare that runs as a web application" can now query:

```
function=crm AND domain=healthcare AND role=standalone-web
```

With the old flat `categories`, the best they could do was search for `crm` and manually filter — assuming the project even listed itself under `crm` rather than `healthcare`.

### Vocabulary Governance

The controlled vocabularies for `domain`, `function`, `role`, `layer`, and `audience` should be maintained as a separate, versioned artifact (like the current [categories list](https://yml.publiccode.tools/categories-list.html)) with:

- A community contribution process via pull requests (as [OSS Taxonomy](https://nesbitt.io/2025/11/29/oss-taxonomy.html) does)
- Each term having: `name`, `description`, `examples`, `related`, `aliases`
- Versioned releases so that publiccode.yml files can reference a vocabulary version

The `tags` field is intentionally free-form to allow experimentation. Tags that gain traction get promoted into controlled vocabularies in future versions.

---

## Extension 2: Supply Chain References

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

  # Security policy — URL to the project's SECURITY.md or
  # equivalent vulnerability disclosure policy.
  securityPolicy: https://github.com/example/project/security/policy

  # REUSE compliance — whether the project follows the FSFE REUSE
  # specification (https://reuse.software/). Boolean or URL to
  # the REUSE lint results.
  reuse: https://api.reuse.software/status/github.com/example/project
```

### Design Rationale

- **SBOMs are release artifacts**, not source-tree files. They change with every release. The `sbom` field points to where the latest SBOM can always be found.
- **Scorecard results are computed externally** by the OpenSSF infrastructure. The field simply links to the canonical viewer URL. Crawlers can follow this to fetch the score programmatically via the [Scorecard API](https://api.scorecard.dev/).
- **REUSE compliance** is already checked by openCode.de badges. Making it a first-class field in publiccode.yml formalizes what's already practiced.

### What This Enables

1. **Procurement security assessment.** Before adopting software, a procurement officer can check its OpenSSF Scorecard, review the vulnerability disclosure policy, and verify SBOM availability — all from a single metadata file, without hunting across multiple external sources.
2. **Compliance verification.** The `reuse` field lets procurement offices confirm FSFE REUSE compliance (per-file licensing) as part of their legal due diligence, and the `securityPolicy` field confirms the project has a responsible disclosure process.
3. **Automated trust signals.** Crawlers (openCode.de, EU OSS Catalogue) can fetch scorecard scores and REUSE status via the referenced URLs, enabling badges and filters like "show me only projects with an OpenSSF score above 7" or "only REUSE-compliant projects."

---

## Extension 3: Vendor Credit System Discovery

The vendor/contributor credit data itself is too dynamic to live in a git-committed file. Instead, publiccode.yml points to one or more **credit registries** — external databases that track who contributes to the project and in what capacity.

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

## Extension 4: Deprecate `usedBy`

The current `usedBy` field is a simple list of organization names maintained by the project. This has fundamental problems:

- **It's unverifiable** — anyone can claim usage
- **It's high-friction** — organizations must ask projects to update their file
- **It's always stale** — organizations adopt and retire software without notifying projects

As described in [Actors and Relationships](#actors-and-relationships), usage data is asserted by deploying organizations, not by projects. The project has no authority over who uses it. Usage tracking therefore lives entirely outside publiccode.yml, in decentralized [usage registries](#registry-discovery-standard) that index projects by their `url` field.

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

## Extension 5: Optional Linked Data Representation (YAML-LD)

publiccode.yml's YAML format is its strength for maintainer adoption. But the linked-data ecosystem (CodeMeta, schema.org, Software Heritage, Zenodo) speaks JSON-LD. The proposal: make publiccode.yml **optionally** expressible as [YAML-LD](https://www.w3.org/community/reports/json-ld/CG-FINAL-yaml-ld-20231206/).

YAML-LD is a W3C Community Group specification (Final Report, December 2023) that defines how to serialize JSON-LD as YAML. Since YAML is a superset of JSON, any JSON-LD document is valid YAML, and YAML-LD defines the conventions for going the other direction.

### How It Works

A standard publiccode.yml file:

```yaml
publiccodeYmlVersion: "1.0"
name: Example CMS
url: https://github.com/example/cms
classification:
  function:
    - content-management
    - crm
  domain:
    - public-administration
  role:
    - standalone-web
maintenance:
  type: community
  contacts:
    - name: Jane Maintainer
      email: jane@example.org
legal:
  license: AGPL-3.0-or-later
```

The same file as YAML-LD, with a `@context` header:

```yaml
"@context":
  "@vocab": https://w3id.org/publiccode/vocab#
  schema: https://schema.org/
  codemeta: https://codemeta.github.io/terms/
  name:
    "@id": schema:name
  url:
    "@id": schema:codeRepository
  license:
    "@id": schema:license
  classification:
    "@id": publiccode:classification
"@type": schema:SoftwareSourceCode

publiccodeYmlVersion: "1.0"
name: Example CMS
url: https://github.com/example/cms
classification:
  function:
    - content-management
    - crm
  domain:
    - public-administration
  role:
    - standalone-web
maintenance:
  type: community
  contacts:
    - name: Jane Maintainer
      email: jane@example.org
legal:
  license: AGPL-3.0-or-later
```

### Design Decisions

- **The `@context` block is optional.** Files without it are plain publiccode.yml. Files with it are valid YAML-LD and can be processed by any linked data toolchain.
- **The `@context` maps publiccode.yml fields to schema.org and CodeMeta terms.** This is a one-time mapping defined by the standard, not something each project writes.
- **A canonical `@context` URL** (e.g., `https://w3id.org/publiccode/context.jsonld`) could be published by the publiccode.yml governance body. Projects then simply write:

```yaml
"@context": https://w3id.org/publiccode/context.jsonld
"@type": schema:SoftwareSourceCode

publiccodeYmlVersion: "1.0"
name: Example CMS
# ... rest of file unchanged
```

- **Crawlers can produce the linked data representation** even from plain publiccode.yml files by applying the canonical context at indexing time. The YAML-LD header is only needed if the project itself wants to be directly consumable as linked data.

### What This Enables

- **CodeMeta interoperability.** Software Heritage, Zenodo, and other academic infrastructure can ingest publiccode.yml files as linked data without a separate crosswalk step.
- **schema.org visibility.** Search engines that understand schema.org `SoftwareSourceCode` markup can index publiccode.yml metadata.
- **No extra burden on maintainers.** The plain YAML form remains the primary authoring format. The linked data layer is an interoperability feature for the ecosystem, not a requirement for projects.

---

## Full Example

Putting all extensions together:

```yaml
publiccodeYmlVersion: "1.0"

# Optionally make this file valid YAML-LD:
# "@context": https://w3id.org/publiccode/context.jsonld
# "@type": schema:SoftwareSourceCode

name: MedusaCMS
applicationSuite: MegaProductivitySuite
url: https://github.com/example/medusa-cms
landingURL: https://medusa-cms.example.org
softwareVersion: "3.2.1"
releaseDate: "2026-01-15"
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
      until: "2027-12-31"
      website: https://acme.example.org
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

dependsOn:
  open:
    - name: PostgreSQL
      versionMin: "14"

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
4. **Inspired by Drupal's Contribution Records system.** Drupal tracks contributions per-issue, links them to users and organizations, and exposes them via [JSON:API endpoints](https://www.drupal.org/drupalorg/docs/apis/rest-and-other-apis) (`contribution-records-by-organization`, `contribution-records-by-user`).

### Endpoints

#### `GET /projects/{project-url}/credits`

Returns all credited entities for a project.

```
GET /projects/github.com%2Fexample%2Fmedusa-cms/credits?since=2025-01-01
```

#### Response Schema

```json
{
  "project": {
    "url": "https://github.com/example/medusa-cms",
    "name": "MedusaCMS"
  },
  "registry": {
    "name": "MedusaCMS Vendor Directory",
    "url": "https://medusa-cms.example.org/vendors",
    "trustModel": "maintainer-verified"
  },
  "credits": [
    {
      "entity": {
        "type": "organization",
        "name": "Acme GmbH",
        "url": "https://acme.example.org",
        "identifier": {
          "type": "url",
          "value": "https://acme.example.org"
        }
      },
      "summary": {
        "totalCredits": 142,
        "periodCredits": 87,
        "firstContribution": "2022-03-15",
        "lastContribution": "2026-01-28",
        "roles": ["maintainer", "code", "documentation"]
      },
      "ranking": {
        "position": 1,
        "tier": "platinum"
      }
    },
    {
      "entity": {
        "type": "organization",
        "name": "Beta Solutions AG",
        "url": "https://beta-solutions.example.org"
      },
      "summary": {
        "totalCredits": 53,
        "periodCredits": 12,
        "firstContribution": "2023-06-01",
        "lastContribution": "2025-11-14",
        "roles": ["code", "security"]
      },
      "ranking": {
        "position": 2,
        "tier": "gold"
      }
    }
  ],
  "meta": {
    "since": "2025-01-01",
    "generatedAt": "2026-02-07T12:00:00Z",
    "totalEntities": 2
  }
}
```

#### Key Fields

| Field                   | Purpose                                                                                                                 |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `entity.type`           | `organization` or `individual`                                                                                          |
| `entity.identifier`     | Stable identifier — URL, ROR ID, Wikidata QID, etc.                                                                     |
| `summary.totalCredits`  | All-time credit count (registry-defined unit)                                                                           |
| `summary.periodCredits` | Credits in the requested time window                                                                                    |
| `summary.roles`         | Types of contributions: `code`, `documentation`, `security`, `translation`, `maintainer`, `funding`, `triage`, `design` |
| `ranking.position`      | Rank among all credited entities for this project                                                                       |
| `ranking.tier`          | Registry-defined tier (e.g., `platinum`, `gold`, `silver` — or Drupal's `premium`, `certified`, `contributing`)         |

#### `GET /organizations/{org-identifier}/credits`

Inverse query: given an organization, return all projects they have credits in. This lets procurement officers look up a vendor and see their full portfolio of contributions.

```
GET /organizations/acme.example.org/credits?since=2025-01-01
```

### What This Doesn't Standardize (Intentionally)

- **How credits are earned.** Each project/registry defines its own contribution tracking. Drupal counts issue credits. Others might count commits, reviews, or funding. The API reports the _result_, not the methodology.
- **The trust model.** Some registries verify contributions via code review sign-off (high trust). Others use self-reporting (low trust). The `trustModel` field lets consumers assess credibility.
- **Tier definitions.** What "platinum" means varies by registry. This is like credit rating agencies — the scale is per-agency, but the API format is standard.

---

## Registry Discovery Standard (Rough Outline)

This is a **companion specification** to publiccode.yml. It solves a bootstrapping problem: if usage registries are decentralized and not listed in project files, how do crawlers find them?

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

##### Schema

```json
{
  "schema": "https://publiccode.net/registry/v1",
  "registry": {
    "name": "openCode.de",
    "url": "https://opencode.de",
    "description": "German public sector open source software registry",
    "operator": {
      "name": "ZenDiS GmbH",
      "url": "https://zendis.de",
      "jurisdiction": "DE"
    },

    "capabilities": ["usage", "credits"],

    "trustModel": "verified-domain",
    "trustDescription": "Organizations verified via institutional accounts on the openCode.de GitLab instance",

    "api": {
      "version": "v1",
      "baseUrl": "https://api.opencode.de/v1",
      "documentation": "https://opencode.de/en/knowledge/api",
      "conformsTo": [
        "https://publiccode.net/registry-api/usage/v1",
        "https://publiccode.net/registry-api/credits/v1"
      ]
    },

    "scope": {
      "jurisdictions": ["DE"],
      "sectors": ["public-administration"]
    },

    "contact": {
      "email": "registry@opencode.de"
    }
  }
}
```

##### Key Fields

| Field                 | Purpose                                                                                      |
| --------------------- | -------------------------------------------------------------------------------------------- |
| `capabilities`        | What the registry tracks: `usage` (who uses what), `credits` (who contributes what), or both |
| `trustModel`          | How the registry verifies claims: `verified-domain`, `signed-attestation`, `self-reported`   |
| `api.conformsTo`      | Which standardized API specs the registry implements (see below)                             |
| `scope.jurisdictions` | Which countries/regions the registry covers (ISO 3166-1)                                     |
| `scope.sectors`       | Which sectors: `public-administration`, `enterprise`, `education`, `ngo`, etc.               |

#### Discovery Mechanisms

Crawlers discover registries through multiple complementary channels:

1. **Well-known URL probing.** Crawlers can probe known domains for `/.well-known/publiccode-registry.json`. Useful for discovering registries at domains already known to the ecosystem (opencode.de, developers.italia.it, etc.).

2. **Central directory (bootstrap list).** A community-maintained list of known registries, similar to how browsers ship a root certificate store. Published as a simple JSON file:

```json
{
  "schema": "https://publiccode.net/registry-directory/v1",
  "registries": [
    {
      "manifestUrl": "https://opencode.de/.well-known/publiccode-registry.json",
      "addedAt": "2026-01-01"
    },
    {
      "manifestUrl": "https://developers.italia.it/.well-known/publiccode-registry.json",
      "addedAt": "2026-01-15"
    }
  ]
}
```

This directory is maintained via pull requests (like a browser's CA inclusion process) and published at a stable URL (e.g., `https://publiccode.net/registries.json`). Any registry operator can request inclusion.

3. **DNS TXT records (future).** A registry could publish a TXT record at `_publiccode-registry.opencode.de` pointing to its manifest URL. This enables fully decentralized discovery without any central directory.

### Usage Registry API

Registries with `"capabilities": ["usage"]` must implement these endpoints.

#### `GET /software/{project-url}/adopters`

Given a project URL (the `url` field from publiccode.yml, URL-encoded), return all organizations that have declared they use it.

```
GET /v1/software/github.com%2Fexample%2Fmedusa-cms/adopters
```

##### Response Schema

```json
{
  "project": {
    "url": "https://github.com/example/medusa-cms",
    "name": "MedusaCMS"
  },
  "adopters": [
    {
      "organization": {
        "name": "Stadt München",
        "url": "https://muenchen.de",
        "type": "public-administration",
        "identifier": {
          "type": "domain",
          "value": "muenchen.de"
        },
        "jurisdiction": "DE"
      },
      "adoption": {
        "status": "production",
        "since": "2024-06-01",
        "declaredAt": "2025-01-15T10:00:00Z",
        "scope": "city-wide CMS for all municipal websites"
      }
    }
  ],
  "meta": {
    "totalAdopters": 1,
    "generatedAt": "2026-02-07T12:00:00Z"
  }
}
```

#### `GET /organizations/{org-identifier}/software`

Inverse query: what software does this organization use?

```
GET /v1/organizations/muenchen.de/software
```

This enables the "software declaration" use case — a procurement office can publish its entire stack, and crawlers can aggregate this into the reuse badge system.

#### `GET /software` (bulk index)

Returns all projects the registry tracks, paginated. Enables crawlers to do a full sync rather than querying project-by-project.

```
GET /v1/software?page=1&per_page=100
```

### Organization-Level Usage Declarations

Usage registries are the canonical aggregation point for adoption data, but the data has to get into registries somehow. The most scalable intake mechanism is a standardized `.well-known` file that deploying organizations publish on their own domains.

#### `/.well-known/publiccode-usage.json`

An organization declares which open source software it uses by publishing a static JSON file at a well-known URL on its domain:

```
https://muenchen.de/.well-known/publiccode-usage.json
```

##### Schema

```json
{
  "schema": "https://publiccode.net/usage-declaration/v1",
  "organization": {
    "name": "Stadt München",
    "url": "https://muenchen.de",
    "type": "public-administration",
    "jurisdiction": "DE"
  },
  "software": [
    {
      "url": "https://github.com/example/medusa-cms",
      "name": "MedusaCMS",
      "status": "production",
      "since": "2024-06-01"
    },
    {
      "url": "https://github.com/drupal/drupal",
      "name": "Drupal",
      "status": "production",
      "since": "2020-03-15"
    },
    {
      "url": "https://github.com/example/old-portal",
      "name": "OldPortal",
      "status": "retired",
      "since": "2018-01-01",
      "until": "2025-06-30"
    }
  ],
  "lastUpdated": "2026-02-01T00:00:00Z"
}
```

##### Key Fields

| Field                 | Purpose                                                                                                                   |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| `organization.url`    | The declaring organization's domain — also serves as its verified identity (domain control = proof of authority)           |
| `software[].url`      | The project's canonical URL (matches the `url` field in publiccode.yml)                                                   |
| `software[].status`   | Deployment status: `production`, `pilot`, `evaluation`, or `retired`                                                      |
| `software[].until`    | When the software was retired (only for `retired` status) — provides an explicit deprecation signal                       |
| `lastUpdated`         | When this file was last modified — crawlers flag files older than a configurable threshold (e.g., 12 months) as potentially stale |

##### Design Rationale

**Deployment-integrated maintenance.** The file should be maintained as part of software deployment processes, not as a separate bureaucratic exercise. Projects like Drupal, Nextcloud, or WordPress can integrate the generation or update of this file into their documented deployment procedures — when an organization deploys the software, the deployment process adds an entry to the usage declaration. When an organization decommissions software, the teardown process marks the entry as `retired`. This makes the file a living artifact that stays current with actual deployments rather than a document that rots in a forgotten web directory.

**Two-tier aggregation.** In practice, organizations deploy software across many internal and public-facing domains (e.g., `intranet.muenchen.de`, `cloud.muenchen.de`, `kita-portal.muenchen.de`). Each deployment can publish its own `/.well-known/publiccode-usage.json` declaring what software it runs — maintained automatically by that deployment's process. An organizational aggregation tool then crawls these per-deployment declarations (both internal and public-facing) and assembles the combined public-facing `/.well-known/publiccode-usage.json` on the organization's primary domain (e.g., `muenchen.de`). This two-tier model has two advantages: (1) staleness risk is minimized because each deployment maintains its own declaration at the source, and (2) both internal and public-facing open source usage can be declared — the aggregation tool decides what to include in the public file, giving the organization control over what is externally visible.

**Domain as identity.** The organization's control over the domain (via TLS certificate and DNS) proves identity without requiring accounts on any registry. This is the same trust primitive used by the `verified-domain` trust model in registry manifests.

**No version or infrastructure details.** The schema deliberately excludes software version numbers, deployment architecture, and infrastructure details. This limits security exposure — the declaration answers "do you use this project?" not "how is it deployed?" Technology detection services like BuiltWith already expose comparable or greater detail for public-facing web applications; a standardized declaration adds minimal incremental attack surface while providing significant ecosystem value.

**Explicit retirement.** The `retired` status with an `until` date provides a positive deprecation signal. When a crawlable file simply disappears, the cause is ambiguous (decommissioned? server migration? IT reorganization?). An explicit `retired` entry in an otherwise-maintained file is unambiguous.

#### Intake Mechanisms for Usage Registries

Usage registries aggregate declaration data from multiple sources:

1. **`.well-known` crawling.** Registries crawl known organizational domains for `/.well-known/publiccode-usage.json`. For government domains, comprehensive lists are often publicly available (e.g., all `.gov.uk` domains, all German municipal domains). Domain control serves as identity verification — no additional trust negotiation needed.

2. **Direct declaration.** Organizations register on a usage registry (e.g., openCode.de) and declare their software use through the registry's UI or API. This is the existing mechanism and remains the path of least resistance for organizations that cannot easily modify their web server's `.well-known` directory.

3. **Aggregation from per-deployment declarations.** When individual deployments each publish their own `/.well-known/publiccode-usage.json` (see "Two-tier aggregation" above), an organizational aggregation tool crawls these per-deployment files — both internal and public-facing — and assembles the combined public declaration on the organization's primary domain. This tool can also be offered as a service by public-sector IT cooperatives or shared service centers.

4. **Bulk import from internal scans.** Organizations can additionally run scanning tools that detect deployed software via fingerprinting or package inventory, then publish results either as a `.well-known` file or via a registry's bulk import API. This complements per-deployment declarations by catching software that doesn't yet publish its own `.well-known` file.

These mechanisms are complementary, not competing. An organization might use per-deployment declarations for software that integrates them, scanning tools for legacy deployments, and a registry's UI for declarations that require additional context or approval workflows. The organizational aggregation tool merges all sources into the public-facing `/.well-known/publiccode-usage.json`.

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

### What This Enables

1. **Any organization can start a usage registry** for its jurisdiction or sector without needing buy-in from every project.
2. **Crawlers discover registries, not projects.** The EU OSS Catalogue crawler checks the central directory, fetches all registry manifests, then queries each registry's API. No per-project configuration needed.
3. **Projects remain in control of credit endorsement** via `creditRegistries` in publiccode.yml, while usage data flows independently.
4. **The openCode.de reuse badge model scales globally.** openCode.de is one registry among many. A French equivalent, a Brazilian equivalent, or a sector-specific registry (e.g., healthcare) can all join the ecosystem by publishing a manifest and conforming to the API.
5. **Deploying organizations can declare usage without intermediaries.** By publishing `/.well-known/publiccode-usage.json` on their domain, organizations assert usage with their domain as proof of identity — no registry account required. Registries crawl these files as one intake mechanism alongside direct declarations.
6. **Deployment processes become the source of truth.** When open source projects integrate usage declaration updates into their deployment documentation, the `.well-known` file stays current with actual deployments. Retirement is explicitly signaled, giving the ecosystem a deprecation mechanism that manual registries lack.
