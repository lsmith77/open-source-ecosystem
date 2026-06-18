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
| `TOOLING.md`           | Catalog of the publiccode.yml tooling ecosystem, gaps, and OSS building blocks (existing tools, adjacent data sources, and the new/adapted tools the proposal needs) |
| `GLOSSARY.md`          | Definitions of key terms, acronyms, and concepts used across all documents                                                                                   |
| `FUTURE_WORK.md`       | Exploratory directions not part of the proposal (deferred, no committed design). Deferred Improvements with concrete schema stay in `PROPOSAL.md`.            |
| `METHODOLOGY.md`       | Research provenance log — AI tool usage, phases, prompts, sources evaluated. Update this document whenever doing deep research prompts                       |
| `AGENTS.md`            | This file                                                                                                                                                    |

## URL Validation

**Validate every URL before including it in any file.**

Before writing a URL into any document, fetch it and confirm it returns a non-error response. A URL that cannot be verified must not be added. If the canonical source is a large PDF or otherwise unfetchable or omit the link and note it as unverified.

## Design Principles

The [Design Principles](PROPOSAL.md#design-principles) are the backbone of the proposal. Check every new or changed field, mechanism, or companion standard against them; if a change conflicts, rework it or make a documented case for revising the principle.

- **The list holds only genuine principles** — cross-cutting design rules. Spec details belong in the relevant Improvement, operational requirements in governance/catalog sections, deferred features in Future Work. Prefer fewer, sharper principles.
- **Keep numbered cross-references in sync** (e.g. "Design Principle 4") when reordering, adding, or removing a principle.

## Document Conventions

- All documents use GitHub-flavoured Markdown.
- New legislative initiatives belong in `EU_POLICY_CONTEXT.md`, not scattered across other documents.
- Stakeholder organisations (lobby groups, industry coalitions) belong in `ROADMAP.md` as allies, not in `EU_POLICY_CONTEXT.md`.
- `PROPOSAL.md` must remain self-contained; cross-references to other documents are welcome but the proposal must be readable without them.
- Record all research phases in `METHODOLOGY.md` following the established phase format.
- Avoid hard counts in prose ("both", "the two seams", "23 issues", "all three") — they go stale when items are added or removed and create needless git-history churn. Refer to items by name/title instead, and let lists speak for themselves. Intrinsic counts that name a fixed concept (e.g. "two faces of one status") are fine.
- Reference tickets/issues by **title** and, once filed, by their real issue number — don't invent a parallel synthetic ID scheme (Issue A/B/C, T1–T5) that has to be kept in sync.
- Tag open questions that need a specific external review **inline, in context** with a single faceted marker `> ⚠️ OPEN[<facet>]: <question>` (collect with `grep -rn 'OPEN\['`). Add a facet only for a genuine external-review handoff (e.g. `legal`); do **not** create topic markers (SECURITY/INFRASTRUCTURE/A11Y) — topic coverage belongs in the relevant section, not in a parallel tagging system.
