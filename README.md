# How to Read This Project

This guide helps you navigate the documentation based on your role and what you need to learn.

## Quick Start: 2-Minute Overview

**What is this project?**

This project evaluates metadata standards for open source software and proposes improvements to `publiccode.yml` as well as concrete standards for decentralized data collection (registry APIs, discovery mechanisms, usage declarations). This data will enable procurement offices, funders, and security teams to discover, evaluate, and confidently adopt open source projects—and to identify and fund critical open source infrastructure and maintenance in a sustainable manner.

**Why this matters:** The proposal pairs technical standards (metadata, APIs, registries) with a strategic push for public procurement policies that prefer open source. This creates a positive feedback loop:

→ procurement policies create demand for standardized project metadata
→ projects and vendors adopt the extended standard
→ procurement offices get better visibility and evaluation tools
→ more confident open source adoption

**Who's it for?**

- **Procurement professionals** considering open source adoption
- **Open source project maintainers** seeking visibility with public sector buyers
- **Policy makers** designing procurement requirements or digital sovereignty mandates
- **Vendors/service providers** wanting to demonstrate expertise
- **Researchers** studying open source governance and digital sovereignty
- **Funders** allocating resources to critical open source infrastructure

**What's proposed?**

A comprehensive ecosystem of standards and registries built around publiccode.yml:

**Proposed Improvements to publiccode.yml:**

1. Faceted classification (better discovery across multiple dimensions)
2. Supply chain references (security assessments, SBOMs, policies)
3. Credit system discovery (vendor contribution tracking)
4. Usage registries (deployment transparency)
5. Temporal field deprecation (cleaner, more trustworthy data)

**Companion Specifications:**

- **Credit Registry API** — Standardized interface for how registries expose vendor/contributor credit data
- **Usage Registry API** — Standardized interface for how registries expose software adoption data
- **Registry Discovery Standard** — How registry operators advertise their existence and capabilities so crawlers can discover them automatically (`/.well-known/publiccode-registry.json`)
- **Organization-Level Usage Declarations** — Standard format for organizations to declare what software they deploy (`.well-known/publiccode-usage.json`)

**Policy Layer:**

