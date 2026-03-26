# EU Policy Context

This document tracks EU legislative initiatives relevant to this proposal and explains how the metadata infrastructure proposed here relates to each.

---

## Overview

| Law / Initiative                                        | Status                                               | Primary hook                                                                                | Proposal role                                                            |
| ------------------------------------------------------- | ---------------------------------------------------- | ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| [Cyber Resilience Act (CRA)](#cyber-resilience-act-cra) | In force (2024), obligations phasing in through 2027 | OSS steward obligations; supply chain security evidence                                     | Machine-discoverable evidence layer for steward compliance               |
| [NIS2 Directive](#nis2-directive)                       | In force, transposition deadline Oct 2024            | Supply chain risk management in 18 critical sectors                                         | Standardized supply chain metadata reduces compliance cost               |
| [Interoperable Europe Act](#interoperable-europe-act)   | In force since July 2024                             | OSS reuse obligation; EU OSS Catalogue                                                      | publiccode.yml is already the Catalogue onboarding standard              |
| [Public Procurement Act](#public-procurement-act)       | Proposal expected Q2 2026                            | Non-price criteria; "Made in Europe"; SME access                                            | Metadata infrastructure makes non-price criteria measurable              |
| [EMBAG (Switzerland)](#embag-switzerland)               | In force since 2024                                  | Open source release mandate for public software                                             | Faceted classification + discovery make the mandate actionable           |
| [EU AI Act](#eu-ai-act)                                 | In force Aug 2024; GPAI obligations from Aug 2025    | Annex XII supply chain docs; OSS GPAI partial exemption; high-risk AI in public procurement | AI component metadata; procurement due diligence layer                   |
| [DORA](#dora--digital-operational-resilience-act)       | In force Jan 2025 (financial sector)                 | Register of ICT contracts; Article 28 supply chain due diligence                            | supplyChain fields fill the OSS gap in DORA's register requirements      |
| [EU Data Act](#eu-data-act)                             | Applicable Sep 2025                                  | Cloud switching interoperability; open API mandate                                          | Registry open API architecture aligns with interoperability obligations  |
| [EHDS](#european-health-data-space-ehds)                | In force Mar 2025; phased application to 2031        | EHR certification; health sector interoperability                                           | supplyChain fields + steward declaration support EHR conformity evidence |
| [EUCS](#eu-cybersecurity-certification-scheme-eucs)     | Candidate scheme; formal adoption pending            | Cloud security certification; tiered assurance levels                                       | supplyChain metadata supports conformity assessment evidence             |
| [European Competitiveness Fund](#european-competitiveness-fund) | Proposal [COM(2025) 16 final](https://commission.europa.eu/publications/european-competitiveness-fund_en), under negotiation | Procurement as "integrated financial toolbox" for technological sovereignty; InvestEU de-risking | Registry infrastructure operationalizes ECF's sovereignty procurement objectives |
| [Council Conclusions on European Competitiveness in the Digital Decade](#council-conclusions-on-european-competitiveness-in-the-digital-decade) | [Adopted 2025 (ST‑16430‑2025‑INIT)](https://data.consilium.europa.eu/doc/document/ST-16430-2025-INIT/en/pdf) | Open standards, open source, interoperability to reduce vendor lock-in; strategic procurement for R&I | Provides political mandate for the metadata infrastructure this proposal delivers |
| [Digital Decade Policy Programme](#digital-decade-policy-programme) | [In force (Regulation (EU) 2022/2481)](https://digital-strategy.ec.europa.eu/en/policies/digital-decade-policy-programme); targets for 2030 | All key public services online by 2030; public administration digital transformation | Usage registries + sovereignty metrics track whether digitalization builds or deepens dependency |

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
- **Improvement 2 (Supply Chain References):** Security and compliance data becomes available at the point of discovery, supporting the Act's requirement to assess interoperability _before_ procurement.
- **Improvement 4 (Usage Registries) + Organization-Level Declarations:** The Act requires sharing reusable solutions. Usage declarations make peer adoption visible — a public body can see which comparable organizations already run a solution, supporting the "consider reuse" obligation.
- **Registry Discovery Standard:** The `/.well-known/publiccode-registry.json` mechanism enables crawlers operated by the Interoperable Europe Portal to discover new registries automatically, without manual onboarding.

**Key insight:** The Interoperable Europe Act creates an _existing, active_ legal obligation for EU public bodies to assess and share open source. This proposal improves the infrastructure that already serves this obligation. The EU OSS Catalogue team is a natural first institutional partner.

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

| Procurement criterion         | Proposal component                      | What it measures                                 |
| ----------------------------- | --------------------------------------- | ------------------------------------------------ |
| Security posture              | Supply Chain References (Improvement 2) | SBOM, OpenSSF Scorecard, vulnerability policy    |
| Vendor expertise              | Credit Registries (Improvement 3)       | Upstream contributions, not self-reported claims |
| Community health / resilience | Usage Registries (Improvement 4)        | Peer deployments, verified by domain control     |
| Sector fit                    | Faceted Classification (Improvement 1)  | Multi-dimensional classification                 |

The Directives already permit these criteria — the barrier to using them is institutional, not legal. Machine-verifiable registry data lowers the legal risk of qualitative award decisions in a way that narrative vendor assessments cannot. See [Cross-Cutting Themes](#cross-cutting-themes).

#### 3. SME discoverability

SMEs with deep open source expertise are invisible to procurement offices that rely on name recognition. The credit registry inverts this: an SME with 400 upstream commits to a project and a verified maintenance role is _findable_ by any procurement office running a registry query. No set-asides or quotas required — just transparent, verifiable contribution data.

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

EMBAG (German: _Bundesgesetz über den Einsatz elektronischer Mittel zur Erfüllung von Behördenaufgaben_) requires Swiss federal authorities to release software developed for or by government under open source licenses, unless security or third-party rights prevent it ("public money, public code" as binding law). It also requires disclosure of algorithms used in automated administrative decisions.

EMBAG is the most concrete legislative template in force for the kind of mandate the APELL initiative and EuroStack coalition are pushing at the EU level.

### How this proposal relates

EMBAG creates the mandate; this proposal provides the infrastructure that makes the mandate actionable.

- **Discovery:** Federal authorities releasing software under EMBAG need a standard way to publish metadata so other authorities (Swiss and European) can find and reuse it. publiccode.yml + faceted classification serves this.
- **Enforcement gap:** Without usage registries and credit registries, there is no way to verify that released software is actually being reused, or that vendors contracted to maintain it are contributing upstream. The proposal closes this gap.
- **Interoperability with EU:** Switzerland participates in the Interoperable Europe ecosystem. publiccode.yml files published by Swiss federal authorities under EMBAG compliance could flow into EU registries through the registry discovery mechanism.

EMBAG demonstrates that "Public Money, Public Code" mandates can be enacted as binding national law. As the EU-level equivalent is debated, Switzerland's implementation provides both a legislative model and a concrete case study for why metadata infrastructure is necessary alongside the mandate.

---

## EU AI Act

**Reference:** [digital-strategy.ec.europa.eu/en/policies/european-approach-artificial-intelligence](https://digital-strategy.ec.europa.eu/en/policies/european-approach-artificial-intelligence)
**Status:** Regulation (EU) 2024/1689, in force August 1, 2024. General-Purpose AI (GPAI) obligations applied August 2, 2025. Full application by August 2, 2026.

### What it does

The AI Act is a horizontal product safety regulation for AI systems. For open source specifically, it creates two distinct compliance contexts:

**Open source GPAI model providers (Article 53):** Providers of general-purpose AI models released under open-source licenses are exempt from the Article 53(1) documentation and copyright compliance requirements — with one exception: models presenting systemic risk (trained using compute exceeding 10²⁵ FLOPs) are not exempt regardless of licensing.

**Supply chain documentation for downstream integrators (Article 25, Annex XII):** GPAI providers must pass structured technical documentation downstream to every organization integrating their model. Annex XII defines what must be documented: model architecture, training data, performance benchmarks, security characteristics, and known limitations. This is functionally an AI-specific software supply chain disclosure requirement.

**High-risk AI in public procurement (Annex III, Article 6):** AI systems used in public administration — for essential public services, benefits decisions, public employment — are classified as high-risk. Public bodies deploying high-risk AI are "deployers" responsible for conformity verification, EU AI database registration, and human oversight, creating procurement due diligence obligations.

### How this proposal relates

The `supplyChain` fields proposed in [Improvement 2](PROPOSAL.md#improvement-2-supply-chain-references) extend naturally to AI components:

- `supplyChain.sbom` can reference an SBOM enumerating AI model dependencies alongside software dependencies
- A future field linking to Annex XII documentation would make GPAI compliance machine-discoverable for any public body procuring an AI-dependent system
- The credit registry (Improvement 3) could surface which EU-registered entities maintain a given open source AI model — providing credible "European origin" evidence for AI procurement preferences

The open source GPAI exemption creates a competitive asymmetry: open source AI models carry lighter documentation obligations than proprietary ones. Exploiting this advantage requires the open source model to be discoverable. Faceted classification (Improvement 1) covering AI model type, domain, and capability enables procurement officers to find open source AI alternatives efficiently.

**Key insight:** The AI Act's Annex XII requirement is structurally the same problem as the CRA's SBOM requirement — both define what supply chain information must flow through product value chains. A unified metadata standard serving both contexts is more efficient than sector-specific compliance silos.

---

## DORA — Digital Operational Resilience Act

**Reference:** [EUR-Lex: Regulation (EU) 2022/2554](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32022R2554)
**Status:** Regulation (EU) 2022/2554. Applied January 17, 2025. Covers banks, insurers, investment firms, payment institutions, and 15 other financial entity types across the EU.

### What it does

DORA requires financial entities to manage digital operational resilience with a specific focus on ICT third-party risk:

- **Register of information (Article 28):** All financial entities must maintain a complete, structured register of every ICT third-party service contract — at entity, sub-consolidated, and consolidated group levels. Submittable to regulators on request, reportable annually. Regulatory technical standards define required fields.
- **Pre-contract due diligence (Article 28(3)):** Before any ICT service contract, entities must assess whether the service relates to a critical function, perform proportionate due diligence, and verify providers meet "appropriate information security standards."
- **Subcontracting transparency (Article 30):** Contracts covering critical functions must specify all sub-contractors; the contracting entity is responsible for sub-contractor ICT risk.
- **ICT concentration risk (Article 29):** Entities must identify over-reliance on a small number of ICT providers — including shared dependencies across the financial system.
- **Critical Third-Party Provider (CTPP) oversight (Articles 31–44):** Direct EU-level oversight of the largest ICT providers by the EBA, ESMA, or EIOPA.

### How this proposal relates

DORA's register covers ICT service _contracts_ — but open source software rarely accompanies a formal contract with the upstream project. This creates a gap: a bank using an open source database driver or cryptographic library for a critical function has a DORA obligation to document that dependency, but DORA provides no mechanism for it.

The proposal addresses this gap in several ways:

1. **At inventory time:** Organization-level usage declarations (`.well-known/publiccode-usage.json`) provide the structured, domain-verified inventory of deployed open source software that DORA's register implicitly requires for open source components.
2. **At assessment time:** The `supplyChain` references (SBOM, security policy, Scorecard, vulnerability disclosure) allow a financial entity to perform Article 28(3) due diligence from a single standardized entry point — rather than manually researching each of the hundreds of open source components in a typical enterprise stack.
3. **Systemic risk monitoring:** DORA Article 29 focuses on vendor concentration, but the systemic risk from shared open source dependencies (see Log4j 2021, XZ Utils 2024) is structurally identical. Usage registries aggregating declared deployments across financial entities could give regulators cross-sector visibility into open source concentration risk — something no current DORA mechanism provides.
4. **At contractor documentation time:** publiccode.yml's `maintenance.contractors[]` lists organizations providing contracted support for a project — useful for Article 28(3) pre-contract due diligence on what support options exist. The usage declaration (`.well-known/publiccode-usage.json`) complements this with an optional `software[].contractors[]` field at the _deployment_ level, using the same `name`/`website`/`identifier` pattern. This lets a financial entity record which specific vendor holds _their_ support contract for a given OSS deployment — mapping directly to DORA's Article 28 register and Article 30 subcontracting transparency requirements, and completing the DORA compliance picture for OSS ICT dependencies where no formal contract with the upstream project exists.

**Key insight:** DORA creates compliance demand for open source supply chain documentation from the financial sector — independently of any public sector procurement or open source policy motivation. Banks and insurers subject to DORA's Article 28 register requirements have a legal obligation to document their open source ICT dependencies; standardized metadata reduces the cost of meeting that obligation and could be recognized as a compliant approach.

---

## EU Data Act

**Reference:** [digital-strategy.ec.europa.eu/en/policies/data-act](https://digital-strategy.ec.europa.eu/en/policies/data-act)
**Status:** Regulation (EU) 2023/2854. In force January 11, 2024. Applicable from **September 12, 2025**.

### What it does

The Data Act governs fair access to and use of data generated by connected products and services:

- **Data access rights (Chapter II):** Individuals and businesses have rights to access data generated by connected IoT devices they own or use. Contractual terms that unfairly block data sharing are prohibited.
- **Cloud switching (Chapter VI, Articles 23–35):** Cloud service providers must enable customers to switch providers within prescribed timeframes, with switching charges eliminated by 2027. Providers must implement "open interoperability specifications and harmonised standards" to enable portability.
- **Open API requirements (Article 26):** Cloud providers must provide and maintain open, published interfaces enabling portability — creating a de facto alignment requirement with open standards.
- **B2G data sharing (Articles 14–20):** Public bodies can compel private sector data holders to share data for specific public interest purposes, including emergency response and public administration.

### How this proposal relates

The Data Act's relevance is structural rather than direct:

- **Cloud interoperability mandate:** The requirement for open, published cloud APIs favors open source cloud implementations (whose APIs are inherently open) and open standards. European cloud providers competing with US hyperscalers can use open source stacks to satisfy portability requirements that proprietary vendors must build specifically for compliance. The registry discovery standard (`.well-known/publiccode-registry.json`) is architecturally consonant with the "open API" model the Data Act mandates.
- **B2G data provisions:** Public administrations can request data from private sector entities for public interest purposes. This mechanism could theoretically be used to obtain structured dependency data for ICT services procured by public bodies — extending the declared usage model to contracted services, not just internally deployed software.

**Key insight:** The Data Act does not directly regulate open source software, but its cloud interoperability and open API provisions create systemic pressure toward open standards. The proposal's registry architecture — open API, standard manifests, domain-verified trust — is more naturally aligned with the Data Act's interoperability direction than a centralized closed-registry approach would be.

---

## European Health Data Space (EHDS)

**Reference:** [health.ec.europa.eu/ehealth-digital-health-and-care/european-health-data-space_en](https://health.ec.europa.eu/ehealth-digital-health-and-care/european-health-data-space_en)
**Status:** Regulation (EU) 2025/..., in force March 26, 2025. Phased application: implementing acts by 2027; patient summaries and ePrescriptions by 2029; medical images and lab results by 2031.

### What it does

The EHDS is the first sector-specific EU data space:

- **EHR system certification:** Electronic health record (EHR) software must meet harmonized interoperability and security standards before market entry — a CE-marking regime for health IT. Manufacturers must demonstrate conformity before their systems can be used in healthcare.
- **Patient data rights:** Individuals gain access, correction, and restriction rights over their health data, portable across EU member states in a standardized European Electronic Health Record Exchange Format (EEHRxF).
- **Secondary use framework:** Regulated access for researchers and policymakers to pseudonymized health data through national health data access bodies.

### How this proposal relates

EHDS EHR certification specifically affects open source healthcare software:

- Open source EHR systems must navigate the same EHDS certification pathway as proprietary vendors. The `supplyChain` fields (SBOM, security policy, Scorecard, vulnerability disclosure) and the CRA steward declaration provide the evidence needed to support a conformity assessment: who is responsible for the software, what its security posture is, and how vulnerabilities are handled.
- The health sector is one of NIS2's 18 critical sectors. EHDS-covered healthcare organizations therefore face both EHDS certification requirements for EHR systems and NIS2 supply chain risk management obligations for all ICT — creating layered demand for the same metadata infrastructure.
- National health data access bodies will operate or procure data governance platforms. As public bodies, they fall under the Interoperable Europe Act's reuse obligation and are natural contributors of open source tooling to the EU OSS Catalogue.

**Key insight:** EHDS adds a sector-specific product certification layer on top of NIS2. Open source EHR software providers who build publiccode.yml supply chain references into their compliance documentation have a reusable evidence artifact for both regimes — without duplicating the underlying data.

---

## EU Cybersecurity Certification Scheme (EUCS)

**Legal basis:** EU Cybersecurity Act, Regulation (EU) 2019/881, Article 48
**Status:** Candidate scheme developed by ENISA; formal Commission implementing act pending as of early 2026.

### What it does

EUCS provides tiered cybersecurity certification for cloud services:

- **Basic:** Self-assessment
- **Substantial:** Third-party conformity assessment by an accredited body
- **High:** Rigorous third-party assessment for sensitive data and critical infrastructure deployments

EUCS is not yet mandatory in procurement, but is expected to function as a de facto requirement for sensitive public sector cloud procurement once formally adopted. NIS2 creates a backdrop in which EUCS-level assurance signals become natural qualification criteria for cloud procurement by critical-sector entities.

The scheme went through significant revision during drafting; an earlier draft containing stricter EU-jurisdiction requirements for the High level was contested by US cloud providers and the US government, and the final scheme adopted an "operational sovereignty" model rather than a corporate ownership criterion.

### How this proposal relates

EUCS conformity assessment at Substantial and High levels requires evidence of the service provider's security practices. For open source cloud services — a growing segment among European providers — the `supplyChain` references (SBOM, Scorecard, security policy, vulnerability disclosure) are machine-readable artifacts directly relevant to conformity assessment. A cloud service provider with these fields in their publiccode.yml has already assembled much of the evidence package a conformity assessment body would request.

**Key insight:** EUCS creates tiered procurement signals for cloud services. Open source cloud providers that maintain machine-readable security metadata via publiccode.yml are structurally better positioned for EUCS conformity assessment — and for public sector procurement contexts where EUCS certification becomes expected.

---

## Policy Environment: Strategic Frameworks and Emerging Legislation

The following are not yet legislative instruments but shape the drafting and political context of the laws above.

### EU Competitiveness Compass and Draghi Report

The **Draghi Report** ("The future of European competitiveness," September 2024) and the **EU Competitiveness Compass** (January 2025) are the Commission's strategic framework driving several of the laws above.

Findings relevant to this proposal:

- European public procurement is **fragmented**: 27 member states procure IT independently, giving US hyperscalers market power through fragmented demand. Cross-border registry infrastructure — a natural output of this proposal — directly addresses this fragmentation.
- The Competitiveness Compass explicitly calls for a **"European preference"** in procurement for critical technology sectors, feeding the Public Procurement Act drafting.
- The Draghi Report identifies regulatory compliance **cost overlap** (NIS2, DORA, CRA, AI Act, and Data Act all simultaneously in force) as a burden on European SMEs. Standardized metadata satisfying multiple compliance regimes is a form of regulatory simplification the Compass endorses in principle.

#### The "European preference" tension with open source

The "European preference" framing creates a conceptual problem when applied to open source software that EU policy documents have not yet resolved.

Open source software released under an OSI-approved license is, **by definition, a global commons**: it carries non-discrimination clauses that forbid restricting use by territory, nationality, or purpose. A piece of software cannot simultaneously be open source and preferentially European in its downstream availability. As Thierry Carrez (OSI Vice-Chair) puts it: calling for "European Open Source" downstream of a project release is a category error — open source has no origin. ([_Open Source: A global commons to enable digital sovereignty_](https://opensource.org/blog/open-source-a-global-commons-to-enable-digital-sovereignty), OSI, November 2025.)

What IS meaningful — and what the Competitiveness Compass likely _intends_ — is an **upstream** question: who authors, maintains, and controls the development of software _before_ it is released? A project developed and governed predominantly by EU-registered entities, even if released as open source for global use, represents a fundamentally different sovereignty posture than one controlled by a single non-EU vendor or government. This is an assertion about the _production_ of software, not its _licensing_.

Carrez frames digital sovereignty through open source around three concrete needs:

- **Day 0 integrity:** Critical services must not be subject to extraterritorial laws or unilateral access demands at the point of deployment.
- **Day 1 resilience:** No party should hold a software kill switch — the ability to disable or degrade the software for a specific user or territory.
- **Day 2 continuity:** In a scenario of global fragmentation, organizations must be able to continue operating the software independently, without upstream cooperation.

Open source satisfies all three by default — but only if the _supply chain_ is healthy. A project nominally open source but de-facto controlled by a single non-EU entity may still fail the day 1 and day 2 tests if that entity can capture or abandon the codebase. This is precisely why upstream governance metadata — maintainer diversity, steward identity, contribution attribution — matters for sovereignty analysis, not just licensing.

**Implication for this proposal:** The credit registry (Improvement 3) provides the infrastructure to answer the upstream sovereignty question in a technically credible, non-gameable way. Verified contribution data tied to EU-registered entities is the only mechanism that can distinguish "software developed by a European community" from "software developed abroad and merely used in Europe." The "European preference" policy goal, properly understood, requires exactly this kind of measurement infrastructure.

#### WTO Government Procurement Agreement (GPA) compatibility

EU preferencing in public procurement must be designed to remain consistent with the EU's obligations under the WTO Government Procurement Agreement, which covers IT services and prohibits discrimination against GPA signatories' suppliers. A blanket "EU providers only" rule for software would raise compliance questions.

The credit registry approach is structurally more GPA-compatible than a nationality restriction: it establishes a quality criterion (verified upstream maintenance contribution) that any provider — EU or non-EU — can satisfy by making substantive, project-endorsed contributions. This is a performance-based criterion, not a nationality restriction. It rewards the behavior the policy actually seeks (sustainable, accountable software development) rather than the organizational attribute of being EU-incorporated. Procurement preferences structured around contribution evidence, security posture, and ecosystem health criteria can be applied in a transparent and proportionate manner consistent with the GPA framework — as other GPA signatories have demonstrated when applying strategic criteria around critical infrastructure.

### CADA — Cloud and AI Development Act

**Status:** Public consultation completed mid-2025. No formal legislative proposal as of early 2026.

The Commission's **AI Continent Action Plan** (February 2025) established CADA as the vehicle for building European cloud and AI infrastructure capacity. If CADA includes open source AI development provisions, the supply chain documentation and discovery infrastructure proposed here becomes directly relevant to AI model governance — extending the scope of this proposal's applicability.

**Scope boundary:** This proposal operates at the software and metadata layer — publiccode.yml, credit registries, usage declarations, supply chain references. It does not address cloud infrastructure sovereignty (i.e., where software physically runs, under whose legal jurisdiction, and on whose hardware). Even with widespread open source software adoption enabled by this proposal, dependency on non-EU hyperscaler infrastructure persists unless addressed separately at the infrastructure layer. CADA is the natural vehicle for that complementary requirement; the two layers are independent and both necessary for full digital sovereignty.

### Digital Omnibus — EU Digital Simplification Package

**Status:** Proposed 2026; under consultation.

The EU Commission's Digital Omnibus proposes simplifying digital regulation by adjusting thresholds and procedures across NIS2, DORA, CRA, and the AI Act simultaneously.

**The risk and the opportunity for this proposal:**

_Risk:_ If NIS2 size thresholds are raised or CRA conformity assessment is simplified, the regulatory demand for supply chain metadata that this proposal targets may be reduced.

_Opportunity:_ The simplification frame creates the strongest possible argument for standardized metadata. A single publiccode.yml supply chain reference satisfying CRA, NIS2, DORA, AI Act, and EHDS evidence requirements simultaneously is less costly than bespoke per-regime documentation. **Standardized metadata is a form of regulatory simplification that does not weaken protection** — the most politically defensible kind.

---

### European Competitiveness Fund

**Reference:** [Proposal for a Regulation on establishing the European Competitiveness Fund, COM(2025) 16 final, 2025/0007 (COD)](https://commission.europa.eu/publications/european-competitiveness-fund_en)
**Status:** Legislative proposal under negotiation; part of the proposed 2028–2034 Multiannual Financial Framework.

#### What it does

The European Competitiveness Fund (ECF) is a proposed MFF instrument consolidating several existing EU funding mechanisms. It explicitly positions procurement as part of an **"integrated financial toolbox"** for achieving technological sovereignty — one of the few EU legislative instruments to treat procurement and investment as coordinated levers rather than separate policy tracks.

The ECF's InvestEU instrument provides loans, guarantees, and equity to de-risk uptake of innovative alternatives to incumbent providers. The "instrument-neutral award procedures" allow applicants to propose solutions without being constrained to a specific funding model — including open source solutions that do not fit traditional licensing cost structures.

#### How this proposal relates

The ECF's framing of procurement as a sovereignty tool creates a direct mandate for the kind of metadata infrastructure this proposal describes. If the ECF is to channel demand toward sovereign alternatives, those alternatives must be discoverable and their sovereignty-relevant properties (EU maintenance, open supply chain, peer adoption) must be verifiable. Without that infrastructure, "sovereignty-oriented procurement" remains a stated objective with no operational mechanism.

The credit registry and usage registry components directly support ECF investment targeting: funders can identify which widely-deployed open source infrastructure is under-resourced (critical gap) and which has a healthy vendor ecosystem (lower investment priority), allocating capital where it creates maximum leverage.

**Key insight:** The ECF is the first EU instrument to explicitly link procurement and investment as coordinated tools for sovereignty. This proposal is the technical infrastructure that makes that linkage operational at the software layer.

---

### Council Conclusions on European Competitiveness in the Digital Decade

**Reference:** [Council of the European Union, ST‑16430‑2025‑INIT](https://data.consilium.europa.eu/doc/document/ST-16430-2025-INIT/en/pdf)
**Status:** Adopted 2025. Council Conclusions are politically binding on the Commission's agenda but do not create directly applicable law.

#### What it does

These Council Conclusions explicitly identify **open standards, open source, and interoperability** as mechanisms to enhance transparency and competition while reducing vendor lock-in and reliance on single providers. Paragraph 27 specifically recognizes strategic public procurement as a tool to support research and innovation and the deployment of EU suppliers.

This is significant because Council Conclusions reflect consensus among all 27 Member States — making the open source and interoperability framing a political baseline, not just a Commission position.

#### How this proposal relates

The Conclusions' framing of open source as a tool for reducing vendor lock-in maps directly to this proposal's credit registry and usage registry components:

- Vendor lock-in reduction requires knowing whether a software choice has multiple independent support providers — exactly what credit registries reveal.
- Reducing reliance on single providers requires procurement criteria that reward multi-vendor ecosystems — criteria this infrastructure makes machine-verifiable.

The Conclusions' endorsement of strategic procurement as an R&I tool also provides political grounding for the pre-commercial procurement mechanisms this proposal can support: credit registry data showing early-stage vendor investment in a project is evidence that public procurement dollars create R&I leverage, not just purchasing.

**Key insight:** The Council Conclusions provide the inter-institutional political mandate for open source procurement preferences that the Commission's proposals are beginning to operationalize. This proposal is the technical layer between the political mandate and enforceable criteria.

---

### Digital Decade Policy Programme

**Reference:** [Regulation (EU) 2022/2481 of the European Parliament and of the Council of 14 December 2022](https://digital-strategy.ec.europa.eu/en/policies/digital-decade-policy-programme)
**Status:** In force. Targets set for 2030; annual progress reporting mechanism in place.

#### What it does

The Digital Decade Policy Programme establishes the EU's framework for digital transformation through 2030, including a target of moving all key public services online. It creates a monitoring and reporting mechanism with annual State of the Digital Decade reports tracking progress against quantitative targets.

The Programme's public administration digitalization targets create a structural demand driver: as more public services move online, the software infrastructure supporting them becomes more consequential — and the question of whether that infrastructure is open, interoperable, and sovereignty-compatible becomes more urgent.

#### How this proposal relates

The Digital Decade Programme sets targets for digital transformation but does not currently include sovereignty-oriented metrics for how that transformation is achieved. A public service moved online on a proprietary non-EU platform satisfies the Programme's target; the same service on an open, interoperable, EU-maintained platform satisfies it equally — but the two outcomes are not equivalent from a sovereignty standpoint.

This creates a measurement gap: the Programme tracks whether services are digital, not whether the digitalization strengthens or weakens EU autonomy. Usage registries — which can capture the technology stack underlying declared public service deployments — are the natural data source for adding sovereignty-oriented metrics to Digital Decade reporting.

Specifically, the Open Future procurement brief identifies a direct dependency: the Digital Decade's 2030 public service targets, absent a framework prioritizing open source and interoperability, will default toward proprietary technology procurement. This proposal's infrastructure gives the Programme the measurement tools to track and incentivize the alternative path.

**Key insight:** Digital Decade targets create a procurement wave for public sector digitalization through 2030. Without sovereignty metrics embedded in that programme's reporting, the wave will flow toward incumbent providers by default. Usage registry data can close this measurement gap without requiring new legislative authority.

---

## Cross-Cutting Themes

Several observations apply across all initiatives covered in this document:

**The infrastructure gap is the common thread.** CRA creates steward obligations but no discovery mechanism. NIS2 requires supply chain assessment but no standard metadata format. DORA mandates a register of ICT contracts but has no mechanism for open source components with no formal contract. The AI Act requires Annex XII documentation to flow through value chains but defines no standard format. The Interoperable Europe Act requires reuse assessment but discovery is incomplete. The Procurement Act will introduce non-price criteria but lacks measurement tools. EHDS requires EHR certification evidence. This proposal addresses the same underlying infrastructure gap across all contexts simultaneously.

**The "European origin" criterion requires a technical answer, not a legal one.** The Competitiveness Compass, the Public Procurement Act's European preference, and industry submissions to the Call for Evidence all converge on the same unsolved problem: how to define "European" for software in a way that is auditable and non-gameable. But the question is also conceptually ill-formed unless it is applied _upstream_: open source software is a non-discriminatory global commons by license, so "European" cannot be a property of the released artifact — it can only be a property of who produces and governs it. The credit registry (Improvement 3) — verified, project-endorsed contribution data tied to EU-registered entities — is the only technically credible answer currently proposed, and it answers the right version of the question (see [European preference tension](#the-european-preference-tension-with-open-source) above).

**Open source is transitioning from principle to measurement problem.** The policy debate has moved from "should public bodies consider open source?" (largely settled in favor) to "how do we measure whether a software choice is European, secure, community-healthy, and reusable?" The Procurement Act's non-price criteria, the Competitiveness Compass's simplification agenda, and the various "open source first" proposals submitted to the Public Procurement Act consultation all presuppose measurement infrastructure that does not yet exist in standardized form. This proposal is that infrastructure.

**Decentralization avoids capture.** A centralized EU-run database would create a single point of control, a target for gaming, and a political lightning rod. The federated registry architecture — standard APIs, domain-verified trust, independent operators — can serve all regulatory contexts without any central authority controlling what is visible.

**Evidence-based compliance is better than checkbox compliance.** Machine-readable supply chain references, contribution records, and usage declarations enable regulators and procurement officers to verify claims rather than accept them. This raises the bar for all participants and creates a self-reinforcing quality incentive.

**Standardized metadata is a form of regulatory simplification.** The Digital Omnibus creates pressure to reduce regulatory compliance costs. One machine-readable metadata entry satisfying CRA, NIS2, DORA, AI Act, and EHDS evidence requirements simultaneously is cheaper than bespoke documentation for each regime — framing this proposal as an enabler of the Commission's own simplification agenda, without weakening any protection.

**The barrier to qualitative procurement criteria is institutional, not legal.** The existing Public Procurement Directives already permit non-price and qualitative award criteria. The reason procurement remains price-dominated is that subjective qualitative assessments are expensive to defend against legal challenge and require technical expertise most contracting authorities lack. Machine-verifiable registry data changes this: a procurement decision backed by an auditable credit registry query is defensible in a way that a narrative vendor evaluation is not. This proposal reduces the institutional cost of using criteria that are already legally available — which matters more than introducing new legal authority to use them.
