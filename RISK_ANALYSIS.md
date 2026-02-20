# Risk Analysis

An examination of significant risks to the [proposal](PROPOSAL.md) for an integrated ecosystem of standards, registries, and policy frameworks built around publiccode.yml. The proposal includes publiccode.yml improvements, companion registries (credit, usage), standardized APIs, decentralized discovery mechanisms, and policy frameworks for procurement. Each risk is assessed for likelihood (probability it will occur) and impact (consequences if it does), with proposed mitigations.

We've organized risks into seven categories: adoption, technical, security/trust, governance, political, economic, and external dependencies.

---

## Adoption Risks

### A1. Branding Problem: "publiccode" Deters Non-Government Projects

The name signals "government software." Mainstream open source projects (React, Django, curl) may not see themselves reflected in this standard, limiting ecosystem adoption to public-sector-specific software rather than reaching the full open source commons that procurement depends on.

- **Likelihood:** High — the project analysis in RESEARCH.md already identifies this as a weakness.
- **Impact:** High — without broad adoption beyond government, procurement catalogs can only index a limited set of available software.
- **Mitigations:**

1. A name change or neutral alias is unlikely to happen — the publiccode.yml maintainers view the "public money → public code → publiccode.yml" framing as core to the project's identity and scope. The risk is real but not addressable by renaming.
2. Focus instead on **careful communication**: consistently present the standard as valuable to any open source project that wants visibility with public sector buyers, not just projects built exclusively for government use.
3. Make public-sector-specific fields optional while keeping the core useful for any project. The improvements proposed here (faceted classification, supply chain references, credit registries) benefit any open source project regardless of sector.
4. Engage large open source foundations (Apache, Eclipse, Linux Foundation) through the value proposition of the improvements themselves — not through a rebranding exercise.

### A2. Chicken-and-egg problem for registries

Credit and usage registries need projects to have publiccode.yml files to index. Projects need registries to exist before the `creditRegistries` field is useful. Neither side moves first.

- **Likelihood:** High — classic platform bootstrapping problem
- **Impact:** High — the entire registry architecture remains theoretical without critical mass
- **Mitigation:** Bootstrap with existing infrastructure. Drupal already has a credit system; ecosyste.ms already tracks contributions. The proposal should demonstrate the improvements working with these existing systems rather than requiring greenfield registries. openCode.de and Developers Italia can serve as initial usage registries.

### A5. Temporal fields cause widespread file staleness

Across the thousands of publiccode.yml files indexed by ecosyste.ms, most are out of date because nobody remembers to update version numbers, release dates, and dependency constraints between releases. Stale files erode trust in the entire metadata format — if a crawler encounters an outdated `softwareVersion`, it has no way to know whether the `maintenance` or `legal` sections are also outdated. Additionally, temporal fields that frequently go stale frustrate maintainers who want to keep their publiccode.yml accurate but find themselves unable to sustain the update discipline across releases.

- **Likelihood:** High — empirically observed at scale by ecosyste.ms across existing publiccode.yml deployments
- **Impact:** High — stale data undermines the credibility of the entire metadata ecosystem; procurement officers who encounter outdated fields lose trust in the file, and maintainers who notice their own files are stale may disengage from the standard entirely
- **Mitigation:** Improvement 5 proposes deprecating temporal fields (`softwareVersion`, `releaseDate`, `dependsOn[].versionMin`, and `maintenance.contractors[].until`) — keeping only slow-changing, human-authored data in the file. Forge APIs and package registries are the authoritative source for release data; the SBOM referenced in `supplyChain` captures dependency version constraints.

---

## Technical Risks

### T1. Vocabulary Governance Becomes a Bottleneck

Controlled vocabularies for `domain`, `function`, `role`, `layer`, and `audience` must evolve as new software categories emerge. A governance process that is too slow or too contentious will stall the usefulness of the classification system.

- **Likelihood:** Medium — any controlled vocabulary faces tension between stability and expressiveness.
- **Impact:** High — projects that can't classify themselves accurately will either misclassify or choose not to adopt.
- **Mitigations:**

1. Use a tiered approach: the `tags` field allows immediate free-form experimentation with no approval needed.
2. Promote tags to controlled vocabulary categories when they gain community traction (e.g., used by 10+ projects) via a transparent, low-friction PR process.
3. Model this approach on how the OSS Taxonomy currently handles vocabulary evolution.

### T3. Registry API Interoperability Failures

The Credit Registry API and Usage Registry API are outlined but not fully specified. Different implementations will interpret the spec differently, causing data integration failures: different semantics for total credits, incompatible page navigation, mismatched trust model definitions, or unexpected field formats.

