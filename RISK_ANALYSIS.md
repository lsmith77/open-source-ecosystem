# Risk Analysis

A focused examination of the highest-impact risks to the [proposal](PROPOSAL.md) for publiccode.yml v1.0 extensions. Each risk includes an assessment of likelihood and impact, plus potential mitigations.

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

### A5. Temporal fields cause widespread file staleness

Across the thousands of publiccode.yml files indexed by ecosyste.ms, most are out of date because nobody remembers to update version numbers, release dates, and dependency constraints between releases. Stale files erode trust in the entire metadata format — if a crawler encounters an outdated `softwareVersion`, it has no way to know whether the `maintenance` or `legal` sections are also outdated.

- **Likelihood:** High — empirically observed at scale by ecosyste.ms across existing publiccode.yml deployments
- **Impact:** High — stale data undermines the credibility of the entire metadata ecosystem; procurement officers who encounter outdated fields lose trust in the file
- **Mitigation:** Extension 5 proposes deprecating temporal fields (`softwareVersion`, `releaseDate`, `dependsOn[].versionMin`) — keeping only slow-changing, human-authored data in the file. Forge APIs and package registries are the authoritative source for release data; the SBOM referenced in `supplyChain` captures dependency version constraints.

---

## Technical Risks

### T1. Vocabulary governance becomes a bottleneck

The controlled vocabularies for `domain`, `function`, `role`, `layer`, and `audience` must evolve as new software categories emerge. A slow or contentious governance process stalls the usefulness of the classification system.

- **Likelihood:** Medium — any controlled vocabulary faces this tension between stability and expressiveness
- **Impact:** High — projects that can't classify themselves accurately will either misclassify or not adopt
- **Mitigation:** Use a tiered approach. The `tags` field allows immediate free-form experimentation. Tags that gain traction (used by N+ projects) are promoted to controlled vocabularies via a transparent, low-friction PR process — modeled on OSS Taxonomy's current approach.

### T3. Registry API interoperability failures

The Credit Registry API and Usage Registry API are "rough outlines." Different implementations will interpret the spec differently, producing data that appears compatible but isn't — different semantics for `totalCredits`, different `trustModel` definitions, incompatible pagination.

- **Likelihood:** High — API interoperability across independent implementors is notoriously difficult
- **Impact:** High — if crawlers can't reliably aggregate data from multiple registries, the federated architecture fails
- **Mitigation:** Publish a conformance test suite that registry implementors can run against their API. Define a minimal viable response (required fields vs. optional) and mandate a common interpretation for core fields. Start with a reference implementation.

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

---

## Governance Risks

### G1. Small spec governance community relative to institutional adoption

