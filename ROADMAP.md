# Roadmap: From Proposal to Reality

One possible phased implementation plan for the integrated ecosystem proposal: publiccode.yml extensions, companion registries and standards (Credit Registry API, Usage Registry API, Registry Discovery Standard, organization-level declarations), policy frameworks, and decentralized trust networks. The plan is sequenced to leverage existing relationships, build credibility through achievable wins, and address critical risks early.

**Important:** This roadmap is illustrative, not prescriptive. It shows one plausible path forward based on current relationships and known allies. The actual implementation will depend on:

- Which individuals within organizations are willing to champion the vision
- How quickly institutional alignment happens
- What overlapping initiatives already exist in related organizations
- What barriers and competing priorities emerge during conversations with stakeholders

Stakeholder conversations will reshape these phases significantly. What matters most is the sequence of prerequisite steps, not the specific timing or organizational implementation details.

---

## Starting Position

### Who We Know

This roadmap assumes we can activate these existing relationships. Conversations will refine, redirect, or expand this list.

| Ally                                            | What they bring                                                                                                                                                          | Current relationship                             |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------ |
| **ZenDiS / Schleswig-Holstein**                 | Operate openCode.de—the most advanced usage registry and reuse badge system in the EU. Political commitment to digital sovereignty.                                      | Direct acquaintance; prior collaboration         |
| **BFH / CHopen**                                | Operate ossdirectory.com—a natural crawler/catalog that benefitsimmediately from standardized metadata. Academic credibility.                                            | Existing relationship through Schleswig-Holstein |
| **OSBA (German Open Source Business Alliance)** | Represent the vendor community most affected by credit visibility and procurement criteria. Actively developing procurement frameworks.                                  | Existing relationship                            |
| **Sovereign Tech Agency**                       | German government funding for critical open source. Needs metadata to make resource allocation decisions.                                                                | Emerging connection                              |
| **Drupal / PHP community**                      | Operate the most mature vendor credit system in open source—living proof the credit model works. Strong commitment to contribution tracking.                             | Strong existing relationship                     |
| **Andrew Nesbitt / ecosyste.ms**                | Created OSS Taxonomy. Operate the largest open source metadata dataset (100K+ projects, 10M+ packages). Direct expertise in federated indexing.                          | Feedback contributor; existing relationship      |
| **CHAOSS**                                      | Community Health Analytics—defines metrics for open source project health. Work on contribution attribution.                                                             | Direct contact                                   |
| **FSFE (Free Software Foundation Europe)**      | Run the "Public Money? Public Code!" campaign (200+ organizations backing it). Maintain REUSE standard (already integrated into openCode.de). European advocacy network. | Emerging connection                              |
| **publiccode.yml maintainers**                  | Control the specification. Any extension must receive their approval.                                                                                                    | Acquaintance; first contact needed               |

### Critical Prerequisite: Spec Maintainer Buy-in