- **Likelihood:** High — API interoperability across independent implementers is notoriously difficult.
- **Impact:** High — if crawlers can't reliably aggregate data from multiple registries, the entire federated architecture fails.
- **Mitigation:** Publish a conformance test suite that registry implementors can run against their API. Define a minimal viable response (required fields vs. optional) and mandate a common interpretation for core fields. Start with a reference implementation.

---

## Security and Trust Risks

### S1. Credit gaming and contribution inflation

If credit registries influence procurement decisions, there is a strong economic incentive to inflate contribution counts. A vendor could generate trivial contributions (typo fixes, whitespace commits) to climb credit rankings. This risk is about the **integrity of the data itself** — whether the numbers in a credit registry can be trusted. (See P3 for the related but distinct risk of how procurement frameworks _interpret_ that data.)

- **Likelihood:** High — any system that ties economic value to measurable contributions gets gamed (Goodhart's Law)
- **Impact:** High — undermines the credibility of the entire credit system, which is central to the proposal's value for procurement
- **Mitigation:** The proposal correctly separates credit _tracking_ (registry-specific) from credit _reporting_ (standardized API). Credit systems should require maintainer or project-level sign-off on contributions (as Drupal's issue credit system does) to prevent unilateral inflation. The `trustModel` field lets consumers weight registries by verification rigor. Procurement officers should be trained to interpret credit data critically, not treat it as a score.

### S2. Sybil attacks on usage registries

An organization (or vendor promoting its product) could create fake adopter declarations in `self-reported` usage registries to inflate adoption numbers, making software appear more widely used than it is.

- **Likelihood:** Medium — depends on the trust model; `verified-domain` registries are harder to game
- **Impact:** High — inflated adoption data misleads procurement decisions and funding allocation
- **Mitigation:** The trust model taxonomy (`verified-domain`, `signed-attestation`, `self-reported`) lets consumers filter by verification level. Catalogs should prominently display the trust model alongside adoption data. High-stakes decisions (funding allocation, procurement) should require `verified-domain` or `signed-attestation` data. For public procurement specifically, catalogs could restrict to declarations from known government domains within the relevant jurisdiction — an allowlist approach that sidesteps the Sybil problem entirely for the highest-stakes use case.

### S3. Privacy and security exposure in usage declarations

Usage registries and `.well-known/publiccode-usage.json` files expose which organizations use which software. This information could theoretically be exploited for targeted attacks or competitive intelligence.

- **Likelihood:** Low-Medium — technology detection services like BuiltWith already expose comparable or greater detail for public-facing web applications. The incremental attack surface from a standardized usage declaration (which deliberately excludes version numbers, deployment architecture, and infrastructure details) is modest. The data is also partially public via job postings, conference talks, and procurement records.
- **Impact:** Medium — primarily affects organizations with strict security postures (defense, critical infrastructure)
- **Mitigation:** The `.well-known/publiccode-usage.json` schema excludes version numbers and infrastructure details by design — it answers "do you use this project?" not "how is it deployed?" Usage registries should allow organizations to control the granularity of their declarations. The `scope` field in adopter responses should be optional. Organizations dealing with sensitive infrastructure may choose not to participate.

---

## Governance Risks

### G1. Small spec governance community relative to institutional adoption

The publiccode.yml spec is maintained by the [publiccodeyml](https://github.com/publiccodeyml/publiccode.yml) GitHub organization with a small maintainer community. The proposed improvements represent a significant scope increase that the current maintainer community may lack capacity or appetite to absorb.

- **Likelihood:** High — the spec's institutional adoption (EU mandate, 640+ projects) outpaces its governance community size
- **Impact:** High — without maintainer buy-in, the improvements remain a proposal indefinitely
- **Mitigation:** The proposal must be brought to the publiccodeyml maintainers with a clear champion and institutional backing (working group). Offering to co-maintain the improvements and providing implementation support (tooling, migration scripts, validator updates) reduces the burden on existing maintainers. Note: the governance model is currently a do-ocracy. Furthermore, every change to the spec must also be reflected in the tooling ecosystem (f.e. [issue #256](https://github.com/publiccodeyml/publiccode.yml/issues/256)), so PRs must come bundled with tooling support to be viable.

---

## Political Risks

### P4. Procurement policy momentum stalls

The proposal's value proposition depends heavily on procurement policies that actually favor open source and evaluate vendors by upstream contributions. If APELL's proposals stall, EuroStack's "Buy Open Source Act" fails to advance, and EMBAG remains an isolated Swiss example, the credit registry and classification infrastructure serves a much smaller audience — enthusiasts rather than a mandated market.

- **Likelihood:** Medium — procurement reform is slow and politically contingent. Current momentum is real (EMBAG enacted, PMPC campaign with 31,000+ signatures, EuroStack's 260+ company coalition, Cyber Resilience Act's supply chain requirements), but translating advocacy into binding procurement criteria has historically taken years.
- **Impact:** High — without policy demand, credit registries lack their primary use case and the economic incentive for vendors to participate collapses. The proposal becomes incremental rather than transformative.
- **Mitigation:** The infrastructure should deliver value independent of mandates. Credit data helps projects attract contributors and funding even without procurement policy. Classification improves discovery for any catalog user. Supply chain references serve Cyber Resilience Act compliance regardless of procurement reform. But the proposal should be honest that the legislative track is what makes the credit system economically viable at scale.

### P3. Credit system creates perverse procurement incentives

Even if credit data is accurate (see S1 for data integrity), procurement regulations may misuse it. If regulations require or favor vendors with high credit scores, vendors may optimize for credit metrics rather than actual software quality. Procurement becomes a game of credit accumulation rather than demonstrated capability. This risk is about the **policy interpretation layer** — how procurement frameworks consume credit data — distinct from S1's concern about whether the underlying data can be trusted.

- **Likelihood:** Medium — regulations tend to simplify complex signals into checkbox requirements. However, Drupal's credit system has operated for years with procurement-relevant credits without devolving into pure gaming — its process (maintainer sign-off, usage-weighted credits, organizational attribution, tiered marketplace) is sophisticated enough to resist it. The fact that this process is open source means other projects can adopt proven anti-gaming measures rather than designing from scratch.
- **Impact:** High — distorts the open source contribution ecosystem and may disadvantage smaller vendors who do high-quality but less frequent work
- **Mitigation:** Credit data should be presented as one input among many in procurement evaluation, not as a scoring system. The proposal should explicitly warn against reducing credit data to single numeric scores in procurement frameworks. Registries should expose contribution type (code, security, documentation) to allow qualitative assessment rather than simple numeric ranking.

---

## Economic Risks

### E1. Registry operation costs have no revenue model

Credit and usage registries require hosting, maintenance, moderation, and API infrastructure. The proposal doesn't address who pays for this.

- **Likelihood:** High — sustainability is the perennial problem for open source infrastructure
- **Impact:** High — registries that aren't funded will degrade or shut down, breaking the ecosystem
- **Mitigation:** Government-funded registries (openCode.de, Developers Italia) have institutional backing. Private registries (Drupal Marketplace) have commercial models. The proposal should acknowledge that registry sustainability is out of scope but critical, and recommend that funding bodies (Sovereign Tech Fund, CISA) include registry infrastructure in their investment portfolios.

---

## Risk Summary Matrix

| ID  | Risk                                                  | Likelihood | Impact | Category       |
| --- | ----------------------------------------------------- | ---------- | ------ | -------------- |
| A1  | "publiccode" branding deters non-government projects  | High       | High   | Adoption       |
| A2  | Chicken-and-egg problem for registries                | High       | High   | Adoption       |
| A5  | Temporal fields cause widespread file staleness       | High       | High   | Adoption       |
| T1  | Vocabulary governance bottleneck                      | Medium     | High   | Technical      |
| T3  | Registry API interoperability failures                | High       | High   | Technical      |
| S1  | Credit gaming and contribution inflation              | High       | High   | Security/Trust |
| S2  | Sybil attacks on usage registries                     | Medium     | High   | Security/Trust |
| S3  | Privacy and security exposure in usage declarations   | Low-Medium | Medium | Security/Trust |
| G1  | Small spec governance community                       | High       | High   | Governance     |
| P4  | Procurement policy momentum stalls                    | Medium     | High   | Political      |
| P3  | Credit system creates perverse procurement incentives | Medium     | High   | Political      |
| E1  | Registry operation costs have no revenue model        | High       | High   | Economic       |

### Critical risks (High likelihood + High impact)

1. **G1 — Small spec governance community**: The single highest-priority prerequisite. The publiccode.yml spec's maintainer community is small relative to its institutional adoption. Without maintainer buy-in and capacity, the improvements remain a proposal indefinitely.
2. **A1 — Branding** and **A2 — Bootstrapping**: The proposal's reach depends on careful communication (a name change is off the table) and solving the platform bootstrapping problem (registries need projects need registries).
3. **A5 — Temporal field staleness**: Empirically observed at scale — most existing publiccode.yml files are out of date because temporal fields aren't maintained between releases. This directly undermines ecosystem credibility and is addressed by Improvement 5.
4. **T3 — API interoperability**: The federated registry architecture is the proposal's most ambitious element. Without a conformance test suite and reference implementation, interoperability will be aspirational.
5. **S1 — Credit gaming**: The economic incentives created by linking contributions to procurement decisions will attract gaming. This is not solvable at the schema level — it requires registry-level countermeasures and procurement officer education.
6. **E1 — Registry sustainability**: The architecture depends on registries existing and operating reliably. Without a sustainability model, the long-term viability of the ecosystem is uncertain.
