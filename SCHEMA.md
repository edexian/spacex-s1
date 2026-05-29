# Workspace schema

This directory holds PDF-derived markdown that an agent navigates as a filesystem.
Each PDF becomes a `<doc-slug>/` subdirectory laid out as:

```
<doc-slug>/
  README.md            # title, abstract, outline, element registry
  manifest.json        # outline tree + element registry + page->section map
  sections/            # one file per section, flat (numeric prefix encodes hierarchy)
  tables/              # `.html` (raw) + `.md` (caption + metadata) sidecars
  figures/             # `.png` (raw) + `.md` sidecars
  raw/                 # original PDF when available
```

## Section frontmatter

```yaml
---
doc: <slug>
section_id: "3.2"
title: "Scaled Dot-Product Attention"
parent: "03-model-architecture"     # filename stem of parent section, or null
pages: [4, 5]
tokens: 612
tables: [t002]
figures: [f002]
description: "≤2-sentence summary — the BM25 target and the agent's open-or-skip hint"
---
```

## Filename conventions

- Sections: `NN-slug.md` (top level) or `NN.M-slug.md` (subsection). The numeric
  prefix is the `section_id` left-padded for stable sort.
- Tables: `tNNN-slug.{html,md}`.
- Figures: `fNNN-slug.{png,md}`.

Slug = lowercase, ASCII, hyphen-separated.

## How to navigate

1. Read `INDEX.md` (this workspace) to see which docs are available.
2. Read `<doc>/README.md` for a doc's outline.
3. `read_section`, `read_table`, `get_figure` MCP tools fetch specific elements.
4. `grep` for keyword matches, `search` for BM25 ranking, `find_section` for a
   single best entry point.
