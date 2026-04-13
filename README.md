# How to Read This Project

This guide helps you navigate the documentation based on your role and what you need to learn.

## Quick Start: 60-Second Summary

### What This Is

- A proposal to improve `publiccode.yml` and related ecosystem standards.
- A practical path to make open source easier to discover, assess, and procure in the public sector.
- A bridge between technical metadata standards and procurement/policy enforcement.

### Why It Matters

- Public institutions often support digital sovereignty in principle, but procurement still defaults to proprietary vendors.
- Procurement officers face institutional risk when deviating from familiar frameworks.
- Security is often perceived as a proprietary advantage, even though roughly 96% of software products already include open source components (OSBA).
- Missing piece: reliable, comparable, auditable infrastructure for evaluating open source options at scale.
- A data-driven approach lowers perceived personal and institutional risk by replacing relationship-based choices with documented evidence, making it easier and safer to deviate from proprietary-first defaults.

### What This Project Adds

- Better project metadata and discovery standards.
- Registry APIs and discovery mechanisms for decentralized evidence collection.
- Policy-aligned structures so procurement decisions can be transparent and defensible.
- A data-driven basis for procurement decisions that supports justified deviation from incumbent proprietary norms.

### Positive Feedback Loop (Design Intent)

1. Procurement policies and sovereignty mandates create demand for standardized metadata.
2. Projects and vendors adopt the extended standards.
3. Governments gain auditable evidence for procurement, security, and funding decisions.
4. Open source adoption becomes more confident, sustainable, and resilient.

### Who This Is For

- Procurement professionals
- Open source maintainers
- Policy makers and legislators
- Vendors and service providers
- Researchers and standards specialists
- Funders and grant managers
- Software catalog operators

### Core Proposal Components

**Improvements to `publiccode.yml`:**

1. Faceted classification (multi-dimensional discovery)
2. Supply chain references (SBOMs, policies, security evidence)
3. Credit system discovery (vendor and contributor visibility)
4. Usage tracking via external registries (verified decentralized adoption signals)
5. Temporal field deprecation (cleaner and more trustworthy data)