- **Public procurement policies** (modeled on "Public Money, Public Code") that prefer open source and require standardized evaluation criteria
- **Procurement selection frameworks** that use publiccode.yml metadata as evidence for vendor expertise, supply chain security, and community health
- **Legislative models** and best practices (including Switzerland's EMBAG law as a reference) for mandating open source in government

**Decentralized Trust Networks:**

- **No central authority controls the data.** Registries operate independently in different jurisdictions and sectors, but use standard APIs and discovery mechanisms.
- **Trust is verifiable, not assumed.** Each registry declares its trust model (`verified-domain`, `signed-attestation`, `self-reported`) so consumers can choose data sources matching their risk tolerance.
- **Data drives decisions, not politics.** By making vendor contributions, supply chain security, and software adoption transparently measurable, procurement and policy become evidence-based rather than relationship-based.

Together, these enable a **self-reinforcing, decentralized ecosystem** where procurement demand drives adoption of open source standards, verified through trust networks, which in turn strengthens the vendors, maintainers, and projects that sustain the digital commons—all while maintaining independence and preventing gatekeeping.

---

## Documents at a Glance

| Document                                 | Purpose                                                                                                                                      | Length  | Best for                                                   | Start here if…                                                          |
| ---------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | ------- | ---------------------------------------------------------- | ----------------------------------------------------------------------- |
| **[GLOSSARY.md](GLOSSARY.md)**           | Key terms explained                                                                                                                          | ~20 min | Everyone                                                   | You encounter unfamiliar terms or acronyms                              |
| **[RESEARCH.md](RESEARCH.md)**           | Comparative analysis of 5 standards                                                                                                          | ~30 min | Researchers, policy makers, infrastructure decision-makers | You want to understand why publiccode.yml and not something else        |
| **[DATA_FLOW.md](DATA_FLOW.md)**         | Ecosystem architecture and data flow diagram                                                                                                 | ~15 min | Architects, system designers, visual learners              | You want to see how the ecosystem connects together                     |
| **[PITCH.md](PITCH.md)**                 | Why each actor benefits                                                                                                                      | 10 min  | Practitioners in each role                                 | You want to know "what's in it for me?"                                 |
| **[PROPOSAL.md](PROPOSAL.md)**           | Detailed technical spec including 5 publiccode.yml improvements plus companion standards (registry APIs, discovery, organization declarations) | ~60 min | Maintainers, spec authors, technical implementers          | You want the concrete technical specification and full ecosystem design |
| **[RISK_ANALYSIS.md](RISK_ANALYSIS.md)** | Identified risks and mitigations                                                                                                             | ~20 min | Risk managers, decision-makers                             | You need to understand what could go wrong                              |
| **[ROADMAP.md](ROADMAP.md)**             | Phased implementation plan                                                                                                                   | ~20 min | Project managers, funders, coalition builders              | You're thinking about how to make this real                             |
| **[METHODOLOGY.md](METHODOLOGY.md)**     | Research process documented                                                                                                                  | ~40 min | Meta/research/reproducibility-focused readers              | You want to understand how conclusions were reached                     |
| **[LICENSE](LICENSE)**                   | CC-BY-SA 4.0                                                                                                                                 | 2 min   | Everyone                                                   | You need reuse permissions                                              |

---

## Reading Paths by Role

### 1. Procurement Professional

_You're evaluating whether open source is right for your organization._

**Start here:**

1. [PITCH.md → Procurement Office](PITCH.md#procurement-office) (2 min) — Why this matters to you
2. [RESEARCH.md → publiccode.yml strengths](RESEARCH.md#1-publiccodeyml) (10 min) — What you're getting
3. [PROPOSAL.md → Design Principles](PROPOSAL.md#design-principles) + [Improvement 1](PROPOSAL.md#improvement-1-faceted-classification-replacing-flat-categories) (15 min) — How discovery improves
4. [PROPOSAL.md → Policy Context](PROPOSAL.md#policy-context-public-procurement-and-open-source) (10 min) — How this connects to procurement frameworks
5. [GLOSSARY.md](GLOSSARY.md) — As needed for unfamiliar terms

**Expected time:** 40 minutes

---

### 2. Open Source Project Maintainer

_You're considering whether to adopt publiccode.yml metadata._

**Start here:**

1. [PITCH.md → OSS Project/Maintainer](PITCH.md#oss-project--maintainer) (2 min) — What you get
2. [RESEARCH.md → publiccode.yml](RESEARCH.md#1-publiccodeyml) (15 min) — How publiccode.yml works
3. [PROPOSAL.md → Design Principles](PROPOSAL.md#design-principles) (5 min) — Philosophy
4. [PROPOSAL.md → Full Example](PROPOSAL.md#full-example) (15 min) — See what it looks like
5. [ROADMAP.md → Phase 1](ROADMAP.md#phase-1-supply-chain-references--temporal-field-deprecation) (5 min) — Earliest opportunities
6. [GLOSSARY.md](GLOSSARY.md) — As needed

**Expected time:** 50 minutes

---

### 3. Vendor / Service Provider

_You deliver expertise around specific open source projects._

**Start here:**

1. [PITCH.md → Vendor/Service Provider](PITCH.md#vendor--service-provider) (2 min) — Why it's good for business
2. [RESEARCH.md → publiccode.yml → No vendor/contributor credit system](RESEARCH.md#1-publiccodeyml) (3 min) — The problem being solved
3. [PROPOSAL.md → Improvement 3: Vendor Credit System Discovery](PROPOSAL.md#improvement-3-vendor-credit-system-discovery) (15 min) — How it works
4. [ROADMAP.md → Phase 3: Credit System Pilot with Drupal](ROADMAP.md#phase-3-credit-system-pilot-with-drupal) (10 min) — Timeline
5. [GLOSSARY.md](GLOSSARY.md) — As needed

**Expected time:** 35 minutes

---

### 4. Policy Maker / Legislator

_You're drafting open source procurement requirements or digital sovereignty mandates._

**Start here:**

1. [PITCH.md → Policy Maker/Legislator](PITCH.md#policy-maker--legislator) (2 min) — Policy opportunities
2. [PROPOSAL.md → Policy Context](PROPOSAL.md#policy-context-public-procurement-and-open-source) (15 min) — Connection to legislation
3. [RESEARCH.md → Other Relevant Standards: Digital Sovereignty Score Initiatives](RESEARCH.md#digital-sovereignty-score-initiatives) (10 min) — Policy landscape
4. [ROADMAP.md → Phases 0, 2, 3, 5](ROADMAP.md) (20 min) — How this would be adopted
5. [GLOSSARY.md](GLOSSARY.md) — As needed

**Expected time:** 50 minutes

---

### 5. Researcher / Standards Specialist

_You're studying open source governance, metadata standards, or supply chain transparency._

**Start here:**

1. [METHODOLOGY.md](METHODOLOGY.md) (40 min) — How conclusions were reached
2. [RESEARCH.md](RESEARCH.md) (30 min) — Comparative analysis of standards
3. [PROPOSAL.md](PROPOSAL.md) (30 min) — Design decisions
4. [RISK_ANALYSIS.md](RISK_ANALYSIS.md) (15 min) — Risk landscape
5. [GLOSSARY.md](GLOSSARY.md) — As needed

**Expected time:** 120 minutes

---

### 6. Funder / Grant Manager

_You allocate money to open source infrastructure and want to understand this proposal._

**Start here:**

1. [PITCH.md → Federal Authority/Funder](PITCH.md#federal-authority--funder) (2 min) — Funding decisions
2. [RESEARCH.md → publiccode.yml](RESEARCH.md#1-publiccodeyml) (15 min) — The infrastructure
3. [RISK_ANALYSIS.md → Critical Risks](RISK_ANALYSIS.md#critical-risks-high-likelihood--high-impact) (10 min) — Where funding is most needed
4. [ROADMAP.md](ROADMAP.md) (20 min) — Phases and milestones for funding decisions
5. [GLOSSARY.md](GLOSSARY.md) — As needed

**Expected time:** 50 minutes

---

### 7. Software Catalog Operator

_You run openCode.de, Developers Italia, or another registry/index._

**Start here:**

1. [PITCH.md → Software Catalog/Crawler](PITCH.md#software-catalog--crawler) (2 min) — Why it matters to you
2. [RESEARCH.md → Federated Architecture](RESEARCH.md#1-publiccodeyml) (5 min) — System design
3. [PROPOSAL.md → Full Design | Improvement 2 | Improvement 4](PROPOSAL.md) (30 min) — What data you'd consume
4. [PROPOSAL.md → Registry Discovery Standard](PROPOSAL.md#registry-discovery-standard-rough-outline) (15 min) — How registries interact
5. [ROADMAP.md → Phase 4](ROADMAP.md#phase-4-registry-discovery--usage-registries) (10 min) — Timeline
6. [GLOSSARY.md](GLOSSARY.md) — As needed

**Expected time:** 65 minutes

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
