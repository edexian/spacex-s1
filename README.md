# SpaceX S-1 — navigable like a codebase

> The largest IPO in history is a 308-page PDF. This repo turns it into a filesystem you can **`grep`, search, and read section-by-section** — no vector database, no chunking, no embeddings.

On **2026-05-20**, Space Exploration Technologies Corp. ("SpaceX") filed its **Form S-1** — the prospectus for what is set to be the largest IPO ever. It's 308 pages of dense financial and technical disclosure.

This repository *is* that filing, run through **[edex](https://tryedex.com)** — a tool that converts a PDF into a navigable tree of markdown sections + HTML tables + PNG figures + a BM25 search index. You (or an AI agent) browse it the way you'd browse a codebase: `ls`, `grep`, open the file.

## What's inside

| | |
|---|---|
| **695** sections | `spacex-s1/sections/` — one markdown file each, with YAML frontmatter: page numbers, token count, and a one-line summary |
| **151** tables | `spacex-s1/tables/` — HTML (merged cells preserved) + markdown sidecars |
| **85** figures | `spacex-s1/figures/` — PNG |
| BM25 search | rebuilt locally from the markdown (the binary index isn't committed); `grep` + section reading need no index |
| full machine outline | `spacex-s1/README.md`, `spacex-s1/manifest.json` |

## Try it — it's just files

```bash
# every section that mentions Starlink
grep -rl "Starlink" spacex-s1/sections/

# find SpaceX's plan to put AI datacenters in orbit
grep -ril "orbital" spacex-s1/sections/

# read the risk-factor summary
cat spacex-s1/sections/37-summary-of-risk-factors.md
```

## Or point your agent at it (edex over MCP)

```text
list_documents()
get_outline("spacex-s1")
search("orbital AI compute", doc="spacex-s1")
read_section("spacex-s1", "284")   # the "AI" section
read_table("spacex-s1", "t005")    # summary financials
```

## A few things buried in 308 pages

- **FY2025 revenue $18,674M** (up from $14,015M in 2024 and $10,387M in 2023); **net loss $(4,937)M** — `read_table t005` (`tables/t005-table-5.html`)
- SpaceX's **"orbital AI"** thesis — moving power-hungry AI compute into space for near-constant solar power, calling itself "the only company with a commercially viable path to building orbital AI compute at scale" — `sections/284-ai.md`
- **Starship V3** expected to carry **100 metric tons** to orbit, reusably — `sections/284-ai.md`
- the **Brazil Asset Seizure** risk factor (a foreign court froze Starlink's assets) — `sections/60-the-global-nature-of-our-business-poses-risks-with-respect-to-unstable.md`

## Key entry points

| Section | File |
|---|---|
| Prospectus Summary | `sections/12-prospectus-summary.md` |
| Summary of Risk Factors | `sections/37-summary-of-risk-factors.md` |
| Use of Proceeds | `sections/79-use-of-proceeds.md` |
| Dilution | `sections/82-dilution.md` |
| Management's Discussion & Analysis | `sections/83-management-s-discussion-and-analysis-of-financial-condition-and-results-of.md` |
| Summary financials (table) | `tables/t005-table-5.html` |

## Source · license · disclaimers

Full provenance in **[SOURCE.md](SOURCE.md)**. In short: this is a **public SEC filing** (a public record, freely redistributable), retrieved from EDGAR on 2026-05-28. It is a **preliminary** prospectus, subject to amendment — some figures may be placeholders. **Nothing here is investment advice.** This project is **not affiliated with or endorsed by** SpaceX or the SEC.

## How it was made

The official document is HTML; it was rendered to PDF and run through `edex ingest`. Because EDGAR's HTML uses visual styling rather than semantic headings, the section outline is **flat and page-anchored** (not nested) — `grep` and BM25 `search` are the primary way in, and every section carries its page number. See [SOURCE.md](SOURCE.md) for the exact pipeline.

---

Built with **[edex](https://tryedex.com)** — turn any PDF into a filesystem-native knowledge base.
