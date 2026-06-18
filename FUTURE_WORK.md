# Future Work

Speculative directions that are **not** part of the [core proposal](PROPOSAL.md) but are recorded here so they are not lost. Unlike the deferred Improvements that remain in `PROPOSAL.md` (CRA Steward Declaration, Platform Lock-in Declaration) — candidate proposals with concrete schema awaiting consensus — the items here are exploratory: ideas worth keeping on the horizon, with no committed design.

## Linked Data Representation _(Deferred)_

The linked-data ecosystem (CodeMeta, schema.org, Software Heritage) could work with publiccode.yml through [YAML-LD](https://www.w3.org/community/reports/json-ld/CG-FINAL-yaml-ld-20231206/). Crawlers could create linked data from plain YAML by applying a standard context. However, this feature is deferred to reduce complexity. The `url` field and any sanctioned mirrors (see [Improvement 7](PROPOSAL.md#improvement-7-sanctioned-mirror-declarations)) are already dereferenceable identifiers suitable as `schema:sameAs` targets in news article entity markup — enabling any project with a publiccode.yml to be referenced from external sources without requiring a Wikidata entry. Wikidata QIDs, where they exist, are complementary aliases the catalog aggregates; they are enrichments, not prerequisites for visibility.

## Project Presentation Profile _(Deferred)_

publiccode.yml's `description` (`shortDescription`, `longDescription`, `features`) is a deliberately compact, machine-first spine, complemented by link-out fields (`landingURL`, `logo`, `documentation`, screenshots, videos). It is **not** designed to carry the rich, formatted narrative a procurement specialist actually reads when evaluating a tool: prose with embedded images, comparison tables, contextual screenshots, and links. Forcing that content into a YAML scalar would be miserable to author, validate, and diff, and would violate [Design Principle 2](PROPOSAL.md#design-principles) (keep only what changes rarely) and [Principle 4](PROPOSAL.md#design-principles) (capture lookup keys, not bodies of content held elsewhere).

There are three distinct layers at play:

1. **Structured metadata** — publiccode.yml. Machine-first, compact. _Exists today._
2. **Rich project self-presentation** — a "datasheet" or product page: formatted prose, in-context screenshots, feature tables, links. _Not covered, and should not live in the YAML._
3. **Third-party editorial** — catalog-authored descriptions, e.g. [softwarehub.schule](https://softwarehub.schule/) (hand-written catalog entries) and [ossdirectory.com](https://ossdirectory.com/) (LLM prose generated from Wikipedia). _Enrichment._

The observed inefficiency is that layer-3 catalogs re-author layer-2 content from scratch because no project-authored, machine-fetchable presentation artifact exists for them to ingest. A **Project Presentation Profile** would be a thin standard for that artifact: a Markdown/HTML document published at a stable, fetchable location (linked from publiccode.yml, or via a `.well-known` convention consistent with the [`.well-known/publiccode-usage.json`](PROPOSAL.md#organization-level-usage-declarations) direction), with a defined asset base so embedded images and links resolve.

Critically, this artifact is **a starting point, not the final catalog entry**. A catalog runs its own editorial process — with or without LLMs — and the project-authored profile is just **one data source** among several (Wikipedia, news articles, other catalogs, the project's own README and website). The standard's job is therefore twofold:

- make the project-authored profile a **high-quality, machine-fetchable input** to that editorial process, and
- provide **stable project identity** (the publiccode.yml `url` and sanctioned mirrors; see [Principle 4](PROPOSAL.md#design-principles)) so that the editorial outputs across multiple catalogs all anchor back to one project rather than fragmenting into disconnected rewrites.

Layer-3 editorial content is inherently audience- and jurisdiction-specific (softwarehub.schule frames a tool for German schools; an EU-wide catalog frames the same tool differently), so it stays per-catalog by design. The standard does not try to standardize the presentation *content* — only the self-published source artifact and the identity that lets sources be fused and attributed. The boundary between project self-presentation (layer 2) and third-party editorial (layer 3) is the same provenance seam that distinguishes maintainer-asserted from catalog-derived data elsewhere in this proposal (see [Principle 8](PROPOSAL.md#design-principles)).
