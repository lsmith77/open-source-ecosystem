# Ecosystem Architecture: Data Flow

This diagram illustrates how the decentralized ecosystem works: how open source projects, registries, and data sources connect to feed procurement offices, researchers, and other stakeholders with comprehensive information about software quality, vendor expertise, and adoption.

```mermaid
graph LR
    subgraph Sources["📦 Data Sources"]
        direction TB
        SEC["SBOM, Advisories,<br/>OpenSSF Scorecard"]
        ORG["Deploying<br/>Organizations"]
        OPEN_SOURCE["Open Source<br/>Projects"]
        OPEN_SOURCE --> SEC --> ORG
    end

    subgraph Registries["📚 Registries"]
        direction TB
        CREDIT["Credit<br/>Registries"]
        USAGE["Usage<br/>Registries"]
        CREDIT --> USAGE
    end

    subgraph Catalogs["📋 Catalogs"]
        direction TB
        CATALOG_EU["EU OSS<br/>Catalogue"]
        CATALOG_CH["Switzerland<br/>Code Catalog"]
        CATALOG_DE["openCode.de"]
        CATALOG_MORE["+ others"]
        CATALOG_EU --> CATALOG_CH --> CATALOG_DE --> CATALOG_MORE
    end

    subgraph Users["👥 Decision Makers"]
        direction TB
        PROCUREMENT["Procurement<br/>Offices"]
        POLICY["Policy Makers"]
        FUNDERS["Funders"]
        RESEARCHERS["Researchers"]
        USER_MORE["+ others"]
        PROCUREMENT --> POLICY --> FUNDERS --> RESEARCHERS --> USER_MORE
    end

    OPEN_SOURCE -->|Contributes to| SEC
    Sources -->|Contributes to| Registries
    Sources -->|Crawled by| Catalogs
    Registries -->|Crawled by| Catalogs
    Catalogs -->|Filter, Interpret<br/>& Verify| Users

    linkStyle 0,1,2,3,4,5,6,7,8,9 stroke:none

    style OPEN_SOURCE fill:#e1f5ff
    style SEC fill:#fff3e0
    style ORG fill:#e8f5e9
    style CREDIT fill:#f3e5f5
    style USAGE fill:#f3e5f5
    style CATALOG_EU fill:#c8e6c9
    style CATALOG_CH fill:#c8e6c9
    style CATALOG_DE fill:#b3e5fc
    style CATALOG_MORE fill:#e0e0e0
    style PROCUREMENT fill:#ffe0b2
    style POLICY fill:#ffe0b2
    style FUNDERS fill:#ffe0b2
    style RESEARCHERS fill:#ffe0b2
    style USER_MORE fill:#e0e0e0
```

_Color coding: blue = open source projects; orange = external security data; green = deploying organizations; purple = registries; darker green / light blue = catalogs; orange = decision makers; gray = "others" placeholders._

## Architecture Overview

The core design is **publish-once, discover-many**: content producers (projects, registries, deploying organizations) publish standardized data in `publiccode.yml`, registry endpoints, and `/.well-known/` declarations without knowing which catalogs exist. Each catalog independently discovers, filters, interprets, and presents data according to its own stakeholders and policies — no catalog controls another, no central authority decides which registries count. All catalogs access the same source data; differentiation comes from interpretation, ranking, and policy application, not privileged data access.

For a full description of each actor's role and the design principles behind the architecture, see [PROPOSAL.md](PROPOSAL.md).

---

See [PROPOSAL.md](PROPOSAL.md) for technical specifications of the extensions, APIs, and standards.
See [ROADMAP.md](ROADMAP.md) for implementation phases.
