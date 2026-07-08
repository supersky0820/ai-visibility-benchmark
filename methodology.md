# Methodology

How the numbers in [`data/benchmark.json`](data/benchmark.json) were collected and aggregated. This mirrors the methodology published on the [interactive version](https://trirankai.com/data/ai-visibility-benchmark) of this benchmark.

## Sample selection

- **101 established SaaS brands** across **23 categories** — project management, CRM, HR, analytics, design, finance, productivity, knowledge base, monitoring, video, automation, developer platforms, forms and more.
- Brands were drawn from a seed list of well-known, publicly marketed SaaS products. **SEO/GEO tools were excluded** from the seed list to avoid self-interest (TriRank operates in that category).
- Only **public information** is involved: brand names and their public domains. No customer data, no private accounts.

## Engine and collection window

- **Engine measured: Perplexity (sonar).** Every number in this dataset was measured on this single engine — no other engine's results are included.
- **Collected: July 2026** (`meta.collectedAt = "2026-07"`), in a single collection run.

## Query design

Three queries per brand, phrased the way a real buyer asks:

1. **Brand-name query** — the brand name itself ("does AI know you?")
2. **Reviews query** — `"[brand] reviews"` ("what does AI read about you?")
3. **Alternatives query** — `"[brand] alternatives"` ("does AI recommend you when buyers compare?")

That is 101 brands × 3 queries = **303 queries**, producing **2,469 structured citations** in total.

## Counting rules

- A brand counts as **cited** on a query only when its **own domain** appears among the engine's **structured citations** for that query. The answer prose is never parsed — no fuzzy text matching, no "brand was mentioned" heuristics.
- Every cited source host is recorded, which is where the source tables (`sources.top`, `sources.reviews`, `sources.alternatives`) come from.
- **Visibility score** per brand = the share of its 3 queries on which its domain was cited (0, 33, 67 or 100). Aggregates of this score appear in `scores` and `categories[].avgScore`.

## Honesty rules

- Failed API calls are recorded as **unknown** and **excluded from every denominator** — never counted as "not cited".
- Rerun rows are deduplicated, keeping the latest attempt.
- In this run, **all 101 brands completed all three queries** (`meta.okBrands = 101`), so every denominator is 101 (or 92 for the alternatives gap, which conditions on brand-name recognition).

## Anonymization

This dataset is **aggregate-only by design**. The point is how the AI recommendation mechanism behaves — not who wins.

- **No per-brand results** are published and no brands are ranked.
- Categories are published only when **≥ 4 brands** were sampled (`meta.minCategoryN`); 13 of 23 categories meet the bar.
- Source hosts are listed only when cited across **≥ 5 different brands** (`meta.minHostBrands`), so no single brand's citation footprint can be reverse-engineered.

## The two source metrics

- **brands%** (coverage breadth) = distinct brands for which a host was cited ÷ 101. Derived from `sources.top[].brands`, which is a **raw count**.
- **share%** (citation share) = a host's citations ÷ all 2,469 citations. Stored directly in `sources.top[].share`.

## Limitations

- **Single engine, single snapshot.** Results describe Perplexity (sonar) in July 2026. Other engines and later dates may differ; this is not a longitudinal study.
- **Structured citations only.** A brand mentioned in the answer text without a structured citation is not counted as cited.
- **SaaS-only sample.** Established brands with real market presence; results may not generalize to other verticals or early-stage products.
- **Category granularity.** 10 of 23 categories fell below the 4-brand anonymity threshold and are aggregated into totals but not broken out.

## Reproducibility

The published JSON is produced from the raw collection run by a deterministic aggregation script; every figure on the interactive page and in this repository derives from `data/benchmark.json`. The raw per-brand run data is intentionally not published (see Anonymization).
