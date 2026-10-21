# ArtifactExplorer

Interactive DFIR knowledge graph for Windows forensic artifacts. Browse 292
artifacts across 15 substrates, see how they corroborate each other through
34 Tier-2 convergences, and walk 11 Tier-3 case-study scenarios end-to-end.

**Live site:** https://dfirjett.github.io/ArtifactExplorer/

## What it is

A structured DFIR corpus + interactive viewer built around a three-tier
evidentiary model:

- **Tier 1 — Artifacts.** One entry per forensic container (registry key,
  event-log record, SQLite database, etc.). Documents fields, locations,
  encodings, observations, anti-forensic surface, and bibliographic provenance.
- **Tier 2 — Convergences.** Multi-artifact inferences joined on a shared
  pivot identifier (a SID, LUID, GUID, serial). Encodes how independent
  artifacts corroborate the same forensic claim.
- **Tier 3 — Scenarios.** Case-study walkthroughs with stepwise investigative
  questions, each step naming the artifacts that answer it, the join key
  threading the steps together, and a Casey C-scale strength rating.

The schema is documented field-by-field with academic reasoning + sourced
citations in **[01-SCHEMA.md](01-SCHEMA.md)**.

## Run the viewer locally

```
python tools/serve.py
```

Then open http://localhost:8000/viewer/index.html

## Rebuild the graph from source

Needs Python 3.10+ and the two packages in `requirements.txt`:

```
pip install -r requirements.txt
python tools/validate.py
python tools/build-graph.py
```

`validate.py` checks every corpus file against the JSON schemas in `schema/`.
`build-graph.py` walks `artifacts/`, `convergences/`, `scenarios/`,
`concepts/`, `substrates/` and the `schema/sources.yaml` registry, refuses to
build if any entity lacks a registered source, and emits `viewer/data.json`
(~2.8MB minified) which the viewer loads. Maintainer audit scoring runs only
when the local, git-ignored `tools/crawl_state.yaml` is present.

GitHub Actions runs the same validate + build on every push and pull request
(`.github/workflows/build.yml`), and `.github/workflows/pages.yml` publishes
only the built site (root redirect, `viewer/`, `01-SCHEMA.md`) to GitHub Pages.

## Layout

```
ArtifactExplorer/
├── 01-SCHEMA.md            # Schema reference + academic reasoning + citations
├── README.md
├── CHANGELOG.md
├── artifacts/              # 292 Tier-1 artifact files, by substrate
├── convergences/           # 34 Tier-2 join-chain definitions
├── scenarios/              # 11 Tier-3 case-study walkthroughs
├── concepts/               # Concept (join-key) definitions
├── substrates/             # Substrate-class definitions
├── schema/                 # JSON schemas + sources.yaml (470 entries)
├── docs/
│   ├── architecture.md     # System architecture overview
│   └── nomenclature.md     # Naming conventions
├── topics/                 # Forensic methodology references
├── tools/                  # Build pipeline + analytic scripts
└── viewer/                 # Interactive d3-based force-graph viewer
    ├── index.html
    └── data.json           # Compiled graph data
```

## Licence

- **Code** (`tools/`, `viewer/index.html`, `index.html`, `schema/*.schema.json`,
  `.github/`): MIT, see [LICENSE](LICENSE).
- **Data and documentation** (`artifacts/`, `concepts/`, `convergences/`,
  `scenarios/`, `substrates/`, `topics/`, `docs/`, `schema/*.md`,
  `schema/*.yaml`, `01-SCHEMA.md` and the generated `viewer/data.json`):
  CC BY 4.0, see [LICENSE-CONTENT](LICENSE-CONTENT).

Third-party works listed in `schema/sources.yaml` are cited, not included, and
remain under their own terms.

## Citing

Citation guidance — including APA / MLA / IEEE forms for the schema doc and
the AI-assistance disclosure — is at the bottom of [01-SCHEMA.md](01-SCHEMA.md).
