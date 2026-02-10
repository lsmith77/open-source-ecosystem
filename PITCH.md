# Stakeholder Pitches

Short pitches for each stakeholder in the [proposal](PROPOSAL.md), focused on economic and political motivations.

---

## OSS Project / Maintainer

- **Reach the public sector market** — EU procurement is shifting to open source (EU CSF, openCode.de, Developers Italia). One metadata file puts your project in front of buyers across 27 member states.
- **Attract funding and contributors** — projects with visible adoption data and classified dependencies are easier for funders (Sovereign Tech Fund, CISA) to justify investing in.
- **Control your narrative** — you decide how the project is classified and which credit registries you endorse, rather than letting third-party catalogs guess.
- **Make adoption visible by default** — integrate usage declaration updates into your deployment documentation so that every deploying organization automatically signals adoption. More visible adoption means more funding, more contributors, and more leverage.

---

## Vendor / Service Provider

- **Win public sector contracts** — procurement offices increasingly evaluate vendor expertise before awarding contracts. Standardized credit data makes your open source contributions a competitive advantage in tenders.
- **Access a growing market** — EU public sector IT spending is hundreds of billions annually. Digital sovereignty mandates are steering more of it toward open source solutions with proven vendor ecosystems.
- **Level the playing field** — small vendors with strong contributions become visible alongside large incumbents. Credit data speaks louder than brand recognition.

---

## Procurement Office

- **Reduce procurement risk** — evaluate security posture, vendor ecosystem, and adoption by peer organizations before committing, not after.
- **Meet digital sovereignty requirements** — the EU Cloud Sovereignty Framework (SOV-5, SOV-6) and national mandates (Munich SDS) require supply chain transparency and open source preference. This metadata makes compliance auditable.
- **Avoid vendor lock-in** — identify software with multiple contributing vendors and active maintenance before you buy, not when a sole contractor walks away.

---

## Deploying Organization

- **Declare usage on your own domain** — publish `/.well-known/publiccode-usage.json` with your domain as proof of identity. No registry account needed, no intermediary required.
- **Integrate declarations into deployment** — when your deployment process updates the usage declaration automatically, the data stays current without manual effort. Retirement is explicitly signaled when software is decommissioned.
- **Influence the ecosystem you depend on** — declaring what you use signals demand to maintainers, funders, and vendors. Software with many declared users attracts more investment and support.
- **Reduce duplication across jurisdictions** — when municipalities, cantons, and agencies can see what peers deploy, they collaborate instead of procuring separately. Shared adoption lowers per-organization cost.
- **Strengthen your negotiating position** — verified usage data from multiple organizations gives collective leverage with vendors and justifies shared maintenance contracts.

---

## Federal Authority / Funder

- **Allocate funding where it matters** — identify critical infrastructure gaps: which widely-deployed software has only one maintainer? Which domains lack open source alternatives entirely?
- **Implement digital sovereignty policy** — the EU CSF, national sovereignty scores, and open source mandates all require the ecosystem transparency this metadata provides. Without it, policy lacks an evidence base.
- **Measure outcomes** — track whether funding leads to improved security scores, broader adoption, or stronger vendor ecosystems. Usage and credit data make impact measurable.

---

## Credit Registry

- **Become procurement infrastructure** — public sector buyers increasingly need vendor expertise data. A standardized API makes your registry part of every EU-wide catalog, not an isolated silo.
- **Gain project endorsement** — when projects list your registry in their metadata, it's a public signal of trust that raises your credibility with consumers of your data.
- **Scale without gatekeeping** — any registry can join the ecosystem by publishing a standard manifest. No central authority decides who participates.

---

## Usage Registry

- **Become the authoritative source for your jurisdiction** — governments need to know what software is deployed in their sector. A standardized registry API makes your data consumable by every catalog and policy tool in the ecosystem.
- **Enable evidence-based policy** — usage data drives reuse badges, sovereignty scores, and funding decisions. Without registries, these initiatives rely on anecdotes.
- **Federate without fragmenting** — each country or sector runs its own registry, but a common API means crawlers aggregate data globally. National sovereignty over data, international interoperability of the format.

---

## Software Catalog / Crawler

- **Richer data, less integration work** — one metadata format across projects, with machine-readable links to security, compliance, credit, and usage data. No per-source custom parsers.
- **Serve the policy layer** — sovereignty assessments, funding allocation, and procurement decisions all depend on the aggregated view you provide. This positions your catalog as critical infrastructure.
- **Grow your coverage automatically** — the Registry Discovery Standard means new registries join your data pipeline by publishing a manifest, not by negotiating a partnership.
