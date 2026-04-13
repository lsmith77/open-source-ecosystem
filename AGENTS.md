# Agent Instructions

Guidelines for AI agents working on this repository.

The PROPOSAL.md is the core document. All other document exist to support this document by adding additional details, perspectives and addressing different audiences.

## Document Index

| File                   | Purpose                                                                                                                                                      |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `README.md`            | Navigation guide; reader paths by role (procurement, maintainer, vendor, legislator, etc.)                                                                   |
| `PROPOSAL.md`          | Core proposal — publiccode.yml schema improvements, design principles, actors, policy context. Must remain self-contained.                                   |
| `RESEARCH.md`          | Comparative analysis of existing metadata standards and initiatives that informed the proposal                                                               |
| `EU_POLICY_CONTEXT.md` | EU legislative initiatives (CRA, NIS2, Interoperable Europe Act, DORA, AI Act, etc.) and how the proposal relates to each. Starting point for policy makers. |
| `PITCH.md`             | Stakeholder-specific pitches (maintainer, vendor, procurement office, legislator, funder, etc.)                                                              |
| `ROADMAP.md`           | Phased implementation plan; ally map; sequencing rationale                                                                                                   |
| `RISK_ANALYSIS.md`     | Risk register across adoption, technical, governance, political, and economic dimensions                                                                     |
| `DATA_FLOW.md`         | Mermaid architecture diagram showing how data flows between ecosystem components                                                                             |
| `GLOSSARY.md`          | Definitions of key terms, acronyms, and concepts used across all documents                                                                                   |
| `METHODOLOGY.md`       | Research provenance log — AI tool usage, phases, prompts, sources evaluated. Update this document whenever doing deep research prompts                       |
| `AGENTS.md`            | This file                                                                                                                                                    |

## URL Validation

**Validate every URL before including it in any file.**

Before writing a URL into any document, fetch it and confirm it returns a non-error response. A URL that cannot be verified must not be added. If the canonical source is a large PDF or otherwise unfetchable or omit the link and note it as unverified.

## Document Conventions

- All documents use GitHub-flavoured Markdown.
- New legislative initiatives belong in `EU_POLICY_CONTEXT.md`, not scattered across other documents.
- Stakeholder organisations (lobby groups, industry coalitions) belong in `ROADMAP.md` as allies, not in `EU_POLICY_CONTEXT.md`.
- `PROPOSAL.md` must remain self-contained; cross-references to other documents are welcome but the proposal must be readable without them.
- Record all research phases in `METHODOLOGY.md` following the established phase format.
