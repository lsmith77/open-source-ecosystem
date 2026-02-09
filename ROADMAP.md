# Roadmap: From Proposal to Reality

One possible phased implementation plan for the [publiccode.yml v1.0 extensions](PROPOSAL.md), sequenced to leverage existing allies, build momentum with achievable milestones, and address [critical risks](RISK_ANALYSIS.md) in order.

This roadmap is illustrative, not prescriptive. Its purpose is to make the [proposal](PROPOSAL.md) more concrete by showing one plausible path to realization. The actual sequence will depend heavily on which individuals within key organizations can be convinced of the vision and how quickly institutional buy-in follows. It is also likely that people within these or adjacent organizations are already working on overlapping initiatives — this can create natural allies but also competing priorities and "not invented here" resistance. Conversations with stakeholders will reshape these phases significantly.

---

## Starting Position

### Confirmed or Likely Allies

| Ally                             | What they bring                                                                                                                                | Relationship                  |
| -------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------- |
| **ZenDiS / Schleswig-Holstein**  | Operate openCode.de — the most advanced usage registry and badge system. Political will for digital sovereignty.                               | Likely willing to collaborate |
| **BFH / CHopen**                 | Owns ossdirectory.com — a natural crawler/catalog that benefits directly from standardized data sources. Academic credibility.                 | Home institution              |
| **OSBA**                         | German Open Source Business Alliance — vendor community that directly benefits from credit visibility. Has already approached BFH.             | Direct contact                |
| **Schleswig-Holstein**           | Has approached BFH for ossdirectory.com collaboration. Strong political champion for open source.                                              | Direct contact                |
| **Sovereign Tech Agency**        | Funds critical open source infrastructure. Needs metadata for investment decisions.                                                            | Connections exist             |
| **Drupal / PHP community**       | Operates the most mature vendor credit system in open source. Living proof the credit model works.                                             | Close relationship            |
| **Andrew Nesbitt / ecosyste.ms** | OSS Taxonomy author. Largest open OSS metadata dataset. ecosyste.ms as potential credit/funding registry.                                      | Direct connection             |
| **CHAOSS**                       | Community Health Analytics — defines metrics for open source project health. Alignment on how to measure contributions and community vitality. | Direct contact                |
| **publiccode.yml maintainers**   | Control the spec. Any extension must go through them.                                                                                          | Acquaintances                 |

### Critical Prerequisite: Governance Clarity

