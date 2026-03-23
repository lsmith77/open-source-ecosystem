# EU Policy Context

This document tracks EU legislative initiatives relevant to this proposal and explains how the metadata infrastructure proposed here relates to each.

---

## Overview

| Law / Initiative | Status | Primary hook | Proposal role |
|---|---|---|---|
| [Cyber Resilience Act (CRA)](#cyber-resilience-act-cra) | In force (2024), obligations phasing in through 2027 | OSS steward obligations; supply chain security evidence | Machine-discoverable evidence layer for steward compliance |
| [NIS2 Directive](#nis2-directive) | In force, transposition deadline Oct 2024 | Supply chain risk management in 18 critical sectors | Standardized supply chain metadata reduces compliance cost |
| [Interoperable Europe Act](#interoperable-europe-act) | In force since July 2024 | OSS reuse obligation; EU OSS Catalogue | publiccode.yml is already the Catalogue onboarding standard |
| [Public Procurement Act](#public-procurement-act) | Proposal expected Q2 2026 | Non-price criteria; "Made in Europe"; SME access | Metadata infrastructure makes non-price criteria measurable |
| [EMBAG (Switzerland)](#embag-switzerland) | In force since 2024 | Open source release mandate for public software | Faceted classification + discovery make the mandate actionable |

---

## Cyber Resilience Act (CRA)

**Reference:** [digital-strategy.ec.europa.eu/en/policies/cra-open-source](https://digital-strategy.ec.europa.eu/en/policies/cra-open-source)
**Status:** Regulation (EU) 2024/2847, published October 2024. Obligations apply in phases through 2027.

### What it does

The CRA applies to products with digital elements placed on the EU market for commercial use. For open source software, it introduces the concept of the **open-source software steward** — a legal entity that:

- Implements a cybersecurity policy for secure development
- Operates a vulnerability handling and disclosure process
- Reports actively exploited vulnerabilities to authorities (ENISA/national CSIRTs)

Stewards must be able to demonstrate these commitments to regulators on request.

### How this proposal relates

The `supplyChain` fields proposed in [Improvement 2](PROPOSAL.md#improvement-2-supply-chain-references) are the natural machine-discoverable evidence layer for CRA steward obligations:

- `supplyChain.sbom` — links to the Software Bill of Materials demonstrating component transparency
- `supplyChain.securityPolicy` — links to the vulnerability disclosure process the steward operates
- `supplyChain.scorecard` — links to the OpenSSF Scorecard assessing secure development practices

A vendor with a sustained, project-endorsed contribution record in a **credit registry** (Improvement 3) provides empirical evidence of "providing sustained support" — the functional test for steward status.

The deferred [Improvement 6: CRA Steward Declaration](PROPOSAL.md#improvement-6-cra-steward-declaration-deferred) would add an explicit `crasteward` field to publiccode.yml, making steward identity machine-discoverable for regulators and deploying organizations.

**Key insight:** Without standardized metadata, demonstrating CRA compliance is an expensive, bespoke exercise for each steward. With it, regulators can query a federated registry and deploying organizations can verify compliance at procurement time.

---

## NIS2 Directive

**Reference:** [digital-strategy.ec.europa.eu/en/policies/nis2-directive](https://digital-strategy.ec.europa.eu/en/policies/nis2-directive)
**Status:** In force. Member state transposition deadline was October 2024.

### What it does

NIS2 requires organizations in 18 critical sectors — energy, health, transport, water, public administration, digital infrastructure, and others — to implement **supply chain security risk management**. This includes:

- Assessing the security posture of ICT products and services they adopt
- Managing risks introduced by direct suppliers and service providers
- Maintaining an inventory of ICT assets and their supply chain relationships

### How this proposal relates

For NIS2-regulated organizations that deploy open source software, the obligation to assess supply chain security is clear — but the mechanism is not. Without standardized metadata, assessment is expensive and inconsistent.

The proposal addresses this on two sides:

1. **At adoption time:** The `supplyChain` references (SBOM, security policy, Scorecard) allow a deploying organization to fetch structured evidence from a single metadata entry point, rather than manually hunting across project websites, GitHub repositories, and external databases.

2. **At inventory time:** The [organization-level usage declaration](PROPOSAL.md#organization-level-usage-declarations-publiccode-usagejson) (`.well-known/publiccode-usage.json`) gives organizations a standard format to maintain a declared software stack. This serves as a natural audit trail for supply chain risk management obligations and supports internal NIS2 compliance documentation.

**Key insight:** NIS2 creates legal demand for exactly the supply chain visibility this proposal provides. Registries that aggregate this data could serve as compliance infrastructure for regulated sectors.

---

## Interoperable Europe Act

**Reference:** [Regulation (EU) 2024/903](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=OJ:L_202400903)
**Status:** In force since January 2024; obligations applicable from July 2024.

### What it does

The Interoperable Europe Act requires public bodies providing cross-border digital public services to:

- Assess interoperability before procuring or developing ICT solutions
- Share reusable solutions (including software) through the EU OSS Catalogue
- Consider open source software as part of procurement decisions
- Report on their interoperability measures

It established the **Interoperable Europe Board** and a **GovTech** support mechanism to help public bodies implement these obligations. The **EU Open Source Software Catalogue** ([interoperable-europe.ec.europa.eu](https://interoperable-europe.ec.europa.eu/collection/open-source-observatory-osor/open-source-observatory)) is the primary discovery tool.

### How this proposal relates

This is the **most direct existing legislative hook** for this proposal.

**publiccode.yml is already the EU OSS Catalogue onboarding standard.** Adding a valid `publiccode.yml` file is a prerequisite for a solution to appear in the Catalogue. This proposal's improvements to publiccode.yml — faceted classification, supply chain references, credit registry discovery — directly improve the quality and usefulness of data available to public bodies fulfilling their Interoperable Europe Act obligations.

Specific connections:

- **Improvement 1 (Faceted Classification):** Better classification means public bodies can actually find solutions matching their specific sector and use case, not just keyword-match against flat categories.
- **Improvement 2 (Supply Chain References):** Security and compliance data becomes available at the point of discovery, supporting the Act's requirement to assess interoperability *before* procurement.
- **Improvement 4 (Usage Registries) + Organization-Level Declarations:** The Act requires sharing reusable solutions. Usage declarations make peer adoption visible — a public body can see which comparable organizations already run a solution, supporting the "consider reuse" obligation.
- **Registry Discovery Standard:** The `/.well-known/publiccode-registry.json` mechanism enables crawlers operated by the Interoperable Europe Portal to discover new registries automatically, without manual onboarding.

**Key insight:** The Interoperable Europe Act creates an *existing, active* legal obligation for EU public bodies to assess and share open source. This proposal improves the infrastructure that already serves this obligation. The EU OSS Catalogue team is a natural first institutional partner.

---

## Public Procurement Act

**Reference:** [EP Legislative Train](https://www.europarl.europa.eu/legislative-train/theme-a-new-plan-for-europe-s-sustainable-prosperity-and-competitiveness/file-public-procurement-act)
**Status:** Pre-legislative. Commission proposal expected Q2 2026. Public consultation closed January 2026.

### What it does (planned)

The Act will revise three existing procurement directives (2014/23/EU, 2014/24/EU, 2014/25/EU) into a consolidated framework. Stated goals:

- **"Made in Europe" preference** — allow EU-origin criteria in strategic sectors (AI, space, defense, and potentially digital)
- **Non-price criteria** — mainstream sustainability, resilience, and social criteria alongside price in award decisions
- **SME access** — improve participation of smaller European firms through simplified procedures and lot requirements
- **Simplification** — reduce administrative burden
- **Transparency** — strengthen auditability of spending decisions

### How this proposal relates

The Procurement Act does not yet mention open source software or metadata standards — but that reflects the drafting stage, not indifference. The [EuroStack coalition](https://euro-stack.com/blog/2025/3/eu-procurement-for-open-source-digital-sovereignty-final) and others are actively lobbying to include open source preference criteria. The proposal's infrastructure fits into five specific leverage points:

#### 1. Operationalizing "Made in Europe" for software

"Made in Europe" is well-defined for physical goods but contested for software. For software, what "European origin" means cannot rely on vendor self-declaration — that is trivially gameable. The **credit registry** (Improvement 3) is the only technically credible mechanism: verified, project-endorsed contribution data tied to EU-registered entities provides auditable evidence of European maintenance. A procurement office could query: "does this software have substantive contributions from EU-based entities?"

#### 2. Making non-price criteria measurable

Non-price criteria collapse back to price in practice because subjective criteria are too expensive to verify. Every improvement converts a stated criterion into a machine-verifiable signal:

| Procurement criterion | Proposal component | What it measures |
|---|---|---|
| Security posture | Supply Chain References (Improvement 2) | SBOM, OpenSSF Scorecard, vulnerability policy |
| Vendor expertise | Credit Registries (Improvement 3) | Upstream contributions, not self-reported claims |
| Community health / resilience | Usage Registries (Improvement 4) | Peer deployments, verified by domain control |
| Sector fit | Faceted Classification (Improvement 1) | Multi-dimensional classification |

#### 3. SME discoverability

SMEs with deep open source expertise are invisible to procurement offices that rely on name recognition. The credit registry inverts this: an SME with 400 upstream commits to a project and a verified maintenance role is *findable* by any procurement office running a registry query. No set-asides or quotas required — just transparent, verifiable contribution data.

#### 4. "Public Money, Public Code" enforcement

An Open Source First mandate (as proposed by EuroStack and FSFE) would be unenforceable without discovery and audit infrastructure. The **usage declaration standard** (`.well-known/publiccode-usage.json`) is the natural compliance mechanism: public bodies could be required to publish their software stack, making policy enforcement auditable. Faceted classification and supply chain references make evaluation of open source alternatives feasible for procurement officers who currently lack tools for it.

#### 5. Software reuse as a measurable criterion

Current procurement guidance already mentions "use in other administrations" as an evaluation factor, but there is no standardized way to query it. **Usage registries** answer exactly this: a procurement office can query which peer organizations already deploy a given solution, with declarations verified by domain control.

### Strategic timing

The Procurement Act consultation has closed and drafting is underway. The window to influence the text is 2026. Key actors already engaged in the process:

- **EuroStack** submitted to the Call for Evidence (March 2025)
- **[OSOR / Interoperable Europe Portal](https://interoperable-europe.ec.europa.eu/collection/open-source-observatory-osor/public-procurement-open-source-software)** is the Commission's existing reference point for OSS procurement guidance
- The IMCO Committee (Internal Market and Consumer Protection) handles procurement legislation in the European Parliament

This proposal should be positioned not as "open source advocacy" but as **technical infrastructure for criteria the Act already intends to introduce** — European origin, community resilience, non-price evaluation. That framing is more durable and less politically contested.

---

## EMBAG (Switzerland)

**Reference:** [fedlex.admin.ch/eli/cc/2023/682/en](https://www.fedlex.admin.ch/eli/cc/2023/682/en)
**Status:** Federal Act on the Use of Electronic Means for the Performance of Government Tasks. In force from January 2024.

### What it does

EMBAG (German: *Bundesgesetz über den Einsatz elektronischer Mittel zur Erfüllung von Behördenaufgaben*) requires Swiss federal authorities to release software developed for or by government under open source licenses, unless security or third-party rights prevent it ("public money, public code" as binding law). It also requires disclosure of algorithms used in automated administrative decisions.

EMBAG is the most concrete legislative template in force for the kind of mandate the APELL initiative and EuroStack coalition are pushing at the EU level.

### How this proposal relates

EMBAG creates the mandate; this proposal provides the infrastructure that makes the mandate actionable.

- **Discovery:** Federal authorities releasing software under EMBAG need a standard way to publish metadata so other authorities (Swiss and European) can find and reuse it. publiccode.yml + faceted classification serves this.
- **Enforcement gap:** Without usage registries and credit registries, there is no way to verify that released software is actually being reused, or that vendors contracted to maintain it are contributing upstream. The proposal closes this gap.
- **Interoperability with EU:** Switzerland participates in the Interoperable Europe ecosystem. publiccode.yml files published by Swiss federal authorities under EMBAG compliance could flow into EU registries through the registry discovery mechanism.

EMBAG demonstrates that "Public Money, Public Code" mandates can be enacted as binding national law. As the EU-level equivalent is debated, Switzerland's implementation provides both a legislative model and a concrete case study for why metadata infrastructure is necessary alongside the mandate.

---

## Cross-Cutting Themes

Several observations apply across all four initiatives:

**The infrastructure gap is the common thread.** CRA creates steward obligations but no discovery mechanism. NIS2 requires supply chain assessment but no standard metadata format. The Interoperable Europe Act requires reuse assessment but discovery is incomplete. The Procurement Act will introduce non-price criteria but lacks measurement tools. This proposal addresses the same underlying gap across all four contexts.

**Decentralization avoids capture.** A centralized EU-run database would create a single point of control, a target for gaming, and a political lightning rod. The federated registry architecture — standard APIs, domain-verified trust, independent operators — can serve all four regulatory contexts without any central authority controlling what is visible.

**Evidence-based compliance is better than checkbox compliance.** Machine-readable supply chain references, contribution records, and usage declarations enable regulators and procurement officers to verify claims rather than accept them. This raises the bar for all participants and creates a self-reinforcing quality incentive.
