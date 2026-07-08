# AI Search Visibility of 101 SaaS Brands — Measured on Perplexity

Open dataset: how often an AI answer engine cites 101 established SaaS brands when buyers ask about them — measured on **Perplexity (sonar)**, July 2026. 2,469 structured citations across 303 buyer-style queries, published as an **anonymous aggregate** (no per-brand results, no rankings).

- **Interactive version:** [trirankai.com/data/ai-visibility-benchmark](https://trirankai.com/data/ai-visibility-benchmark)
- **Static charts (GitHub Pages):** `index.html` in this repo
- **Raw aggregate data:** [`data/benchmark.json`](data/benchmark.json)
- **Built by:** [TriRank](https://trirankai.com)

## Key findings

- **91%** of brands are cited on their own brand-name query (92 of 101) — AI clearly *knows* them.
- Only **55%** are cited on "[brand] alternatives" queries (56 of 101) — the question a buyer asks right before choosing.
- **42%** of the brands AI clearly knows **vanish from alternatives answers** (39 of the 92 name-recognized brands were not cited on the alternatives query for their own category).
- About **90%** of everything AI cites is third-party content: only 10.2% of all citations point at brand-owned domains.
- The sources AI leans on most, by **coverage breadth (brands%)**: Reddit **97%**, YouTube **88%**, G2 **72%**, Trustpilot **72%**.
- The same sources by **citation share (share%)**: Reddit **9.1%**, YouTube **8%**, G2 **3.9%**, Trustpilot **3.7%**.

## Two metrics — read this before quoting numbers

The source table is measured two different ways. They answer different questions and are **not interchangeable**:

| Metric | Definition | Question it answers | Where it lives in the JSON |
| --- | --- | --- | --- |
| **brands%** | Number of brands (out of 101) for which a source was cited at least once, **÷ 101** | "How *widely* does AI rely on this source across brands?" | Derived: `sources.top[].brands / meta.brands` (`brands` is a **raw count**, not a percentage) |
| **share%** | A source's citations ÷ all 2,469 citations | "How much of AI's *total sourcing* does this host own?" | Direct: `sources.top[].share` |

Example: Reddit appears for 98 of 101 brands (**97 brands%**) while owning **9.1 share%** of all citations. Same Reddit, two lenses — quote the one you mean.

## Methodology (summary)

Full write-up in [`methodology.md`](methodology.md).

- **Sample:** 101 established SaaS brands across 23 categories (project management, CRM, HR, analytics, design, finance and more). SEO/GEO tools were excluded from the seed list to avoid self-interest.
- **Engine:** Perplexity (sonar). Data collected July 2026.
- **Queries:** three per brand, phrased the way a real buyer asks — the brand name, "[brand] reviews", and "[brand] alternatives".
- **Counting rule:** a brand counts as cited on a query only when its **own domain** appears among the engine's structured citations; the answer prose is never parsed.
- **Honesty rules:** failed calls are recorded as unknown and excluded from every denominator — never counted as "not cited". Rerun rows are deduplicated keeping the latest attempt. In this run, all 101 brands completed all three queries.
- **Anonymity:** only aggregates are published. Categories appear only with 4+ brands sampled; source hosts only when cited across 5+ different brands. No per-brand data is released.

## Data dictionary — `data/benchmark.json`

### `meta`
| Field | Meaning |
| --- | --- |
| `brands` | Brands in the sample (101) |
| `okBrands` | Brands with complete, successful runs included in every aggregate (101) |
| `categoriesTotal` | Categories in the sample (23) |
| `categoriesShown` | Categories published (13) — only those meeting `minCategoryN` |
| `promptsPerBrand` | Queries per brand (3: brand name / reviews / alternatives) |
| `totalCitations` | Structured citations parsed in total (2,469) |
| `engine` | AI answer engine measured — `"Perplexity (sonar)"` |
| `collectedAt` | Collection month, `YYYY-MM` |
| `minCategoryN` | Anonymity threshold: categories with fewer sampled brands are not published (4) |
| `minHostBrands` | Anonymity threshold: hosts cited for fewer distinct brands are not listed (5) |

### `promptRates[]`
Per query type: `key` (`brand` / `reviews` / `alternatives`), `cited` (brands whose own domain was cited), `total` (denominator, 101), `rate` (percent).

### `alternativesGap`
`missed` (brands cited on their brand-name query but **not** on the alternatives query, 39), `of` (brands cited on their brand-name query, 92), `rate` (percent, 42.4).

### `scores`
Visibility score = the share of a brand's 3 queries on which its domain was cited (0–100). `avg` (mean across brands, 67.6), `zero` (brands cited on 0 queries, 5), `full` (cited on all 3, 42), `of` (denominator, 101). `citedOn.dist[]` is the full histogram: `cited` (0–3 queries) → `n` (brands).

### `categories[]`
Per published category: `slug` (data label from the seed list), `n` (brands sampled), `avgScore` (average visibility score), `brandRate` (% cited on brand-name query), `altRate` (% cited on alternatives query).

### `sources`
- `top[]` — hosts across all queries: `host`, `share` (% of all 2,469 citations), `brands` (**raw count** of distinct brands, out of 101, for which the host was cited — divide by 101 for brands%).
- `reviews[]` / `alternatives[]` — top hosts within that query type: `host`, `share` (% of that slot's citations).
- `selfShare` — % of all citations pointing at the measured brand's own domain (10.2).

## Limitations

- Single engine (Perplexity, sonar model) and a single collection window (July 2026) — a snapshot, not a longitudinal study.
- Structured citations only; brands mentioned in answer prose without a citation are not counted.
- SaaS-only sample; other verticals may behave differently.

## License

- **Data** (`data/benchmark.json` and all derived figures): [CC BY 4.0](LICENSE) — free to use and republish **with attribution to "TriRank" and a link to [trirankai.com](https://trirankai.com)**.
- **Code** (`index.html`, scripts): [MIT](LICENSE).

## How to cite

> TriRank (2026). *AI Search Visibility of 101 SaaS Brands — Measured on Perplexity.* https://trirankai.com/data/ai-visibility-benchmark
