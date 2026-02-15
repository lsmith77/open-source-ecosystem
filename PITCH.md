# Stakeholder Pitches

Why each actor in this ecosystem benefits from the proposed integrated infrastructure: publiccode.yml extensions, companion registries and standards, and policy frameworks. Each pitch focuses on economic and political motivations—why implementation matters to them.

---

## OSS Project / Maintainer

- **Reach the public sector market** — EU procurement is shifting to open source (EU CSF, openCode.de, Developers Italia). One metadata file plus integration with credit registries and usage registries puts your project in front of buyers across 27 member states.
- **Attract funding and contributors** — projects with visible adoption data (via usage registries), clear classification, credited vendor ecosystem, and linked security assessments are easier for funders (Sovereign Tech Fund, CISA) to justify investing in.
- **Control your narrative** — you decide how the project is classified, which credit registries you endorse, what your supply chain references point to, and which vendors or projects are recognized as contributors—rather than letting third-party catalogs guess.
- **Make adoption visible and current** — integrate usage declaration updates into your deployment documentation so that every deploying organization automatically signals adoption through decentralized registries. More visible adoption means more funding, more contributors, and more leverage.
- **Participate in verifiable vendor ecosystems** — credit registries with explicit trust models allow you to surface and recognize the organizations that maintain and extend your software, building trust for procurement decisions without centralizing data.
- **Provide the data that lawmakers and procurement offices need** — "Public Money, Public Code" laws and open source procurement mandates are spreading across Europe, but without comprehensive metadata infrastructure (classification, credit registries, usage registries, supply chain visibility, trust networks), compliance is unverifiable. The full ecosystem makes your project the kind of software these policies can actually require and evaluate.

---

## Vendor / Service Provider

- **Win public sector contracts** — procurement offices increasingly evaluate vendor expertise before awarding contracts. Standardized credit registries with verifiable trust models make your open source contributions a transparent, auditable competitive advantage in tenders—not just self-reported claims.
- **Access a growing market** — EU public sector IT spending is hundreds of billions annually. Digital sovereignty mandates are steering more of it toward open source solutions with proven, visible vendor ecosystems. Registries and decentralized trust networks make that ecosystem visible and trustworthy.
- **Level the playing field** — small vendors with strong contributions become visible across multiple registries alongside large incumbents. Credit data speaks louder than brand recognition, and decentralized registries prevent gatekeeping.
- **Protect your investment in contributions** — in well-designed credit registries like Drupal's, credits attributed through your contributors remain with your organization even after individual contributors change employers. Your track record reflects cumulative investment, not current headcount. Multiple registries prevent vendor lock-in to a single platform.
- **Participate in transparent usage networks** — deploying organizations voluntarily declare their usage through decentralized registries, creating visibility into installed bases without depending on proprietary market research or vendor lock-in to central catalogs.
- **Ride the legislative wave** — Switzerland's EMBAG, APELL's procurement criteria proposals, and EuroStack's "Buy Open Source Act" are all moving toward requiring verified upstream contributions in tenders. Vendors with participation in standardized credit registries and usage networks are positioned for this future; those without it will scramble to catch up.

---

## Procurement Office