Risk G1 (small spec governance community) is the single highest-priority blocker. The publiccode.yml spec is maintained by the [publiccodeyml](https://github.com/publiccodeyml/publiccode.yml) GitHub organization with a small maintainer community relative to its institutional adoption. The proposed extensions represent a significant scope increase.

---

## Phase 0: Coalition & Governance

**Goal:** Establish who governs publiccode.yml extensions and build a working group with enough institutional weight to move the spec.

### Actions

1. **Secure publiccode.yml maintainer buy-in.** Through the existing acquaintance with publiccode.yml maintainers (the [publiccodeyml](https://github.com/publiccodeyml/publiccode.yml) org), determine:
   - What is the process for proposing v1.0 extensions?
   - What capacity exists to review and absorb the proposed scope increase?
   - Is Developers Italia still actively involved in spec governance?
   - What support (co-maintenance, tooling, migration scripts) would make the extensions acceptable?

2. **Form a working group** with academic, vendor, procurement, and registry operator perspectives — plus at least one publiccode.yml maintainer as spec liaison.

3. **Engage Developers Italia early.** They co-created publiccode.yml and operate the Italian catalog. Without them, any v1.0 proposal lacks legitimacy.

4. **Engage the OSS Taxonomy author.** The faceted classification proposal directly builds on the [OSS Taxonomy](https://nesbitt.io/2025/11/29/oss-taxonomy.html) work, and ecosyste.ms brings data infrastructure as a potential credit registry.

5. **Engage procurement policy stakeholders.** Position the proposed extensions as the data infrastructure that procurement policy initiatives (APELL, EuroStack, FSFE's Public Money? Public Code!) need. The extensions answer the practical "how do you actually procure open source?" question that follows from policy adoption.

### Deliverable

A published working group charter with institutional commitments and at least one publiccode.yml maintainer. A clear understanding of the governance path for extensions. A mapping of active procurement policy initiatives to the extensions they require.

---

## Phase 1: Supply Chain References & Temporal Field Deprecation

**Goal:** Ship the simplest, least controversial extensions first. Build credibility and demonstrate the working group can deliver.

### Why Start Here

- **Extension 2 (Supply Chain References)** requires no new infrastructure — it only adds URL fields to publiccode.yml pointing to things that already exist (SBOMs, Scorecard, REUSE, security policies).
- **Extension 5 (Deprecate Temporal Fields)** pairs naturally with supply chain references. The SBOM reference in `supplyChain` replaces `dependsOn[].versionMin` as the authoritative source for dependency data. Removing `softwareVersion` and `releaseDate` addresses the most common source of stale data — empirically observed across thousands of files indexed by ecosyste.ms.
- openCode.de already checks REUSE compliance and displays badges. Formalizing this in the schema is a natural next step.
- Supply chain security aligns with funder priorities (e.g., Sovereign Tech Agency) and could unlock funding support.
- No chicken-and-egg problem. No new registries needed.

### Actions

1. **Draft the `supplyChain` schema extension** as a formal PR to the publiccode.yml spec.
2. **Draft the temporal field deprecation** (`softwareVersion`, `releaseDate`, `dependsOn[].versionMin`) as a companion PR, motivated by empirical evidence from ecosyste.ms indexing.
3. **Implement support in at least two software indexes** (e.g., openCode.de, ossdirectory.com) — demonstrating multi-crawler interoperability from day one. Crawlers stop relying on `softwareVersion` and pull release data from forge APIs instead.
4. **Pilot with 5-10 projects** that already have Scorecards and SBOMs.
5. **Demonstrate supply chain visibility to funders** — the metadata enables exactly the investment decision support they need.

### Deliverable

Accepted publiccode.yml extensions for `supplyChain` and temporal field deprecation. At least two operational crawlers consuming supply chain data. A handful of real projects using the new fields without temporal fields.

---

## Phase 2: Faceted Classification

**Goal:** Replace flat `categories` with the faceted classification system. This can overlap with Phase 1 since it's also a schema-only change.

### Why This Comes Second

- More complex than supply chain references (migration path, vocabulary governance) but still doesn't require external infrastructure.
- The OSS Taxonomy vocabulary already exists — the classification proposal builds directly on it.
- Vendor communities and procurement offices can validate that the faceted classification actually improves discovery.

### Actions

1. **Align the `classification` schema with OSS Taxonomy dimensions.** Collaborate with the OSS Taxonomy author. Publish a joint vocabulary artifact.
2. **Build a classification wizard** in a publiccode.yml editor — reducing the complexity risk (A6) by guiding maintainers through facet selection.
3. **Provide automated migration tooling** that converts existing `categories` + `softwareType` to the new `classification` structure for the 640+ projects in the EU OSS Catalogue.
4. **Pilot faceted search on at least one catalog** — demonstrate the procurement discovery improvement ("show me a CRM for healthcare in the backend layer").
5. **Draft procurement selection criteria** aligned with the [OSBA framework](https://osb-alliance.de/publikationen/veroeffentlichungen/selection-criteria-for-the-sustainable-procurement-of-open-source-software) that reference publiccode.yml extensions as evidence sources. Validate with a real procurement office. Publish as a companion guide: "How to use publiccode.yml metadata in procurement scoring."

### Deliverable

Accepted `classification` extension. Migration tooling. Faceted search operational on at least one catalog. Published vocabulary with a community contribution process. Draft procurement selection criteria referencing publiccode.yml metadata.

---

## Phase 3: Credit System Pilot with Drupal

**Goal:** Prove the credit registry architecture works using Drupal's existing credit system as the first conforming registry.

### Why Drupal First

- Drupal already has the most mature contribution credit system in open source — issue credits, organizational attribution, marketplace tiers.
- Drupal's system is exactly what the proposal describes: a registry that tracks who contributes, endorsed by the project, used by procurement offices to evaluate vendors.
- This solves the chicken-and-egg problem (A2): we don't need to build a new registry, we need to put a standardized API in front of an existing one.

### Actions

1. **Define the Credit Registry API v0.1** by mapping it to Drupal's existing API endpoints. The Drupal community validates whether the abstraction captures their system accurately.
2. **Build a Credit Registry API adapter for Drupal.org** — a thin layer that translates Drupal's existing contribution data into the standardized format.
3. **Add `creditRegistries` to Drupal's publiccode.yml** (or equivalent metadata) — Drupal endorses its own credit system via the new field.
4. **Implement credit data aggregation in crawlers** — follow `creditRegistries` links and display vendor credit data alongside project metadata.
5. **Engage a second credit registry** (e.g., ecosyste.ms) to implement the Credit Registry API for contribution/funding data, demonstrating multi-registry aggregation.
6. **Run a procurement pilot with credit data.** Work with a procurement office to use credit registry data in an actual tender evaluation — scoring vendors by verified upstream contributions as the [OSBA criteria](https://osb-alliance.de/publikationen/veroeffentlichungen/selection-criteria-for-the-sustainable-procurement-of-open-source-software) and [APELL](https://apell.info/) propose. Document the process and outcomes as a replicable case study.
7. **Evaluate Drupal's `contribution_records` module for reuse.** The module is [open source and ticket-system-agnostic](https://git.drupalcode.org/project/contribution_records). Assess what it would take to package it as a standalone credit system that other OSS projects can adopt — either self-hosted or as a SaaS. Identify gaps (e.g., non-Drupal issue tracker integrations, Credit Registry API conformance) and estimate the effort to close them.

### Deliverable

An assessment of `contribution_records` reusability with a concrete plan for packaging it for other projects. Two operational credit registries (Drupal.org and at least one more larger OSS project) conforming to a published API spec with `creditRegistries` in its publiccode.yml.

---

## Phase 4: Registry Discovery & Usage Registries

**Goal:** Formalize the Registry Discovery Standard so that usage registries (openCode.de, Developers Italia) become discoverable by any crawler.

### Why This Comes After Credits

- The credit system pilot (Phase 3) validates the registry API pattern. The usage registry API follows the same architectural model.
- openCode.de's badge system already functions as a usage registry — it just needs a standardized API and manifest.
- By this point, the working group has shipped three extensions and has credibility to propose the more ambitious companion specifications.

### Actions

1. **First registry publishes a `/.well-known/publiccode-registry.json` manifest** — becoming discoverable via the standard. openCode.de is the natural candidate given its existing badge system.
2. **Implement the Usage Registry API** on the first registry — standardizing the adoption/reuse data that already powers its badge system.
3. **Engage a second usage registry** (e.g., Developers Italia) to demonstrate cross-registry interoperability.
4. **Publish the central registry directory** (bootstrap list) with initial entries.
5. **Define the `/.well-known/publiccode-usage.json` specification** for organization-level usage declarations. Pilot with a small set of municipalities willing to publish their software usage on their domains.
6. **Work with deployment-heavy projects** (e.g., Drupal, Nextcloud) to integrate usage declaration updates into their documented deployment processes.
7. **Prototype an internal scan-and-publish tool** that organizations can run to inventory deployed software and generate a `.well-known/publiccode-usage.json` or bulk-import into a registry.
8. **Deprecate `usedBy`** in the publiccode.yml spec (Extension 4) — now that proper mechanisms exist.
9. **Demonstrate cross-registry aggregation in a crawler** — discovering registries, crawling `.well-known/publiccode-usage.json` files from organizational domains, and presenting a unified view.

### Deliverable

Registry Discovery Standard published. Organization-level usage declaration spec published. At least two operational usage registries crawling both direct declarations and `.well-known` files. At least one open source project with deployment-integrated usage declaration updates. A crawler demonstrating automated registry discovery and cross-source aggregation. `usedBy` deprecated.

---

## Phase 5: Scale & Broader Adoption

**Goal:** Expand beyond the initial coalition. Address the branding risk (A1).

### Actions

1. **Engage the EU Commission / Interoperable Europe** to adopt the extensions in the EU OSS Catalogue — this is the forcing function for broad adoption.
2. **Address the branding problem (A1).** Work with the publiccode.yml governance body on either:
   - A neutral alias/profile (e.g., "softwarecode.yml") that maps 1:1, or
   - A reframing campaign demonstrating the standard's value beyond government.
3. **Engage non-EU players** — Canada's Open Resource Exchange, U.S. DSACMS, Brazilian government programs — to adopt publiccode.yml with country extensions.
4. **Build the conformance test suite** for the Credit Registry API and Usage Registry API — critical for scaling beyond hand-coordinated implementations (risk T3).
5. **Develop auto-generation tooling** that populates publiccode.yml from package.json, pyproject.toml, Cargo.toml etc. — reducing maintainer burden (risk A3) and making adoption viable for the long tail of projects.
6. **Publish procurement selection criteria as a formal recommendation.** Refine the draft criteria from Phase 2 based on the Phase 3 procurement pilot. Work with OSBA, APELL, and EuroStack to position the criteria for adoption in national and EU-level procurement guidelines. Target inclusion in frameworks like the [Interoperable Europe Act](https://interoperable-europe.ec.europa.eu/) implementing measures.
7. **Engage legislators using the Swiss EMBAG as a model.** Document EMBAG's outcomes and package a "legislative toolkit" — model clauses that reference publiccode.yml metadata for procurement compliance, supply chain transparency, and vendor evaluation. Coordinate with FSFE's Public Money? Public Code! campaign to connect legislation advocacy with the metadata infrastructure that makes compliance practical.
8. **Ship a turnkey credit system for OSS projects.** Based on the Phase 3 reusability assessment, package Drupal's `contribution_records` module (or a derivative) as a standalone credit system that any OSS project can deploy — either self-hosted or as a hosted SaaS. Provide integrations for common issue trackers (GitHub, GitLab, Jira) and out-of-the-box Credit Registry API conformance. This lowers the barrier for projects that want vendor credit tracking but lack the resources to build it themselves.

### Deliverable

EU OSS Catalogue consuming the full extension set. At least one non-EU country adopting publiccode.yml. Conformance test suites published. Auto-generation tooling available for major ecosystems. Published procurement selection criteria endorsed by at least two procurement policy organizations. Legislative toolkit with model clauses referencing publiccode.yml metadata. A deployable credit system solution available for adoption by OSS projects beyond Drupal.

---

## Additional Key Players Needed

Players not yet in the coalition but necessary for the proposal to succeed. (Players with existing relationships are listed in [Confirmed or Likely Allies](#confirmed-or-likely-allies) above.)

### Essential (without them, the proposal stalls)

| Player                                   | Why they're essential                                                                                                        |
| ---------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| **Developers Italia**                    | Co-creators of publiccode.yml. Governance legitimacy. Operate the Italian catalog and can become a usage registry.           |
| **EU Commission / Interoperable Europe** | Operate the EU OSS Catalogue (640+ projects). Their adoption of extensions is the forcing function for the entire ecosystem. |

### Valuable Potential Allies

| Player                         | Why they matter                                                                                                                                                                                                             |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **OpenSSF**                    | Scorecard integration in `supplyChain`. Credibility on supply chain security.                                                                                                                                               |
| **Foundation for Public Code** | Dutch foundation maintaining [The Standard for Public Code](https://standard.publiccode.net/) — a broader framework that recommends publiccode.yml. Could amplify adoption by referencing the extensions in their standard. |
| **APELL**                      | [Proposes](https://apell.info/) making upstream contribution an explicit procurement criterion. Needs credit and classification metadata as evidence infrastructure for enforceable criteria.                               |
| **EuroStack**                  | [Coalition](https://eurostack.eu/) of 260+ companies advocating a "Buy Open Source Act" at EU level. Needs the same procurement metadata infrastructure.                                                                    |

### Important for Global Scale (Phase 5)

| Player                                                        | Why they matter                                                                                                   |
| ------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| **DSACMS (U.S.)**                                             | gov-codejson could adopt publiccode.yml with a U.S. country extension instead of maintaining a parallel standard. |
| **Canada Open Resource Exchange**                             | Already discussed adopting publiccode.yml. English-speaking government OSS community.                             |
| **Software Heritage**                                         | Academic/archival legitimacy. CodeMeta bridge.                                                                    |
| **Large OSS foundations (Apache, Eclipse, Linux Foundation)** | Adoption beyond government-specific projects. Address branding risk A1.                                           |

---

## Sequencing Rationale

The phases are ordered to:

1. **Solve governance first** (Phase 0) — nothing moves without this.
2. **Ship the easiest win** (Phase 1: supply chain) — builds credibility, requires no new infrastructure, aligns with Sovereign Tech Agency interests.
3. **Deliver procurement value early** (Phase 2: classification) — gives procurement offices a reason to care, gives OSBA a reason to champion.
4. **Prove the hardest piece with an existing system** (Phase 3: credits via Drupal) — avoids the chicken-and-egg problem by retrofitting a standardized API onto something that already works.
5. **Formalize what's already happening** (Phase 4: registry discovery) — openCode.de is already a usage registry; the standard just makes it interoperable.
6. **Scale once the pattern is proven** (Phase 5) — branding and global expansion only make sense after the core architecture works.

A **procurement policy track** runs in parallel across the phases: stakeholder mapping (Phase 0), draft criteria (Phase 2), procurement pilot with credit data (Phase 3), and formal criteria publication with legislative engagement (Phase 5). This mirrors the technical sequencing — you can't write procurement criteria for credit data before credit registries exist, and you can't run a procurement pilot before the data is real.

Each phase produces tangible, demonstrable results that justify the next phase to funders and stakeholders. No phase depends on solving the hardest problems first.