_(Improvement 6: CRA Steward Declaration is designed but deferred pending CRA regulatory guidance. See [PROPOSAL.md](PROPOSAL.md#improvement-6-cra-steward-declaration-deferred).)_

**Companion specifications:**

- Credit Registry API
- Usage Registry API
- Registry Discovery Standard (`/.well-known/publiccode-registry.json`)
- Organization-Level Usage Declarations (`.well-known/publiccode-usage.json`)

**Policy layer and governance alignment:**

- Procurement policies that prefer open source and use standardized criteria
- Selection frameworks based on metadata-backed evidence
- Legislative reference models (including Switzerland's EMBAG law)

### Policy Primer (Optional)

For policy makers new to open source policy: Mirko Boehm, "How Open Source Coordinates: A Guide for Policy Makers" (SSRN, 2026): [https://papers.ssrn.com/sol3/papers.cfm?abstract_id=6303038](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=6303038)

### Trust Model in One Line

- Independent registries expose standardized data and declare trust models (`verified-domain`, `signed-attestation`, `self-reported`) so evidence consumers can choose risk-appropriate sources. See [Design Principles](PROPOSAL.md#design-principles).

---

## Documents at a Glance

| Document                                         | Purpose                                                                                                                                        | Length  | Best for                                                   | Start here if…                                                          |
| ------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------- | ------- | ---------------------------------------------------------- | ----------------------------------------------------------------------- |
| **[GLOSSARY.md](GLOSSARY.md)**                   | Key terms explained                                                                                                                            | ~20 min | Everyone                                                   | You encounter unfamiliar terms or acronyms                              |
| **[RESEARCH.md](RESEARCH.md)**                   | Comparative analysis of 5 standards                                                                                                            | ~30 min | Researchers, policy makers, infrastructure decision-makers | You want to understand why publiccode.yml and not something else        |
| **[DATA_FLOW.md](DATA_FLOW.md)**                 | Ecosystem architecture and data flow diagram                                                                                                   | ~15 min | Architects, system designers, visual learners              | You want to see how the ecosystem connects together                     |
| **[PITCH.md](PITCH.md)**                         | Why each actor benefits                                                                                                                        | 10 min  | Practitioners in each role                                 | You want to know "what's in it for me?"                                 |
| **[PROPOSAL.md](PROPOSAL.md)**                   | Detailed technical spec including 5 publiccode.yml improvements plus companion standards (registry APIs, discovery, organization declarations) | ~60 min | Maintainers, spec authors, technical implementers          | You want the concrete technical specification and full ecosystem design |
| **[EU_POLICY_CONTEXT.md](EU_POLICY_CONTEXT.md)** | How this proposal relates to CRA, NIS2, Interoperable Europe Act, and the Public Procurement Act                                               | ~20 min | Policy makers, legal teams, procurement professionals      | You want to understand the regulatory landscape and legislative hooks   |
| **[RISK_ANALYSIS.md](RISK_ANALYSIS.md)**         | Identified risks and mitigations                                                                                                               | ~20 min | Risk managers, decision-makers                             | You need to understand what could go wrong                              |
| **[ROADMAP.md](ROADMAP.md)**                     | Phased implementation plan                                                                                                                     | ~20 min | Project managers, funders, coalition builders              | You're thinking about how to make this real                             |
| **[METHODOLOGY.md](METHODOLOGY.md)**             | Research process documented                                                                                                                    | ~40 min | Meta/research/reproducibility-focused readers              | You want to understand how conclusions were reached                     |
| **[LICENSE](LICENSE)**                           | CC-BY-SA 4.0                                                                                                                                   | 2 min   | Everyone                                                   | You need reuse permissions                                              |

---

## Reading Paths by Role

Pick the track that matches your available time:

- **Quick path:** 10-20 minutes for orientation and decisions
- **Deep dive:** 35-120 minutes for implementation and policy detail

### 1. Procurement Professional

_You're evaluating whether open source is right for your organization._

**Quick path (12-15 min):**

- [PITCH.md → Procurement Office](PITCH.md#procurement-office)
- [RESEARCH.md → publiccode.yml strengths](RESEARCH.md#1-publiccodeyml)
- [PROPOSAL.md → Policy Context](PROPOSAL.md#policy-context-public-procurement-and-open-source)

**Deep dive (40 min):**

- Add [PROPOSAL.md → Design Principles](PROPOSAL.md#design-principles) and [Improvement 1](PROPOSAL.md#improvement-1-faceted-classification-replacing-flat-categories)
- Use [GLOSSARY.md](GLOSSARY.md) for unfamiliar terms

---

### 2. Open Source Project Maintainer

_You're considering whether to adopt publiccode.yml metadata._

**Quick path (15-20 min):**

- [PITCH.md → Open Source Project/Maintainer](PITCH.md#open-source-project--maintainer)
- [PROPOSAL.md → Full Example](PROPOSAL.md#full-example)
- [ROADMAP.md → Phase 1](ROADMAP.md#phase-1-supply-chain-references--temporal-field-deprecation)

**Deep dive (50 min):**

- Add [RESEARCH.md → publiccode.yml](RESEARCH.md#1-publiccodeyml)
- Add [PROPOSAL.md → Design Principles](PROPOSAL.md#design-principles)
- Use [GLOSSARY.md](GLOSSARY.md) as needed

---

### 3. Vendor / Service Provider

_You deliver expertise around specific open source projects._

**Quick path (12-15 min):**

- [PITCH.md → Vendor/Service Provider](PITCH.md#vendor--service-provider)
- [PROPOSAL.md → Improvement 3: Vendor Credit System Discovery](PROPOSAL.md#improvement-3-vendor-credit-system-discovery)
- [ROADMAP.md → Phase 3](ROADMAP.md#phase-3-credit-system-pilot-with-drupal)

**Deep dive (35 min):**

- Add [RESEARCH.md → publiccode.yml](RESEARCH.md#1-publiccodeyml) for background
- Use [GLOSSARY.md](GLOSSARY.md) for terminology

---

### 4. Policy Maker / Legislator

_You're drafting open source procurement requirements or digital sovereignty mandates._

**Quick path (20-25 min):**

- [EU_POLICY_CONTEXT.md](EU_POLICY_CONTEXT.md)
- [PITCH.md → Policy Maker/Legislator](PITCH.md#policy-maker--legislator)
- [PROPOSAL.md → Policy Context](PROPOSAL.md#policy-context-public-procurement-and-open-source)

**Deep dive (70 min):**

- Add [RESEARCH.md → Digital Sovereignty Score Initiatives](RESEARCH.md#digital-sovereignty-score-initiatives)
- Add [ROADMAP.md](ROADMAP.md) (focus on Phases 0, 2, 3, and 5)
- Use [GLOSSARY.md](GLOSSARY.md) as needed

---

### 5. Researcher / Standards Specialist

_You're studying open source governance, metadata standards, or supply chain transparency._

**Quick path (20-30 min):**

- [RESEARCH.md](RESEARCH.md)
- [PROPOSAL.md](PROPOSAL.md)
- [RISK_ANALYSIS.md](RISK_ANALYSIS.md)

**Deep dive (120 min):**

- Add [METHODOLOGY.md](METHODOLOGY.md) first for provenance and process
- Use [GLOSSARY.md](GLOSSARY.md) for terminology normalization

---

### 6. Funder / Grant Manager

_You allocate money to open source infrastructure and want to understand this proposal._

**Quick path (15-20 min):**

- [PITCH.md → Federal Authority/Funder](PITCH.md#federal-authority--funder)
- [RISK_ANALYSIS.md → Critical Risks](RISK_ANALYSIS.md#critical-risks-high-likelihood--high-impact)
- [ROADMAP.md](ROADMAP.md)

**Deep dive (50 min):**

- Add [RESEARCH.md → publiccode.yml](RESEARCH.md#1-publiccodeyml)
- Use [GLOSSARY.md](GLOSSARY.md) as needed

---

### 7. Software Catalog Operator

_You run openCode.de, Developers Italia, or another registry/index._

**Quick path (15-20 min):**

- [PITCH.md → Software Catalog/Crawler](PITCH.md#software-catalog--crawler)
- [PROPOSAL.md → Registry Discovery Standard](PROPOSAL.md#registry-discovery-standard-rough-outline)
- [ROADMAP.md → Phase 4](ROADMAP.md#phase-4-registry-discovery--usage-registries)

**Deep dive (65 min):**

- Add [RESEARCH.md → Federated Architecture](RESEARCH.md#1-publiccodeyml)
- Add [PROPOSAL.md](PROPOSAL.md) (focus on Improvement 2 and Improvement 4)
- Use [GLOSSARY.md](GLOSSARY.md) as needed

---

## Tips for Reading

**Terminology tip:** If you see an acronym or unfamiliar term, check [GLOSSARY.md](GLOSSARY.md) first. It's designed to make everything accessible.

**Links:** Click links to jump between sections. Most documents cross-reference each other so you can read non-linearly.

**Tables:** Scan tables first—they often contain the key insights in compact form.

**Technical sections:** [PROPOSAL.md](PROPOSAL.md) has YAML examples. You don't need to understand YAML syntax; read the descriptions alongside the code blocks.

**Scope boundaries:** Each document tries to be honest about what it does _not_ cover. If something seems missing, check the Risk Analysis and Methodology sections for why.

---

## How to Contribute Feedback

This project is open for feedback and collaboration. Key places to raise ideas:

- **On the proposal:** See [ROADMAP.md → Phase 0 → Actions](ROADMAP.md#phase-0-coalition--governance) for governance processes
- **On research methodology:** See [METHODOLOGY.md → Limitations](METHODOLOGY.md#limitations)
- **On specific risks:** See [RISK_ANALYSIS.md](RISK_ANALYSIS.md) — if you see a missing risk, that's valuable input
- **On implementation details:** See [ROADMAP.md](ROADMAP.md) — phases are sequenced but not set in stone

---

## Project Status

- ✅ **Completed:** Initial comparative analysis (RESEARCH.md), proposal (PROPOSAL.md), pitches (PITCH.md), risk analysis (RISK_ANALYSIS.md)
- ✅ **Completed:** Possible roadmap and initial coalition mapping (ROADMAP.md)
- 🔄 **Current:** Community review for completeness and accuracy
- 🔄 **Next:** Find initial allies and open governance discussions (Phase 0 of ROADMAP.md)

---

**License:** CC-BY-SA 4.0 (see LICENSE file)