Risk G1 (split governance) is the single highest-priority blocker. The two GitHub organizations ([publiccodeyml](https://github.com/publiccodeyml/publiccode.yml) and [publiccodenet](https://github.com/publiccodenet/publiccode.yml)) must be navigated before any extension can move from proposal to spec. The acquaintance with publiccode.yml maintainers is the entry point here.

---

## Phase 0: Coalition & Governance (Months 1-3)

**Goal:** Establish who governs publiccode.yml extensions and build a working group with enough institutional weight to move the spec.

### Actions

1. **Clarify publiccode.yml governance.** Through the existing acquaintance with publiccode.yml maintainers, determine:
   - Which GitHub org is canonical (publiccodeyml vs. publiccodenet)?
   - What is the process for proposing v1.0 extensions?
   - Who are the de facto decision-makers?
   - Is Developers Italia still actively involved in spec governance?

2. **Form a working group.** Recruit from existing allies:
   - **BFH** — academic lead, ossdirectory.com as proving ground
   - **ZenDiS / openCode.de** — usage registry reference implementation
   - **OSBA** — vendor community voice
   - **Schleswig-Holstein** — political champion, procurement perspective
   - One publiccode.yml maintainer as spec liaison

3. **Engage Developers Italia early.** They co-created publiccode.yml and operate the Italian catalog. Without them, any v1.0 proposal lacks legitimacy. The Swiss/ZenDiS connection may help open this door given the existing EU collaboration patterns.

4. **Contact Andrew Nesbitt (ecosyste.ms / OSS Taxonomy).** The faceted classification proposal directly builds on his taxonomy work. His support lends credibility with the broader OSS community and he brings the ecosyste.ms data infrastructure as a potential credit registry.

### Deliverable

A published working group charter with institutional commitments from at least BFH, ZenDiS, and one publiccode.yml maintainer. A clear understanding of the governance path for extensions.

---

## Phase 1: Supply Chain References (Months 3-6)

**Goal:** Ship the simplest, least controversial extension first. Build credibility and demonstrate the working group can deliver.

### Why Start Here

- **Extension 2 (Supply Chain References)** requires no new infrastructure — it only adds URL fields to publiccode.yml pointing to things that already exist (SBOMs, Scorecard, REUSE, security policies).
- openCode.de already checks REUSE compliance and displays badges. Formalizing this in the schema is a natural step ZenDiS will support.
- The Sovereign Tech Agency cares about supply chain security — this extension directly serves their mission and could unlock funding support.
- No chicken-and-egg problem. No new registries needed.

### Actions

1. **Draft the `supplyChain` schema extension** as a formal PR to the publiccode.yml spec.
2. **Implement support in openCode.de** — ZenDiS adds crawler support for the new fields, feeding into their existing badge system.
3. **Implement support in ossdirectory.com** — BFH's catalog consumes the same fields, demonstrating multi-crawler interoperability from day one.
4. **Pilot with 5-10 projects** that already have Scorecards and SBOMs. Drupal is an obvious candidate given the close relationship.
5. **Present results to the Sovereign Tech Agency** — demonstrate that the metadata enables exactly the supply chain visibility they need for investment decisions.

### Deliverable

Accepted publiccode.yml extension for `supplyChain`. Two operational crawlers consuming it (openCode.de, ossdirectory.com). A handful of real projects using it.

---

## Phase 2: Faceted Classification (Months 4-8)

**Goal:** Replace flat `categories` with the faceted classification system. This can overlap with Phase 1 since it's also a schema-only change.

### Why This Comes Second

- More complex than supply chain references (migration path, vocabulary governance) but still doesn't require external infrastructure.
- The OSS Taxonomy collaboration with Andrew Nesbitt makes this feasible — the vocabulary already exists.
- The OSBA and procurement offices in Schleswig-Holstein can validate that the faceted classification actually improves discovery.

### Actions

1. **Collaborate with Andrew Nesbitt** to align the `classification` schema with OSS Taxonomy dimensions. Publish a joint vocabulary artifact.
2. **Build the classification wizard** in the openCode.de publiccode.yml editor — reducing the complexity risk (A5) by guiding maintainers through facet selection.
3. **Provide automated migration tooling** that converts existing `categories` + `softwareType` to the new `classification` structure for the 640+ projects in the EU OSS Catalogue.
4. **Pilot with ossdirectory.com** — implement faceted search to demonstrate the procurement discovery improvement ("show me a CRM for healthcare in the backend layer").

### Deliverable

Accepted `classification` extension. Migration tooling. Faceted search operational on at least one catalog. Published vocabulary with a community contribution process.

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

### Deliverable

Two operational credit registries (Drupal.org, ecosyste.ms) conforming to a published API spec. At least one catalog displaying aggregated credit data. A reference project (Drupal) with `creditRegistries` in its publiccode.yml.

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
5. **Deprecate `usedBy`** in the publiccode.yml spec (Extension 4) — now that a proper mechanism exists.
6. **ossdirectory.com implements cross-registry aggregation** — demonstrating that a crawler can discover registries, fetch usage data from multiple jurisdictions, and present a unified view.

### Deliverable

Registry Discovery Standard published. Two operational usage registries (openCode.de, Developers Italia). Crawler (ossdirectory.com) demonstrating automated registry discovery and cross-registry aggregation. `usedBy` deprecated.

---

## Phase 5: Scale & Linked Data (Months 14-24)

**Goal:** Expand beyond the initial coalition. Address the branding risk (A1). Enable the linked data ecosystem.

### Actions

1. **Engage the EU Commission / Interoperable Europe** to adopt the extensions in the EU OSS Catalogue — this is the forcing function for broad adoption.
2. **Address the branding problem (A1).** Work with the publiccode.yml governance body on either:
   - A neutral alias/profile (e.g., "softwarecode.yml") that maps 1:1, or
   - A reframing campaign demonstrating the standard's value beyond government.
3. **Publish the YAML-LD context** (Extension 5) — enabling CodeMeta/Software Heritage interoperability for projects that want it.
4. **Engage non-EU players** — Canada's Open Resource Exchange, U.S. DSACMS, Brazilian government programs — to adopt publiccode.yml with country extensions.
5. **Build the conformance test suite** for the Credit Registry API and Usage Registry API — critical for scaling beyond hand-coordinated implementations (risk T3).
6. **Develop auto-generation tooling** that populates publiccode.yml from package.json, pyproject.toml, Cargo.toml etc. — reducing maintainer burden (risk A3) and making adoption viable for the long tail of projects.

### Deliverable

EU OSS Catalogue consuming the full extension set. At least one non-EU country adopting publiccode.yml. Conformance test suites published. Auto-generation tooling available for major ecosystems.

---

## Additional Key Players Needed

Players not yet in the coalition but necessary for the proposal to succeed. (Players with existing relationships are listed in [Confirmed or Likely Allies](#confirmed-or-likely-allies) above.)

### Essential (without them, the proposal stalls)

| Player                                   | Why they're essential                                                                                                        | How to engage                                                                                                   |
| ---------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| **Developers Italia**                    | Co-creators of publiccode.yml. Governance legitimacy. Operate the Italian catalog and can become a usage registry.           | Through ZenDiS/EU collaboration channels.                                                                       |
| **EU Commission / Interoperable Europe** | Operate the EU OSS Catalogue (640+ projects). Their adoption of extensions is the forcing function for the entire ecosystem. | Through ZenDiS and Developers Italia who already feed the EU catalogue. Present at Interoperable Europe events. |

### Highly Valuable (accelerate success significantly)

| Player                         | Why they matter                                                                                                                | How to engage                                                                                    |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------ |
| **OpenSSF**                    | Scorecard integration in `supplyChain`. Credibility on supply chain security.                                                  | Through Sovereign Tech Agency. Present the supply chain extension as amplifying Scorecard reach.  |
| **Foundation for Public Code** | They maintain one of the two publiccode.yml GitHub orgs. Governance clarification depends on understanding their role.         | May overlap with publiccode.yml maintainer acquaintances.                                         |

### Important for Global Scale (Phase 5)

| Player                                                        | Why they matter                                                                                                   | How to engage                                             |
| ------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------- |
| **DSACMS (U.S.)**                                             | gov-codejson could adopt publiccode.yml with a U.S. country extension instead of maintaining a parallel standard. | Through Sovereign Tech Agency or CISA contacts.           |
| **Canada Open Resource Exchange**                             | Already discussed adopting publiccode.yml. English-speaking government OSS community.                             | Direct outreach — they're already interested.             |
| **Software Heritage**                                         | Academic/archival legitimacy. YAML-LD/CodeMeta bridge.                                                            | Through the linked data extension (Phase 5).              |
| **FSFE**                                                      | REUSE integration in `supplyChain`. European OSS advocacy network.                                                | Through ZenDiS (REUSE is already in openCode.de badges). |
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

Each phase produces tangible, demonstrable results that justify the next phase to funders and stakeholders. No phase depends on solving the hardest problems first.