- **Reduce procurement risk** — Evaluate security posture (via supply chain references and SBOMs), vendor ecosystem (via credit registries), peer adoption (via decentralized usage registries), and compliance posture before making a purchase commitment, not after. All evidence is verifiable and auditable, not vendor-provided marketing.
- **Meet digital sovereignty requirements** — Regulations like the EU Cloud Sovereignty Framework (SOV-5, SOV-6) and national mandates (Munich SDS) require supply chain transparency and open source preference. Standardized metadata, decentralized registries, and trust model declarations make compliance verifiable and auditable without centralized gatekeeping.
- **Avoid vendor lock-in** — Identify software with multiple active vendors across decentralized credit registries and strong maintenance culture before you commit, not when a single contractor walks away. Registries operated independently prevent consolidation risks.
- **Use proven selection criteria** — The [OSBA's procurement framework](https://osb-alliance.de/publikationen/veroeffentlichungen/selection-criteria-for-the-sustainable-procurement-of-open-source-software) defines four evaluation criteria (community relationship, upstream contributions, professional support, supply chain security) that map directly to publiccode.yml metadata plus the companion registries and standards. No need to invent evaluation criteria from scratch for each procurement process.

---

## Deploying Organization

- **Declare usage on your own domain** — publish `/.well-known/publiccode-usage.json` with your domain as proof of identity. No registry account needed, no intermediary required. Multiple registries can crawl your declarations independently.
- **Integrate declarations into deployment** — when your deployment process updates the usage declaration automatically, the data stays current without manual effort. Retirement is explicitly signaled when software is decommissioned. Data integrity is maintained through decentralized verification, not central curation.
- **Influence the ecosystem you depend on** — declaring what you use signals demand to maintainers, funders, and vendors through multiple independent registries. Software with many verified declared users attracts more investment and support.
- **Reduce duplication across jurisdictions** — when municipalities, cantons, and agencies can see what peers deploy via decentralized registries with verifiable trust models, they collaborate instead of procuring separately. Shared adoption lowers per-organization cost without requiring a central coordinator.
- **Strengthen your negotiating position** — verified usage data from multiple organizations across independent registries gives collective leverage with vendors and justifies shared maintenance contracts. Registry independence prevents vendors from exerting control over visibility or data.

---

## Federal Authority / Funder

- **Allocate funding where it matters** — identify critical infrastructure gaps by analyzing decentralized usage registries: which widely-deployed software has only one maintainer? Which domains lack open source alternatives entirely? Which vendors need investment to strengthen their ecosystem presence?
- **Implement digital sovereignty policy** — the EU CSF, national sovereignty scores, and open source mandates all require the ecosystem transparency that metadata, credit registries, and usage registries provide. Without decentralized, verifiable infrastructure, policy lacks an evidence base.
- **Measure outcomes** — track whether funding leads to improved security scores (via supply chain references), broader adoption (via usage registries), stronger vendor ecosystems (via credit registries), or more resilient supply chains. Data from independent registries with explicit trust models makes impact measurable and auditable.

---

## Policy Maker / Legislator

- **Turn principles into enforceable criteria** — "Public Money, Public Code" and digital sovereignty mandates need verifiable compliance mechanisms. This metadata makes "prioritize open source" auditable rather than aspirational.
- **Build on proven legislative models** — Switzerland's EMBAG demonstrates that mandating open source release works. The proposed extensions answer the next question: how do procurement offices actually find, evaluate, and maintain the software the law requires them to prefer?
- **Strengthen European digital sovereignty** — the EU CSF's SOV-5 (supply chain) and SOV-6 (open source) objectives require exactly the transparency this metadata provides. Standardized credit and classification data turns sovereignty scores from self-assessments into evidence-based evaluations.
- **Create a level playing field for European vendors** — procurement criteria based on verified upstream contributions (as APELL and EuroStack propose) reward companies that invest in open source, rather than incumbents who simply resell it.

---

## Credit Registry

- **Become procurement infrastructure** — public sector buyers increasingly need vendor expertise data. A standardized API makes your registry part of every EU-wide catalog, not an isolated silo.
- **Build on proven infrastructure** — Drupal's open-source [`contribution_records` module](https://git.drupalcode.org/project/contribution_records) is ticket-system-agnostic and API-ready. New registries can adopt it rather than building credit tracking from scratch.
- **Gain project endorsement** — when projects list your registry in their metadata, it's a public signal of trust that raises your credibility with consumers of your data.
- **Scale without gatekeeping** — any registry can join the ecosystem by publishing a standard manifest. No central authority decides who participates.

---

## Usage Registry

- **Become the authoritative source for your jurisdiction or sector** — Governments need a verifiable inventory of deployed software. A standardized registry API makes your data consumable by every catalog and policy analysis tool in the ecosystem.
- **Enable evidence-based policy** — Usage data drives reuse badges, sovereignty scores, and funding allocation. Without registries, policymakers act on anecdotes instead of evidence.
- **Federate without fragmenting** — Each country, sector, or city can operate its own registry while using a common API standard. This means national data sovereignty combined with international interoperability.

---

## Software Catalog / Crawler

- **Richer data, less integration work** — one metadata format across projects, with machine-readable links to security, compliance, credit, and usage data. No per-source custom parsers.
- **Serve the policy layer** — sovereignty assessments, funding allocation, and procurement decisions all depend on the aggregated view you provide. This positions your catalog as critical infrastructure.
- **Grow your coverage automatically** — the Registry Discovery Standard means new registries join your data pipeline by publishing a manifest, not by negotiating a partnership.
