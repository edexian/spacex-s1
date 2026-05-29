# Source & provenance

This repository is a **navigable, machine- and human-readable rendering** of a public
SEC filing. It reproduces the filing's content as markdown sections, HTML tables, and
figures so it can be browsed and searched like a codebase. The authoritative document
is always the original on SEC EDGAR (linked below).

## Original filing

| | |
|---|---|
| **Issuer** | Space Exploration Technologies Corp. ("SpaceX") |
| **CIK** | 0001181412 |
| **Form** | S-1 (registration statement, Securities Act of 1933) |
| **Filed** | 2026-05-20 |
| **Accession** | 0001628280-26-036936 |
| **Primary document** | `spaceexplorationtechnologi.htm` (~11.5 MB HTML, 308 rendered pages) |
| **Retrieved** | 2026-05-28 |

- **Filing index:** https://www.sec.gov/Archives/edgar/data/1181412/000162828026036936/0001628280-26-036936-index.htm
- **Primary document:** https://www.sec.gov/Archives/edgar/data/1181412/000162828026036936/spaceexplorationtechnologi.htm
- **All SpaceX filings:** https://www.sec.gov/cgi-bin/browse-edgar?action=getcompany&CIK=0001181412&type=S-1

## Why this is OK to host

SEC filings are public records: they are submitted to a U.S. government agency for
the express purpose of public disclosure and are distributed for free public access
via EDGAR. SpaceX filed this document to be read by the public. There is no copyright
notice or redistribution restriction on the filing, and mirroring EDGAR filings is a
long-standing, uncontested practice. This repo adds clear attribution and a link back
to the authoritative source.

## How it was processed

The official primary document is **HTML**. To run it through [edex](https://tryedex.com)
— which converts PDFs into a filesystem-native knowledge base — the pipeline was:

1. Downloaded the primary HTML document and its 82 inline images from EDGAR.
2. Rendered the HTML to a single PDF with headless Chrome.
3. Downsampled the inline images to ≤1400 px to keep the artifact lean. **This only
   affects figure resolution** — text and tables are unchanged. For pixel-perfect
   figures, use the EDGAR original.
4. Ran the PDF through `edex ingest`, which produced the `spacex-s1/` tree
   (markdown sections + HTML tables + PNG figures + a BM25 search index).
5. Captioned every figure with a vision model — the figures are charts, spec
   boxes, and infographics with no embedded captions. Each figure's title,
   description, extracted data, and in-image text were written to its sidecar
   and the search index, so figures are keyword-searchable.

## Disclaimers

- This is a **preliminary** prospectus. It is subject to amendment, and some figures
  (offering price, valuation, share counts, ownership percentages) may be placeholders
  in this version. Always check the latest S-1/A on EDGAR.
- **Nothing here is investment advice** or an offer to sell or a solicitation to buy
  any security.
- This project is **not affiliated with, endorsed by, or sponsored by** SpaceX or the
  U.S. Securities and Exchange Commission.
- **Figure captions and extracted figure data are machine-generated** (a vision model,
  prompted to transcribe only what is printed and never guess) and are **not
  human-verified**. Treat figure numbers as a search aid, not authoritative — confirm
  against the figure image or the EDGAR original.
