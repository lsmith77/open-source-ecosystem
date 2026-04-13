# Glossary of Key Terms

This glossary explains technical and domain-specific terms used throughout the project documentation. It's designed to make the documents accessible to readers from different backgrounds.

## Technical Standards & Acronyms

**Open Source Software (OSS)**
"Open source" is the preferred term throughout this documentation. "OSS" is used as an abbreviation in some standards and legacy documents, but always defined at first use. Alternatives: FOSS (Free and Open Source Software), FLOSS (Free/Libre and Open Source Software). All refer to software released under licenses that allow use, modification, and redistribution.

**SBOM (Software Bill of Materials)**
A complete list of all software components, libraries, and dependencies included in a piece of open source software. Similar to a food label that lists all ingredients. Required by U.S. executive order and increasingly by EU regulations for supply chain security.

**SPDX**
A standardized format for declaring software licenses and components. Maintained by the Linux Foundation. Makes licensing information machine-readable.

**CycloneDX**
An alternative SBOM format focused on software supply chain security. Used by many enterprises and security tools.

**OpenSSF Scorecard**
An automated security audit tool that checks open source projects for common security practices (like code review, CI/CD, signed releases). Produces a 0-10 score. Maintained by the Open Source Security Foundation.

**REUSE**
A standard created by the Free Software Foundation Europe (FSFE) for making per-file licensing transparent. Every source file declares its license, making compliance auditable by machines.

**Cyber Resilience Act (CRA)**
An EU regulation establishing requirements for open source software security and liability. Introduces the "open source software steward"—an organization responsible for maintaining security and handling vulnerabilities in critical software. Requires stewards to publish security policies and software bill of materials (SBOMs).

**NIS2 Directive**
EU regulation requiring organizations in critical sectors (energy, health, transport, government) to assess and manage security risks in their open source software supply chain. For deploying organizations, this means evaluating the security, maintenance, and quality of software before adoption.

**Upstream / Downstream Contributions**
Describes the direction of contributions to open source projects:

- **Upstream**: Contributing code back to the original project (generally considered good practice)
- **Downstream**: Using a project's code but keeping modifications separate

Example: A vendor contributes new features to Drupal (upstream). A hospital deploys Drupal with custom modifications (downstream).

**Trust Model** (for registries)
The method a registry uses to verify the accuracy of information it publishes:

- **Self-reported**: Organizations declare information without registry verification (least verification)
- **Verified-domain**: Registry confirms organization control of a domain before accepting declarations (medium verification)
- **Signed-attestation**: Registry cryptographically verifies signatures on declarations (most verification)

**Bootstrap / Bootstrapping**
Starting a system using existing resources rather than building everything new. Example: "Bootstrap the credit registry using Drupal's existing system" means using Drupal as the foundation rather than building from scratch.

**JSON-LD**
A format that embeds structured data into JSON so that web crawlers and tools can understand it. Enables data interoperability across platforms.

**YAML-LD**
YAML version of JSON-LD. A way to combine YAML's readability with semantic web standards.

**CodeMeta**
A standard schema for research software metadata, emphasizing citation and reproducibility. Built on JSON-LD and schema.org.

**Open Source Taxonomy (OSS Taxonomy)**
A multi-dimensional classification system for open source projects (domain, function, technology, role, layer, audience). Designed to enable rich, faceted discovery and gap analysis across the ecosystem. Sometimes called "OSS Taxonomy" in external sources.

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

For **credit registries**, the operator can be the OSS project itself, a generic third-party platform (e.g., GitHub Sponsors), or a SaaS provider. In all cases the project endorses the registry by listing it in `creditRegistries` — this endorsement is what gives the data authority. The sign-off process varies: some projects simply point at an existing platform URL; others (like Drupal) define a formal process for reviewing and approving credits.

For **usage registries**, the operator is typically an independent organization (a national catalog, a sector registry) and has no endorsement relationship with the projects it tracks — deploying organizations declare their own usage.

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

## Repository-Level Policy File Conventions

A growing pattern of named files in repository roots that make a specific policy dimension legible to humans and tools. None of these have reached formal standard status, but they are gaining traction through platform support and community adoption.

**SECURITY.md** - A de facto convention popularized by GitHub, which renders it as a "Security policy" tab in a repository. Contains a project's vulnerability disclosure policy and contact information for reporting security issues. Has no formal specification — content is free-form prose. The more rigorous machine-parseable alternative is RFC 9116 `security.txt`.

**RFC** - Request for Comments. Internet Engineering Task Force (IETF) standard documents that formally specify how internet systems should work. RFCs are numbered (e.g., RFC 9116) and become the official specifications that tools and systems implement.

**RFC 9116 `security.txt`** - An IETF standard ([RFC 9116](https://www.rfc-editor.org/rfc/rfc9116)) defining a structured file at `/.well-known/security.txt` for declaring vulnerability disclosure contacts, preferred languages, policy URLs, and expiry dates for a domain. Unlike SECURITY.md, its format is formally specified and machine-parseable. Relevant to CRA steward obligations for vulnerability handling.
