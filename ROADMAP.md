# Roadmap: From Proposal to Reality

One possible phased implementation plan for the [publiccode.yml v1.0 extensions](PROPOSAL.md), sequenced to leverage existing allies, build momentum with achievable milestones, and address [critical risks](RISK_ANALYSIS.md) in order.

This roadmap is illustrative, not prescriptive. Its purpose is to make the [proposal](PROPOSAL.md) more concrete by showing one plausible path to realization. The actual sequence will depend heavily on which individuals within key organizations can be convinced of the vision and how quickly institutional buy-in follows. It is also likely that people within these or adjacent organizations are already working on overlapping initiatives — this can create natural allies but also competing priorities and "not invented here" resistance. Conversations with stakeholders will reshape these phases significantly.

---

## Starting Position

### Confirmed or Likely Allies

| Ally                             | What they bring                                                                                                                                | Relationship                  |
| -------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------- |
| **ZenDiS / Schleswig-Holstein**  | Operate openCode.de — the most advanced usage registry and badge system. Political will for digital sovereignty.                               | Likely willing to collaborate |
| **BFH (Bern University of Applied Sciences) / CHopen** | Owns ossdirectory.com — a natural crawler/catalog that benefits directly from standardized data sources. Academic credibility.                 | Home institution              |
| **OSBA**                         | German Open Source Business Alliance — vendor community that directly benefits from credit visibility. Has already approached BFH.             | Direct contact                |
| **Schleswig-Holstein**           | Has approached BFH for ossdirectory.com collaboration. Strong political champion for open source.                                              | Direct contact                |
| **Sovereign Tech Agency**        | Funds critical open source infrastructure. Needs metadata for investment decisions.                                                            | Connections exist             |
| **Drupal / PHP community**       | Operates the most mature vendor credit system in open source. Living proof the credit model works.                                             | Close relationship            |
| **Andrew Nesbitt / ecosyste.ms** | OSS Taxonomy author. Largest open OSS metadata dataset. ecosyste.ms as potential credit/funding registry.                                      | Direct connection             |
| **CHAOSS**                       | Community Health Analytics — defines metrics for open source project health. Alignment on how to measure contributions and community vitality. | Direct contact                |
| **FSFE**                         | Runs the [Public Money? Public Code!](https://publiccode.eu/) campaign. Maintains [REUSE](https://reuse.software/) tooling (already referenced in `supplyChain`). European advocacy network for open source policy. | Direct contact                |
| **Swiss Federal Chancellery / BK** | Operates under [EMBAG](https://www.fedlex.admin.ch/eli/cc/2023/682/en) — the most advanced open source law in Europe. Legislative proof that mandating open source release works. BFH/Prof. Stürmer connections. | Home jurisdiction             |
| **publiccode.yml maintainers**   | Control the spec. Any extension must go through them.                                                                                          | Acquaintances                 |

### Critical Prerequisite: Spec Maintainer Buy-in

Risk G1 (small spec governance community) is the single highest-priority blocker. The publiccode.yml spec is maintained by the [publiccodeyml](https://github.com/publiccodeyml/publiccode.yml) GitHub organization with a small maintainer community relative to its institutional adoption. The proposed extensions represent a significant scope increase. The existing acquaintance with publiccode.yml maintainers is the entry point for securing buy-in.

---

## Phase 0: Coalition & Governance (Months 1-3)

**Goal:** Establish who governs publiccode.yml extensions and build a working group with enough institutional weight to move the spec.

### Actions

1. **Secure publiccode.yml maintainer buy-in.** Through the existing acquaintance with publiccode.yml maintainers (the [publiccodeyml](https://github.com/publiccodeyml/publiccode.yml) org), determine:
   - What is the process for proposing v1.0 extensions?
   - What capacity exists to review and absorb the proposed scope increase?
   - Is Developers Italia still actively involved in spec governance?
   - What support (co-maintenance, tooling, migration scripts) would make the extensions acceptable?

2. **Form a working group.** Recruit from existing allies:
   - **BFH** — academic lead, ossdirectory.com as proving ground
   - **ZenDiS / openCode.de** — usage registry reference implementation
   - **OSBA** — vendor community voice
   - **Schleswig-Holstein** — political champion, procurement perspective
   - One publiccode.yml maintainer as spec liaison

3. **Engage Developers Italia early.** They co-created publiccode.yml and operate the Italian catalog. Without them, any v1.0 proposal lacks legitimacy. The Swiss/ZenDiS connection may help open this door given the existing EU collaboration patterns.

4. **Contact Andrew Nesbitt (ecosyste.ms / OSS Taxonomy).** The faceted classification proposal directly builds on his taxonomy work. His support lends credibility with the broader OSS community and he brings the ecosyste.ms data infrastructure as a potential credit registry.

5. **Engage procurement policy stakeholders.** Connect with APELL and EuroStack to position the proposed extensions as the data infrastructure their policy initiatives need. Coordinate with FSFE to align with the Public Money? Public Code! campaign — the extensions answer the practical "how do you actually procure open source?" question that follows from PMPC adoption. Review EMBAG implementation with the Swiss Federal Chancellery to understand how legislative mandates create demand for this metadata.

### Deliverable

A published working group charter with institutional commitments from at least BFH, ZenDiS, and one publiccode.yml maintainer. A clear understanding of the governance path for extensions. A mapping of active procurement policy initiatives (APELL, EuroStack, PMPC) to the extensions they require.

---

## Phase 1: Supply Chain References & Temporal Field Deprecation (Months 3-6)

**Goal:** Ship the simplest, least controversial extensions first. Build credibility and demonstrate the working group can deliver.

### Why Start Here

- **Extension 2 (Supply Chain References)** requires no new infrastructure — it only adds URL fields to publiccode.yml pointing to things that already exist (SBOMs, Scorecard, REUSE, security policies).
- **Extension 5 (Deprecate Temporal Fields)** pairs naturally with supply chain references. The SBOM reference in `supplyChain` replaces `dependsOn[].versionMin` as the authoritative source for dependency data. Removing `softwareVersion` and `releaseDate` addresses the most common source of stale data — empirically observed across thousands of files indexed by ecosyste.ms.
- openCode.de already checks REUSE compliance and displays badges. Formalizing this in the schema is a natural step ZenDiS will support.
- The Sovereign Tech Agency cares about supply chain security — this extension directly serves their mission and could unlock funding support.
- No chicken-and-egg problem. No new registries needed.

### Actions

1. **Draft the `supplyChain` schema extension** as a formal PR to the publiccode.yml spec.
2. **Draft the temporal field deprecation** (`softwareVersion`, `releaseDate`, `dependsOn[].versionMin`) as a companion PR. Present Andrew Nesbitt's empirical evidence from ecosyste.ms indexing as motivation.
3. **Implement support in openCode.de** — ZenDiS adds crawler support for the new fields, feeding into their existing badge system. Crawlers stop relying on `softwareVersion` and pull release data from forge APIs instead.
4. **Implement support in ossdirectory.com** — BFH's catalog consumes the same fields, demonstrating multi-crawler interoperability from day one.
5. **Pilot with 5-10 projects** that already have Scorecards and SBOMs. Drupal is an obvious candidate given the close relationship.
6. **Present results to the Sovereign Tech Agency** — demonstrate that the metadata enables exactly the supply chain visibility they need for investment decisions.

### Deliverable

Accepted publiccode.yml extensions for `supplyChain` and temporal field deprecation. Two operational crawlers consuming supply chain data (openCode.de, ossdirectory.com). A handful of real projects using the new fields without temporal fields.

---

## Phase 2: Faceted Classification (Months 4-8)

**Goal:** Replace flat `categories` with the faceted classification system. This can overlap with Phase 1 since it's also a schema-only change.

### Why This Comes Second

- More complex than supply chain references (migration path, vocabulary governance) but still doesn't require external infrastructure.
- The OSS Taxonomy collaboration with Andrew Nesbitt makes this feasible — the vocabulary already exists.
- The OSBA and procurement offices in Schleswig-Holstein can validate that the faceted classification actually improves discovery.

### Actions

1. **Collaborate with Andrew Nesbitt** to align the `classification` schema with OSS Taxonomy dimensions. Publish a joint vocabulary artifact.
2. **Build the classification wizard** in the openCode.de publiccode.yml editor — reducing the complexity risk (A6) by guiding maintainers through facet selection.
3. **Provide automated migration tooling** that converts existing `categories` + `softwareType` to the new `classification` structure for the 640+ projects in the EU OSS Catalogue.
4. **Pilot with ossdirectory.com** — implement faceted search to demonstrate the procurement discovery improvement ("show me a CRM for healthcare in the backend layer").
5. **Draft procurement selection criteria** aligned with the [OSBA framework](https://osb-alliance.de/publikationen/veroeffentlichungen/selection-criteria-for-the-sustainable-procurement-of-open-source-software) that reference publiccode.yml extensions as evidence sources. Work with Schleswig-Holstein's procurement office to validate that the criteria are practical for real tenders. Publish as a companion guide: "How to use publiccode.yml metadata in procurement scoring."

### Deliverable

Accepted `classification` extension. Migration tooling. Faceted search operational on at least one catalog. Published vocabulary with a community contribution process. Draft procurement selection criteria referencing publiccode.yml metadata.

---

## Phase 3: Credit System Pilot with Drupal (Months 6-12)

**Goal:** Prove the credit registry architecture works using Drupal's existing credit system as the first conforming registry.

### Why Drupal First

- Drupal already has the most mature contribution credit system in open source — issue credits, organizational attribution, marketplace tiers.
- The close relationship with the Drupal/PHP community makes this a natural pilot.
- Drupal's system is exactly what the proposal describes: a registry that tracks who contributes, endorsed by the project, used by procurement offices to evaluate vendors.
- This solves the chicken-and-egg problem (A2): we don't need to build a new registry, we need to put a standardized API in front of an existing one.

### Actions

1. **Define the Credit Registry API v0.1** by mapping it to Drupal's existing API endpoints. The Drupal community validates whether the abstraction captures their system accurately.
2. **Build a Credit Registry API adapter for Drupal.org** — a thin layer that translates Drupal's existing contribution data into the standardized format.
3. **Add `creditRegistries` to Drupal's publiccode.yml** (or equivalent metadata) — Drupal endorses its own credit system via the new field.
4. **Implement credit data aggregation** in openCode.de and ossdirectory.com — crawlers follow `creditRegistries` links and display vendor credit data alongside project metadata.
5. **Engage ecosyste.ms as a second credit registry** — Andrew Nesbitt implements the Credit Registry API for ecosyste.ms contribution/funding data, demonstrating multi-registry aggregation.
6. **Run a procurement pilot with credit data.** Work with a procurement office (Schleswig-Holstein or a Swiss canton) to use credit registry data in an actual tender evaluation — scoring vendors by verified upstream contributions as the [OSBA criteria](https://osb-alliance.de/publikationen/veroeffentlichungen/selection-criteria-for-the-sustainable-procurement-of-open-source-software) and [APELL](https://apell.info/) propose. Document the process and outcomes as a replicable case study.
7. **Evaluate Drupal's `contribution_records` module for reuse.** The module is [open source and ticket-system-agnostic](https://git.drupalcode.org/project/contribution_records). Assess what it would take to package it as a standalone credit system that other OSS projects can adopt — either self-hosted or as a SaaS. Identify gaps (e.g., non-Drupal issue tracker integrations, Credit Registry API conformance) and estimate the effort to close them.

### Deliverable

An assessment of `contribution_records` reusability with a concrete plan for packaging it for other projects. Two operational credit registries (Drupal.org and at least one more larger OSS project) conforming to a published API spec with `creditRegistries` in its publiccode.yml.

---

## Phase 4: Registry Discovery & Usage Registries (Months 10-16)

**Goal:** Formalize the Registry Discovery Standard so that usage registries (openCode.de, Developers Italia) become discoverable by any crawler.

### Why This Comes After Credits

- The credit system pilot (Phase 3) validates the registry API pattern. The usage registry API follows the same architectural model.
- openCode.de's badge system already functions as a usage registry — it just needs a standardized API and manifest.
- By this point, the working group has shipped three extensions and has credibility to propose the more ambitious companion specifications.

### Actions

1. **openCode.de publishes a `/.well-known/publiccode-registry.json` manifest** — the first registry to be discoverable via the standard.
2. **Implement the Usage Registry API on openCode.de** — standardizing the adoption/reuse data that already powers their badge system.
3. **Engage Developers Italia** to become the second usage registry. Their catalog already tracks Italian public administration software use.
4. **Publish the central registry directory** (bootstrap list) with openCode.de and Developers Italia as initial entries.
5. **Define the `/.well-known/publiccode-usage.json` specification** for organization-level usage declarations. Pilot with a small set of German municipalities willing to publish their software usage on their domains.
6. **Work with Drupal, Nextcloud, and other deployment-heavy projects** to integrate usage declaration updates into their documented deployment processes — so that deploying the software automatically adds an entry to the organization's `.well-known/publiccode-usage.json`.
7. **Prototype an internal scan-and-publish tool** that organizations can run to inventory deployed software and generate a `.well-known/publiccode-usage.json` or bulk-import into a registry. This is particularly valuable for large organizations with hundreds of deployments.
8. **Deprecate `usedBy`** in the publiccode.yml spec (Extension 4) — now that proper mechanisms exist.
9. **ossdirectory.com implements cross-registry aggregation** — demonstrating that a crawler can discover registries, crawl `.well-known/publiccode-usage.json` files from organizational domains, and present a unified view.

### Deliverable

Registry Discovery Standard published. Organization-level usage declaration spec (`/.well-known/publiccode-usage.json`) published. Two operational usage registries (openCode.de, Developers Italia) crawling both direct declarations and `.well-known` files. At least one open source project with deployment-integrated usage declaration updates. Crawler (ossdirectory.com) demonstrating automated registry discovery and cross-source aggregation. `usedBy` deprecated.

---

## Phase 5: Scale & Linked Data (Months 14-24)

**Goal:** Expand beyond the initial coalition. Address the branding risk (A1). Enable the linked data ecosystem.

### Actions

1. **Engage the EU Commission / Interoperable Europe** to adopt the extensions in the EU OSS Catalogue — this is the forcing function for broad adoption.
2. **Address the branding problem (A1).** Work with the publiccode.yml governance body on either:
   - A neutral alias/profile (e.g., "softwarecode.yml") that maps 1:1, or
   - A reframing campaign demonstrating the standard's value beyond government.
3. **Publish the YAML-LD context** (Extension 6) — enabling CodeMeta/Software Heritage interoperability for projects that want it.
4. **Engage non-EU players** — Canada's Open Resource Exchange, U.S. DSACMS, Brazilian government programs — to adopt publiccode.yml with country extensions.
5. **Build the conformance test suite** for the Credit Registry API and Usage Registry API — critical for scaling beyond hand-coordinated implementations (risk T3).
6. **Develop auto-generation tooling** that populates publiccode.yml from package.json, pyproject.toml, Cargo.toml etc. — reducing maintainer burden (risk A3) and making adoption viable for the long tail of projects.
7. **Publish procurement selection criteria as a formal recommendation.** Refine the draft criteria from Phase 2 based on the Phase 3 procurement pilot. Work with OSBA, APELL, and EuroStack to position the criteria for adoption in national and EU-level procurement guidelines. Target inclusion in frameworks like the [Interoperable Europe Act](https://interoperable-europe.ec.europa.eu/) implementing measures.
8. **Engage legislators using the Swiss EMBAG as a model.** Document EMBAG's outcomes and package a "legislative toolkit" — model clauses that reference publiccode.yml metadata for procurement compliance, supply chain transparency, and vendor evaluation. Coordinate with FSFE's Public Money? Public Code! campaign to connect legislation advocacy with the metadata infrastructure that makes compliance practical.
9. **Ship a turnkey credit system for OSS projects.** Based on the Phase 3 reusability assessment, package Drupal's `contribution_records` module (or a derivative) as a standalone credit system that any OSS project can deploy — either self-hosted or as a hosted SaaS. Provide integrations for common issue trackers (GitHub, GitLab, Jira) and out-of-the-box Credit Registry API conformance. This lowers the barrier for projects that want vendor credit tracking but lack the resources to build it themselves.

### Deliverable

EU OSS Catalogue consuming the full extension set. At least one non-EU country adopting publiccode.yml. Conformance test suites published. Auto-generation tooling available for major ecosystems. Published procurement selection criteria endorsed by at least two procurement policy organizations. Legislative toolkit with model clauses referencing publiccode.yml metadata. A deployable credit system solution available for adoption by OSS projects beyond Drupal.

---

## Additional Key Players Needed

Players not yet in the coalition but necessary for the proposal to succeed. (Players with existing relationships are listed in [Confirmed or Likely Allies](#confirmed-or-likely-allies) above.)

### Essential (without them, the proposal stalls)

| Player                                   | Why they're essential                                                                                                        | How to engage                                                                                                   |
| ---------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| **Developers Italia**                    | Co-creators of publiccode.yml. Governance legitimacy. Operate the Italian catalog and can become a usage registry.           | Through ZenDiS/EU collaboration channels.                                                                       |
| **EU Commission / Interoperable Europe** | Operate the EU OSS Catalogue (640+ projects). Their adoption of extensions is the forcing function for the entire ecosystem. | Through ZenDiS and Developers Italia who already feed the EU catalogue. Present at Interoperable Europe events. |

### Valuable Potential Allies

| Player                         | Why they matter                                                                                                                | How to engage                                                                                    |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------ |
| **OpenSSF**                    | Scorecard integration in `supplyChain`. Credibility on supply chain security.                                                  | Through Sovereign Tech Agency. Present the supply chain extension as amplifying Scorecard reach.  |
| **Foundation for Public Code** | Dutch foundation maintaining [The Standard for Public Code](https://standard.publiccode.net/) — a broader framework that recommends publiccode.yml. Could amplify adoption by referencing the extensions in their standard. | No current connection.                                         |
| **APELL**                      | [Proposes](https://apell.info/) making upstream contribution an explicit procurement criterion. Needs credit and classification metadata as evidence infrastructure for enforceable criteria. | Acquaintances. Present credit registries as the data layer their policy proposals need. |
| **EuroStack**                  | [Coalition](https://eurostack.eu/) of 260+ companies advocating a "Buy Open Source Act" at EU level. Needs the same procurement metadata infrastructure. | Acquaintances. Through OSBA (member of the coalition). |

### Important for Global Scale (Phase 5)

| Player                                                        | Why they matter                                                                                                   | How to engage                                             |
| ------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------- |
| **DSACMS (U.S.)**                                             | gov-codejson could adopt publiccode.yml with a U.S. country extension instead of maintaining a parallel standard. | Through Sovereign Tech Agency or CISA contacts.           |
| **Canada Open Resource Exchange**                             | Already discussed adopting publiccode.yml. English-speaking government OSS community.                             | Direct outreach — they're already interested.             |
| **Software Heritage**                                         | Academic/archival legitimacy. YAML-LD/CodeMeta bridge.                                                            | Through the linked data extension (Phase 5).              |
| **Large OSS foundations (Apache, Eclipse, Linux Foundation)** | Adoption beyond government-specific projects. Address branding risk A1.                                           | Later phases. Need proven value before approaching.       |

---

## Sequencing Rationale

The phases are ordered to:

1. **Solve governance first** (Phase 0) — nothing moves without this.
2. **Ship the easiest win** (Phase 1: supply chain) — builds credibility, requires no new infrastructure, aligns with Sovereign Tech Agency interests.
3. **Deliver procurement value early** (Phase 2: classification) — gives procurement offices a reason to care, gives OSBA a reason to champion.
4. **Prove the hardest piece with an existing system** (Phase 3: credits via Drupal) — avoids the chicken-and-egg problem by retrofitting a standardized API onto something that already works.
5. **Formalize what's already happening** (Phase 4: registry discovery) — openCode.de is already a usage registry; the standard just makes it interoperable.
6. **Scale once the pattern is proven** (Phase 5) — branding, linked data, and global expansion only make sense after the core architecture works.

A **procurement policy track** runs in parallel across the phases: stakeholder mapping (Phase 0), draft criteria (Phase 2), procurement pilot with credit data (Phase 3), and formal criteria publication with legislative engagement (Phase 5). This mirrors the technical sequencing — you can't write procurement criteria for credit data before credit registries exist, and you can't run a procurement pilot before the data is real.

Each phase produces tangible, demonstrable results that justify the next phase to funders and stakeholders. No phase depends on solving the hardest problems first.
