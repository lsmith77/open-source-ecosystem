# Glossary of Key Terms

This glossary explains technical and domain-specific terms used throughout the project documentation. It's designed to make the documents accessible to readers from different backgrounds.

## Technical Standards & Acronyms

**SBOM (Software Bill of Materials)**
A complete list of all software components, libraries, and dependencies included in a piece of software. Similar to a food label that lists all ingredients. Required by U.S. executive order and increasingly by EU regulations for supply chain security.

**SPDX**
A standardized format for declaring software licenses and components. Maintained by the Linux Foundation. Makes licensing information machine-readable.

**CycloneDX**
An alternative SBOM format focused on software supply chain security. Used by many enterprises and security tools.

**OpenSSF Scorecard**
An automated security audit tool that checks open source projects for common security practices (like code review, CI/CD, signed releases). Produces a 0-10 score. Maintained by the Open Source Security Foundation.

**REUSE**
A standard created by the Free Software Foundation Europe (FSFE) for making per-file licensing transparent. Every source file declares its license, making compliance auditable by machines.

**JSON-LD**
A format that embeds structured data into JSON so that web crawlers and tools can understand it. Enables data interoperability across platforms.

**YAML-LD**
YAML version of JSON-LD. A way to combine YAML's readability with semantic web standards.

**CodeMeta**
A standard schema for research software metadata, emphasizing citation and reproducibility. Built on JSON-LD and schema.org.

**OSS Taxonomy**
A multi-dimensional classification system for open source projects (domain, function, technology, role, layer, audience). Designed to enable rich, faceted discovery and gap analysis across the ecosystem.

---

## Organizational & Policy Concepts

**"Public Money, Public Code"**
A campaign and principle asserting that software developed or purchased with public funds should be released as open source. If taxpayers funded it, the code should be publicly available.

**EMBAG (Switzerland)**
Switzerland's federal law (enacted 2023) mandating that software developed or substantially customized for the Swiss government be released as open source.

**Digital Sovereignty**
Control over one's own technology and data. In the context of open source: reducing dependence on foreign vendors, using open standards, maintaining ability to audit and modify software, and protecting sensitive data from unnecessary trans-border flows.

**Supply Chain Security**
Ensuring that software and its dependencies are trustworthy and haven't been tampered with. Involves verifying download integrity, checking for known vulnerabilities, and understanding who maintains each component.

**Procurement**
The process by which organizations purchase goods or services. In the context of software: the formal process of identifying, evaluating, and selecting software and related vendor services.

---

## Ecosystem Actors

**Open Source Project / Maintainer**
The person or team that creates and manages an open source software project. They decide what to publish, how to license it, and what contributions are accepted.

**Vendor / Service Provider**
A company or consultant that contributes to open source projects and/or sells expertise and support around them. Often the people hired by organizations to help deploy and maintain open source software.

**Procurement Office**
The team within an organization (usually government or large enterprise) responsible for finding and purchasing software. They evaluate options, negotiate contracts, and make buying decisions.

**Deploying Organization**
An organization that uses software in production (e.g., a city government running Drupal, a hospital using an open source EHR system).

**Funder**
An organization that gives money to support open source development (e.g., Sovereign Tech Fund, CISA, Sloan Foundation).

**Registry Operator**
A person or organization that runs a registry—a database that tracks specific information about projects. Examples: Credit registry (who contributes), Usage registry (who uses it).

**Crawler / Catalog**
A tool or service that automatically finds and indexes information about open source projects. Examples: GitHub search, libraries.io, EU Open Source Catalogue.

---

## Governance & Architecture Concepts

**Federated Architecture**
A system where multiple independent nodes (countries, organizations, registries) participate without central control. Each runs its own system but using a shared standard so data can be aggregated. Example: national open source catalogs (Italy, Germany) feeding into the EU catalogue.

**Registry**
A database or service that tracks specific information about projects:

- **Credit Registry**: Tracks who contributes and how much
- **Usage Registry**: Tracks which organizations use which software
- **Package Registry**: Hosts downloadable packages (npm, PyPI, Maven, etc.)

**API**
Application Programming Interface. A standard way for different software systems to request and share data. Think of it as a plug that any compatible tool can use.

**Conformance**
Meeting a standard. A registry that "conforms to the Registry API" implements all required fields and behaviors so that any crawler can use it.

**Governance**
How decisions are made about a standard: who maintains it, how changes get approved, who participates in decisions. For standards to remain trustworthy, governance must be transparent and inclusive.

---

## Procurement & Policy Concepts

**OSBA (Open Source Business Alliance)**
A German industry association for companies working in or around open source. Publishes procurement selection criteria and advocacy positions.

**APELL**
A European policymaking network focused on digital sovereignty and open source in public procurement.

**EuroStack**
A coalition of 260+ European companies advocating for "Buy Open Source Act"—legislation to require public procurement to prefer open source software.

**FSFE (Free Software Foundation Europe)**
A nonprofit advocacy organization focused on digital freedom and open source. Runs campaigns like "Public Money, Public Code" and maintains the REUSE standard.

**CISA (U.S. Cybersecurity and Infrastructure Security Agency)**
The U.S. federal agency responsible for critical infrastructure security. Funds security research and maintains vulnerability databases.

**Sovereign Tech Fund**
A German funding program supporting critical open source infrastructure, with focus on security and sustainability.

**EU Cloud Sovereignty Framework**
A European Commission framework for assessing whether cloud services meet EU data protection, security, and digital sovereignty requirements. Explicitly encourages open source.

---

## Metadata Concepts

**Metadata**
"Data about data." Information that describes something else. For software: metadata includes the version number, who maintains it, what license it uses, what other software it depends on.

**Faceted Classification**
A system for organizing information across multiple independent dimensions. Example: a movie could be classified by genre (comedy, drama, action) AND language (English, Spanish, French) simultaneously. This is more expressive than a flat list where you pick one.

**Temporal Data**
Information that changes over time. In software: version numbers, release dates, dependency version minimums. These are "temporal" because they change with every release.

**Slow-Changing Data**
Information that changes rarely, if ever. In software: classification (what domain it serves), license, contact information, maintenance arrangements. These don't require updating for every release.

**Authoritative Source**
The definitive, most-accurate place to find information. For software version numbers: the GitHub releases page or package registry is authoritative, not a metadata file that nobody remembers to update.

---

## Use Cases Referenced

**Procurement Discovery**
Help procurement officers find open source software that meets their needs. Example: "Show me all CMS tools suitable for public administrations that run as web applications."

**Vendor Evaluation**
Assess whether a vendor has genuine expertise in a software project. Do they actually contribute code? How long have they been involved? Are they trusted by the project maintainers?

**Supply Chain Assessment**
Evaluate whether software is secure and maintainable. Does it have known vulnerabilities? Are dependencies well-maintained? Who controls the project?

**Ecosystem Gap Analysis**
Identify which areas lack open source options or lack adequate funding. Example: "Which widely-used functions in healthcare IT lack maintained open source alternatives?"

---

## Related Standards (Brief References)

**CITATION.cff** - Format for software citation metadata (author, version, publication date). Lightweight complement to CodeMeta.

**schema.org** - Web standard for structured data. Provides shared vocabulary for everything from recipes to software. Many metadata standards build on schema.org.

**Software Heritage** - Non-profit long-term archive of all public source code. Accepts deposits in CodeMeta format.

**Software Heritage SoftwareSourceCode** - The metadata model used to describe software in the Software Heritage archive.
