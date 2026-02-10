# Risk Analysis

A systematic examination of risks to the [proposal](PROPOSAL.md) for publiccode.yml v1.0 extensions, organized by category. Each risk includes an assessment of likelihood and impact, plus potential mitigations.

---

## Contents

- [Adoption Risks](#adoption-risks)
- [Technical Risks](#technical-risks)
- [Security and Trust Risks](#security-and-trust-risks)
- [Governance Risks](#governance-risks)
- [Political and Jurisdictional Risks](#political-and-jurisdictional-risks)
- [Economic and Sustainability Risks](#economic-and-sustainability-risks)
- [Dependency and External Risks](#dependency-and-external-risks)
- [Risk Summary Matrix](#risk-summary-matrix)

---

## Adoption Risks

### A1. "publiccode" branding deters non-government projects

The name signals "government software." Mainstream open source projects (React, Django, curl) may not see themselves in this standard, limiting the ecosystem to public-sector-specific software rather than the full open source commons that procurement depends on.

- **Likelihood:** High — the [README](README.md) already identifies this as a weakness
- **Impact:** High — without broad OSS adoption, procurement catalogs only index a fraction of available software
- **Mitigation:** Create a neutral profile or alias (e.g., "softwarecode.yml") that maps 1:1 to publiccode.yml. Alternatively, invest in outreach demonstrating that the vendor credit system benefits any project seeking commercial adoption, not just government-facing ones.

### A2. Chicken-and-egg problem for registries

Credit and usage registries need projects to have publiccode.yml files to index. Projects need registries to exist before the `creditRegistries` field is useful. Neither side moves first.

- **Likelihood:** High — classic platform bootstrapping problem
- **Impact:** High — the entire registry architecture remains theoretical without critical mass
- **Mitigation:** Bootstrap with existing infrastructure. Drupal already has a credit system; ecosyste.ms already tracks contributions. The proposal should demonstrate the extensions working with these existing systems rather than requiring greenfield registries. openCode.de and Developers Italia can serve as initial usage registries.

### A3. Maintainer fatigue from yet another metadata file

Open source maintainers already manage package.json, pyproject.toml, CITATION.cff, .github/ config, and more. Adding or extending publiccode.yml is additional maintenance burden with unclear direct benefit to the maintainer.

- **Likelihood:** High — well-documented problem in the OSS ecosystem
- **Impact:** Medium — affects breadth of adoption; high-profile projects with dedicated teams will adopt, long-tail projects won't
- **Mitigation:** Auto-generation tooling that populates publiccode.yml from existing metadata (package.json, pyproject.toml, GitHub API). The publiccode.yml editor at openCode.de is a start, but CI-based generators that keep it in sync reduce the maintenance cost to near zero.

### A4. Migration friction from v0.x to v1.0

Existing publiccode.yml adopters (640+ in the EU OSS Catalogue) must migrate from flat `categories` to faceted `classification`. Even with backward compatibility, the dual-format period creates confusion and inconsistency across catalogs.

- **Likelihood:** Medium — backward compatibility reduces urgency to migrate, but also reduces adoption of new features
- **Impact:** Medium — catalogs must support both schemas indefinitely, increasing implementation cost
- **Mitigation:** Provide automated migration tools that convert existing `categories` and `softwareType` values to the new `classification` structure. Set a clear deprecation timeline (e.g., 2 years) for the old fields.

### A5. Faceted classification is too complex for most projects

The flat `categories` list is simple: pick one or two terms. The faceted `classification` with 5 controlled dimensions plus free-form tags requires understanding the distinction between domain, function, role, layer, and audience.

- **Likelihood:** Medium — similar multi-dimensional classifications (e.g., library catalog systems, DCAT) are routinely misapplied
- **Impact:** Medium — inconsistent classification degrades search quality, undermining the procurement discovery use case
- **Mitigation:** Provide a guided classification wizard in the publiccode.yml editor. Offer default mappings so that a project selecting "content-management" gets sensible defaults for the other dimensions. Allow minimal adoption (just `function`) as a valid starting point.

---

## Technical Risks

### T1. Vocabulary governance becomes a bottleneck

The controlled vocabularies for `domain`, `function`, `role`, `layer`, and `audience` must evolve as new software categories emerge. A slow or contentious governance process stalls the usefulness of the classification system.

- **Likelihood:** Medium — any controlled vocabulary faces this tension between stability and expressiveness
- **Impact:** High — projects that can't classify themselves accurately will either misclassify or not adopt
- **Mitigation:** Use a tiered approach. The `tags` field allows immediate free-form experimentation. Tags that gain traction (used by N+ projects) are promoted to controlled vocabularies via a transparent, low-friction PR process — modeled on OSS Taxonomy's current approach.

### T2. URL rot in supply chain references

The `supplyChain` section links to external resources (SBOM, Scorecard, security policy). These URLs break as projects move repositories, change hosting, or restructure release artifacts.

- **Likelihood:** High — URL rot is pervasive on the web; SBOM URLs tied to "latest release" patterns are particularly fragile
- **Impact:** Medium — broken links degrade trust signals but don't break the core metadata
- **Mitigation:** Recommend URL patterns that are resilient (e.g., GitHub's `/releases/latest/download/` redirects). Crawlers should report link rot back to projects. Consider supporting URL templates with variables (e.g., `{version}`) that crawlers resolve dynamically.

### T3. Registry API interoperability failures

The Credit Registry API and Usage Registry API are "rough outlines." Different implementations will interpret the spec differently, producing data that appears compatible but isn't — different semantics for `totalCredits`, different `trustModel` definitions, incompatible pagination.

- **Likelihood:** High — API interoperability across independent implementors is notoriously difficult
- **Impact:** High — if crawlers can't reliably aggregate data from multiple registries, the federated architecture fails
- **Mitigation:** Publish a conformance test suite that registry implementors can run against their API. Define a minimal viable response (required fields vs. optional) and mandate a common interpretation for core fields. Start with a reference implementation.

### T4. YAML-LD adds complexity without adoption

Extension 5 (YAML-LD) adds a linked data layer that most maintainers won't use and most crawlers won't need. The `@context` block may confuse maintainers, and the YAML-LD W3C spec is a Community Group report, not a full Recommendation.

- **Likelihood:** Medium — the "optional" framing reduces risk, but the mere presence of the option adds cognitive load to the spec
- **Impact:** Low — since it's optional, it mainly affects the specification's perceived complexity rather than practical adoption
- **Mitigation:** Keep YAML-LD in a separate appendix or companion document rather than as a numbered extension. Make clear that crawlers produce the linked data representation at indexing time — projects never need to write it.

### T5. Schema complexity creep over time

The proposal adds 3 new top-level sections (`classification`, `supplyChain`, `creditRegistries`) and deprecates/modifies 3 existing fields. Future extensions could compound this, making publiccode.yml unwieldy.

- **Likelihood:** Medium — natural tendency for standards to accrete features
- **Impact:** Medium — complexity discourages adoption and increases the error surface
- **Mitigation:** Establish a design principle: publiccode.yml points to external data, it doesn't embed it. Each new field must justify why it belongs in the project's metadata file rather than in an external registry or toolchain.

---

## Security and Trust Risks

### S1. Credit gaming and contribution inflation

If credit registries influence procurement decisions, there is a strong economic incentive to inflate contribution counts. A vendor could generate trivial contributions (typo fixes, whitespace commits) to climb credit rankings.

- **Likelihood:** High — any system that ties economic value to measurable contributions gets gamed (Goodhart's Law)
- **Impact:** High — undermines the credibility of the entire credit system, which is central to the proposal's value for procurement
- **Mitigation:** The proposal correctly separates credit *tracking* (registry-specific) from credit *reporting* (standardized API). Registries must implement their own anti-gaming measures (e.g., Drupal's issue credit system requires maintainer sign-off). The `trustModel` field lets consumers weight registries by verification rigor. Procurement officers should be trained to interpret credit data critically, not treat it as a score.

### S2. Sybil attacks on usage registries

An organization (or vendor promoting its product) could create fake adopter declarations in `self-reported` usage registries to inflate adoption numbers, making software appear more widely used than it is.

- **Likelihood:** Medium — depends on the trust model; `verified-domain` registries are harder to game
- **Impact:** High — inflated adoption data misleads procurement decisions and funding allocation
- **Mitigation:** The trust model taxonomy (`verified-domain`, `signed-attestation`, `self-reported`) lets consumers filter by verification level. Catalogs should prominently display the trust model alongside adoption data. High-stakes decisions (funding allocation, procurement) should require `verified-domain` or `signed-attestation` data.

### S3. Malicious supply chain metadata

A compromised project could list a malicious SBOM URL, scorecard link, or security policy that serves misleading information — e.g., an SBOM that hides known-vulnerable dependencies, or a scorecard URL that redirects to a fake results page.

- **Likelihood:** Low — requires compromising the project's repository (at which point the attacker has broader access)
- **Impact:** High — procurement officers relying on these links may adopt software with hidden vulnerabilities
- **Mitigation:** Crawlers should verify that `scorecard` URLs point to the canonical scorecard.dev domain. SBOM integrity can be validated against package registry data. The `securityPolicy` link should be verified as pointing to the same domain as the project `url`. These checks should be implemented in validator tooling, not left to human reviewers.

### S4. Project endorsement of unreliable credit registries

A project could list a credit registry in `creditRegistries` that misrepresents contribution data — either a registry operated by the project itself (self-dealing) or one with lax verification.

- **Likelihood:** Medium — project endorsement is a trust signal, and trust signals can be abused
- **Impact:** Medium — affects the credibility of specific projects, not the system as a whole
- **Mitigation:** Catalogs should cross-reference credit data from multiple registries. The central registry directory could include community-maintained trust assessments of registries. Over time, a reputation system for registries themselves may emerge.

### S5. Privacy risks in usage declarations

Usage registries expose which organizations use which software. This information could be exploited for targeted attacks (knowing a municipality runs a specific CMS version reveals its attack surface) or competitive intelligence.

- **Likelihood:** Medium — usage data is already partially public (job postings, conference talks), but systematic aggregation increases exposure
- **Impact:** Medium — primarily affects organizations with strict security postures (defense, critical infrastructure)
- **Mitigation:** Usage registries should allow organizations to control the granularity of their declarations (e.g., declaring software use without version or scale). The `scope` field in adopter responses should be optional. Organizations dealing with sensitive infrastructure may choose not to participate.

---

## Governance Risks

### G1. Small spec governance community relative to institutional adoption

The publiccode.yml spec is maintained by the [publiccodeyml](https://github.com/publiccodeyml/publiccode.yml) GitHub organization with a small maintainer community. The separately-governed [Foundation for Public Code](https://publiccode.net/) (a Dutch foundation) maintains [The Standard for Public Code](https://standard.publiccode.net/) — a broader framework that recommends publiccode.yml as a tool but does not control the spec itself. The proposed extensions represent a significant scope increase that the current maintainer community may lack capacity or appetite to absorb.

- **Likelihood:** High — the spec's institutional adoption (EU mandate, 640+ projects) outpaces its governance community size
- **Impact:** High — without maintainer buy-in, the extensions remain a proposal indefinitely
- **Mitigation:** The proposal must be brought to the publiccodeyml maintainers with a clear champion and institutional backing (working group). Offering to co-maintain the extensions and providing implementation support (tooling, migration scripts, validator updates) reduces the burden on existing maintainers. The Foundation for Public Code is a potential amplifier — their Standard for Public Code could reference the extensions — but they do not control the spec.

### G2. Central registry directory becomes a gatekeeper

The "registry of registries" (bootstrap list maintained via pull requests) introduces a centralized control point. Whoever maintains this list has de facto power over which registries are discoverable.

- **Likelihood:** Medium — the browser CA model this mirrors has faced similar criticism
- **Impact:** Medium — a biased directory could exclude registries from certain jurisdictions or sectors
- **Mitigation:** Keep the directory as a convenience, not a requirement. The well-known URL mechanism (`/.well-known/publiccode-registry.json`) allows fully decentralized discovery. The DNS TXT record mechanism (mentioned as future work) provides a second decentralized path. The central directory should have a published inclusion policy with objective criteria.

### G3. EU-centric governance excludes global community

publiccode.yml's institutional backing is European (Italy, Germany, EU Commission). Non-EU countries and communities may feel excluded from governance decisions or see the standard as imposing European regulatory priorities.

- **Likelihood:** Medium — the standard's current adoption is almost entirely European
- **Impact:** High — a truly global open source index requires global participation; EU-only adoption limits the proposal's ambition
- **Mitigation:** Actively engage non-EU initiatives (Canada's Open Resource Exchange, U.S. DSACMS, Brazilian government open source programs). The country extension mechanism already supports jurisdiction-specific additions. Governance should include representatives from outside the EU.

---

## Political and Jurisdictional Risks

### P1. Digital sovereignty framing creates fragmentation

Different countries and blocs define "digital sovereignty" differently. The EU emphasizes data protection and industrial policy; the U.S. emphasizes supply chain security; India and Brazil emphasize technological autonomy. A standard perceived as serving one sovereignty agenda may face resistance from others.

- **Likelihood:** Medium — sovereignty is inherently political, and standards embedded in sovereignty narratives inherit the politics
- **Impact:** High — fragmentation into regional standards (EU vs. U.S. vs. BRICS) defeats the goal of a global index
- **Mitigation:** Frame the standard around universal needs (procurement transparency, supply chain security, interoperability) rather than any specific sovereignty doctrine. The technical benefits should stand independent of political framing.

### P2. Usage registries become surveillance infrastructure

Systematic tracking of which organizations use which software, aggregated across registries, creates a map of institutional technology dependencies. This data could be compelled by governments for strategic purposes or leaked in a breach.

- **Likelihood:** Low — the data is somewhat sensitive but not classified
- **Impact:** High — could chill participation, especially from organizations in adversarial jurisdictions or those deploying security-sensitive software
- **Mitigation:** Usage declarations should always be voluntary. Registries should support organization-controlled visibility (public, restricted, or private declarations). The architecture should not enable a single entity to build a comprehensive global picture without each registry's cooperation.

### P3. Credit system creates perverse procurement incentives

If procurement regulations begin to require or favor vendors with high credit scores, vendors may optimize for credit metrics rather than actual software quality. Procurement becomes a game of credit accumulation rather than demonstrated capability.

- **Likelihood:** Medium — regulations tend to simplify complex signals into checkbox requirements
- **Impact:** High — distorts the open source contribution ecosystem and may disadvantage smaller vendors who do high-quality but less frequent work
- **Mitigation:** Credit data should be presented as one input among many in procurement evaluation, not as a scoring system. The proposal should explicitly warn against reducing credit data to single numeric scores in procurement frameworks. Registries should expose contribution type (code, security, documentation) to allow qualitative assessment.

---

## Economic and Sustainability Risks

### E1. Registry operation costs have no revenue model

Credit and usage registries require hosting, maintenance, moderation, and API infrastructure. The proposal doesn't address who pays for this.

- **Likelihood:** High — sustainability is the perennial problem for open source infrastructure
- **Impact:** High — registries that aren't funded will degrade or shut down, breaking the ecosystem
- **Mitigation:** Government-funded registries (openCode.de, Developers Italia) have institutional backing. Private registries (Drupal Marketplace) have commercial models. The proposal should acknowledge that registry sustainability is out of scope but critical, and recommend that funding bodies (Sovereign Tech Fund, CISA) include registry infrastructure in their investment portfolios.

### E2. Credit system advantages large vendors over small ones

Large vendors with dedicated open source program offices can systematically generate contributions across many projects. Small vendors doing deep, specialized work on fewer projects may appear less prominent despite higher per-project impact.

- **Likelihood:** High — resource asymmetry is structural
- **Impact:** Medium — partially defeats the stated goal of "leveling the playing field" from [PITCH.md](PITCH.md)
- **Mitigation:** Credit APIs include `roles` (code, security, documentation, maintainer) and time-windowed queries. Procurement officers can filter by contribution type and recency. Registries should be encouraged to report intensity metrics (contributions per project) alongside breadth metrics (number of projects). The Drupal tier system (platinum/gold/silver) at least provides relative ranking within a project.

### E3. Procurement offices lack capacity to consume the metadata

The proposal assumes procurement officers will query catalogs for faceted classifications, review scorecard results, check vendor credits, and verify usage data. Many procurement offices — especially at the municipal level — lack the technical sophistication or staffing for this.

- **Likelihood:** High — procurement capacity varies enormously across organizations and jurisdictions
- **Impact:** Medium — the data exists but doesn't influence decisions; adoption effort is wasted
- **Mitigation:** The value chain requires intermediaries: catalog UIs (openCode.de, EU OSS Catalogue) that present the data as simple badges, comparison tables, and recommendations rather than raw API queries. Training programs (like the [U.S. DITAP curriculum](https://github.com/usds/ditap-curriculum-update)) should incorporate open source metadata literacy.

### E4. Free-rider problem in usage declarations

Organizations benefit from seeing what peers deploy but may not declare their own usage — especially if declaration requires effort or exposes strategic information.

- **Likelihood:** High — classic collective action problem
- **Impact:** Medium — sparse usage data limits the value of reuse signals and sovereignty assessments
- **Mitigation:** Tie declaration to benefits: openCode.de's reuse badge system already creates a positive feedback loop where declared usage leads to higher project visibility. Policy mandates (like Italy's requirement for public administrations to evaluate open source) could extend to requiring usage declarations.

---

## Dependency and External Risks

### D1. OpenSSF Scorecard availability and continuity

The `supplyChain.scorecard` field depends on the OpenSSF Scorecard infrastructure continuing to operate and maintain its API. OpenSSF is funded by industry contributions that could shift.

- **Likelihood:** Low — OpenSSF has broad industry backing (Google, Microsoft, GitHub)
- **Impact:** Medium — scorecard links become dead references; procurement loses a key security signal
- **Mitigation:** The field is a URL, not a score. If Scorecard's API changes, only the URL pattern needs updating. The schema could support alternative security scoring systems by making the field more generic (e.g., `securityScoring` with a `provider` subfield).

### D2. ecosyste.ms as single-maintainer infrastructure

ecosyste.ms is the largest open dataset of OSS metadata and a natural credit/funding registry. It is maintained primarily by one person (Andrew Nesbitt). The proposal references it as a credit registry example.

- **Likelihood:** Medium — single-maintainer risk is real but Nesbitt has institutional support via Sovereign Tech Fund
- **Impact:** Medium — loss of ecosyste.ms would remove a key implementation partner but not break the architecture (multiple registries by design)
- **Mitigation:** The federated architecture means no single registry is critical. The proposal should avoid creating implicit dependence on any one registry. Encouraging multiple implementations of the Credit Registry API reduces single-point-of-failure risk.

### D3. YAML-LD specification stalls

YAML-LD is a W3C Community Group Final Report, not a W3C Recommendation. Community Group reports can be abandoned if the group loses interest.

- **Likelihood:** Medium — W3C Community Groups have variable longevity
- **Impact:** Low — Extension 5 is explicitly optional, and crawlers can produce linked data from plain YAML using a canonical context
- **Mitigation:** The proposal already treats YAML-LD as optional. If the spec stalls, the fallback is that crawlers apply a server-side context mapping. The linked data interoperability goal is achieved either way.

### D4. GitHub-centric assumptions

The proposal's examples and URL patterns assume GitHub hosting (e.g., `/releases/latest/download/`, `/security/policy`). Projects on GitLab, Gitea, Codeberg, or self-hosted forges may not have equivalent URL patterns.

- **Likelihood:** Medium — openCode.de itself runs on GitLab
- **Impact:** Medium — forge-specific URL patterns create friction for non-GitHub projects
- **Mitigation:** Define SBOM and security policy references as generic URLs, not forge-specific patterns. Provide examples for multiple forges. Validator tooling should accept any URL, not just GitHub patterns.

---

## Risk Summary Matrix

| ID | Risk | Likelihood | Impact | Category |
|----|------|-----------|--------|----------|
| A1 | "publiccode" branding deters non-government projects | High | High | Adoption |
| A2 | Chicken-and-egg problem for registries | High | High | Adoption |
| A3 | Maintainer fatigue from another metadata file | High | Medium | Adoption |
| A4 | Migration friction from v0.x to v1.0 | Medium | Medium | Adoption |
| A5 | Faceted classification too complex | Medium | Medium | Adoption |
| T1 | Vocabulary governance bottleneck | Medium | High | Technical |
| T2 | URL rot in supply chain references | High | Medium | Technical |
| T3 | Registry API interoperability failures | High | High | Technical |
| T4 | YAML-LD complexity without adoption | Medium | Low | Technical |
| T5 | Schema complexity creep | Medium | Medium | Technical |
| S1 | Credit gaming and contribution inflation | High | High | Security/Trust |
| S2 | Sybil attacks on usage registries | Medium | High | Security/Trust |
| S3 | Malicious supply chain metadata | Low | High | Security/Trust |
| S4 | Endorsement of unreliable credit registries | Medium | Medium | Security/Trust |
| S5 | Privacy risks in usage declarations | Medium | Medium | Security/Trust |
| G1 | Small spec governance community | High | High | Governance |
| G2 | Central directory becomes gatekeeper | Medium | Medium | Governance |
| G3 | EU-centric governance excludes global community | Medium | High | Governance |
| P1 | Digital sovereignty framing creates fragmentation | Medium | High | Political |
| P2 | Usage registries become surveillance infrastructure | Low | High | Political |
| P3 | Credit system creates perverse procurement incentives | Medium | High | Political |
| E1 | Registry operation costs have no revenue model | High | High | Economic |
| E2 | Credit system advantages large vendors | High | Medium | Economic |
| E3 | Procurement offices lack capacity | High | Medium | Economic |
| E4 | Free-rider problem in usage declarations | High | Medium | Economic |
| D1 | OpenSSF Scorecard continuity | Low | Medium | Dependency |
| D2 | ecosyste.ms single-maintainer risk | Medium | Medium | Dependency |
| D3 | YAML-LD specification stalls | Medium | Low | Dependency |
| D4 | GitHub-centric assumptions | Medium | Medium | Dependency |

### Critical risks (High likelihood + High impact)

1. **A1 — Branding** and **A2 — Bootstrapping**: The proposal's reach depends on solving both the perception problem (non-government projects) and the platform bootstrapping problem (registries need projects need registries).
2. **T3 — API interoperability**: The federated registry architecture is the proposal's most ambitious element. Without a conformance test suite and reference implementation, interoperability will be aspirational.
3. **S1 — Credit gaming**: The economic incentives created by linking contributions to procurement decisions will attract gaming. This is not solvable at the schema level — it requires registry-level countermeasures and procurement officer education.
4. **G1 — Small spec governance community**: The publiccode.yml spec's maintainer community is small relative to its institutional adoption. The proposed extensions represent a significant scope increase. Without maintainer buy-in and capacity, the extensions remain a proposal indefinitely. This is the highest-priority prerequisite.
5. **E1 — Registry sustainability**: The architecture depends on registries existing and operating reliably. Without a sustainability model, the long-term viability of the ecosystem is uncertain.