The publiccode.yml spec is maintained by the [publiccodeyml](https://github.com/publiccodeyml/publiccode.yml) GitHub organization with a small maintainer community. The proposed extensions represent a significant scope increase that the current maintainer community may lack capacity or appetite to absorb.

- **Likelihood:** High — the spec's institutional adoption (EU mandate, 640+ projects) outpaces its governance community size
- **Impact:** High — without maintainer buy-in, the extensions remain a proposal indefinitely
- **Mitigation:** The proposal must be brought to the publiccodeyml maintainers with a clear champion and institutional backing (working group). Offering to co-maintain the extensions and providing implementation support (tooling, migration scripts, validator updates) reduces the burden on existing maintainers.

---

## Political Risks

### P4. Procurement policy momentum stalls

The proposal's value proposition depends heavily on procurement policies that actually favor open source and evaluate vendors by upstream contributions. If APELL's proposals stall, EuroStack's "Buy Open Source Act" fails to advance, and EMBAG remains an isolated Swiss example, the credit registry and classification infrastructure serves a much smaller audience — enthusiasts rather than a mandated market.

- **Likelihood:** Medium — procurement reform is slow and politically contingent. Current momentum is real (EMBAG enacted, PMPC campaign with 31,000+ signatures, EuroStack's 260+ company coalition, Cyber Resilience Act's supply chain requirements), but translating advocacy into binding procurement criteria has historically taken years.
- **Impact:** High — without policy demand, credit registries lack their primary use case and the economic incentive for vendors to participate collapses. The proposal becomes incremental rather than transformative.
- **Mitigation:** The infrastructure should deliver value independent of mandates. Credit data helps projects attract contributors and funding even without procurement policy. Classification improves discovery for any catalog user. Supply chain references serve Cyber Resilience Act compliance regardless of procurement reform. But the proposal should be honest that the legislative track is what makes the credit system economically viable at scale.

### P3. Credit system creates perverse procurement incentives

If procurement regulations begin to require or favor vendors with high credit scores, vendors may optimize for credit metrics rather than actual software quality. Procurement becomes a game of credit accumulation rather than demonstrated capability.

- **Likelihood:** Medium — regulations tend to simplify complex signals into checkbox requirements. However, Drupal's credit system has operated for years with procurement-relevant credits without devolving into pure gaming — its process (maintainer sign-off, usage-weighted credits, organizational attribution, tiered marketplace) is sophisticated enough to resist it. The fact that this process is open source means other projects can adopt proven anti-gaming measures rather than designing from scratch.
- **Impact:** High — distorts the open source contribution ecosystem and may disadvantage smaller vendors who do high-quality but less frequent work
- **Mitigation:** Credit data should be presented as one input among many in procurement evaluation, not as a scoring system. The proposal should explicitly warn against reducing credit data to single numeric scores in procurement frameworks. Registries should expose contribution type (code, security, documentation) to allow qualitative assessment.

---

## Economic Risks

### E1. Registry operation costs have no revenue model

Credit and usage registries require hosting, maintenance, moderation, and API infrastructure. The proposal doesn't address who pays for this.

- **Likelihood:** High — sustainability is the perennial problem for open source infrastructure
- **Impact:** High — registries that aren't funded will degrade or shut down, breaking the ecosystem
- **Mitigation:** Government-funded registries (openCode.de, Developers Italia) have institutional backing. Private registries (Drupal Marketplace) have commercial models. The proposal should acknowledge that registry sustainability is out of scope but critical, and recommend that funding bodies (Sovereign Tech Fund, CISA) include registry infrastructure in their investment portfolios.

---

## Risk Summary Matrix

| ID | Risk | Likelihood | Impact | Category |
|----|------|-----------|--------|----------|
| A1 | "publiccode" branding deters non-government projects | High | High | Adoption |
| A2 | Chicken-and-egg problem for registries | High | High | Adoption |
| A5 | Temporal fields cause widespread file staleness | High | High | Adoption |
| T1 | Vocabulary governance bottleneck | Medium | High | Technical |
| T3 | Registry API interoperability failures | High | High | Technical |
| S1 | Credit gaming and contribution inflation | High | High | Security/Trust |
| S2 | Sybil attacks on usage registries | Medium | High | Security/Trust |
| G1 | Small spec governance community | High | High | Governance |
| P4 | Procurement policy momentum stalls | Medium | High | Political |
| P3 | Credit system creates perverse procurement incentives | Medium | High | Political |
| E1 | Registry operation costs have no revenue model | High | High | Economic |

### Critical risks (High likelihood + High impact)

1. **G1 — Small spec governance community**: The single highest-priority prerequisite. The publiccode.yml spec's maintainer community is small relative to its institutional adoption. Without maintainer buy-in and capacity, the extensions remain a proposal indefinitely.
2. **A1 — Branding** and **A2 — Bootstrapping**: The proposal's reach depends on solving both the perception problem (non-government projects) and the platform bootstrapping problem (registries need projects need registries).
3. **A5 — Temporal field staleness**: Empirically observed at scale — most existing publiccode.yml files are out of date because temporal fields aren't maintained between releases. This directly undermines ecosystem credibility and is addressed by Extension 5.
4. **T3 — API interoperability**: The federated registry architecture is the proposal's most ambitious element. Without a conformance test suite and reference implementation, interoperability will be aspirational.
5. **S1 — Credit gaming**: The economic incentives created by linking contributions to procurement decisions will attract gaming. This is not solvable at the schema level — it requires registry-level countermeasures and procurement officer education.
6. **E1 — Registry sustainability**: The architecture depends on registries existing and operating reliably. Without a sustainability model, the long-term viability of the ecosystem is uncertain.
