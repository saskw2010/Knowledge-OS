# WytSky / Sky365 Repository Documentation Standard

**Status:** Proposed reusable repository standard.

## Purpose

Every important WytSky / Sky365 repository should be self-describing for humans and coding agents before implementation work begins. The repository should expose both its machine-readable/source documentation and its visual/published documentation without duplicating canonical content manually.

## Minimum documentation skeleton

```text
project/
├── README.md
├── MASTER-ROADMAP.md
├── PROJECT_MEMORY.md
├── index.html
├── decisions/
├── docs/
│   ├── INDEX.md
│   ├── repository-map.html
│   ├── viewer.html
│   ├── architecture/
│   ├── infographics/
│   └── research/
├── schemas/
├── prompts/
├── data/
└── src/
```

Not every repository must contain every directory on day one. The standard is a navigation contract, not a requirement to create empty folders.

## Required roles

- **README.md** — immediate orientation, architecture summary, start-here order, and repository map.
- **MASTER-ROADMAP.md** — program-level scope, workstreams, milestones, dependencies, and future layers.
- **PROJECT_MEMORY.md** — compact operational state, accepted decisions, blockers, current work queue, and rules.
- **docs/INDEX.md** — canonical documentation navigation layer.
- **docs/repository-map.html** — visual repository explorer that distinguishes file types and opens the correct viewing experience.
- **docs/viewer.html** — rendered Markdown viewer for GitHub Pages. Markdown remains the source; HTML is generated at view time.
- **index.html** — visual landing page for humans. It should link to the repository explorer, roadmap, current state, major architecture areas, and visual documentation.
- **decisions/** — architecture/governance decision records.
- **schemas/** — executable machine-readable contracts when applicable.

## File-type visual contract

Repository explorers should visually distinguish these asset classes:

| Type | Role | Suggested visual treatment |
|---|---|---|
| Markdown (`.md`) | Canonical documentation and source-of-truth prose | Blue / document icon |
| HTML (`.html`) | Visual or interactive published pages | Cyan/green / web icon |
| JSON/Data (`.json`) | Machine-readable records and datasets | Orange / data icon |
| JSON Schema (`*.schema.json`) | Executable contracts | Purple / schema icon |
| KDR / Decision | Architecture and governance authority | Gold / decision icon |
| Prompt | Controlled AI instruction asset | Pink / brain icon |
| Interactive explorer | Browser/editor/visual graph experience | Bright green / interactive icon |

## Markdown-to-HTML rule

Do **not** maintain a handwritten HTML twin for every Markdown document.

Preferred pattern:

```text
Canonical Markdown
      ↓
Markdown renderer
      ↓
Knowledge-OS / WytSky HTML theme
```

A shared `viewer.html` may use a Markdown parser such as `marked.js` or `markdown-it`, HTML sanitization such as `DOMPurify`, and syntax highlighting such as `highlight.js`.

Native HTML should be reserved for pages that are inherently visual or interactive: explorers, dashboards, diagrams, infographics, simulations, or custom layouts.

## Repository Explorer rule

The explorer should expose:

1. A live or generated repository tree.
2. Search by path/file name.
3. Filtering by file type.
4. Distinct actions for Markdown and HTML.
5. Direct GitHub source links.
6. An **Agent Start Here** section.
7. Links to roadmap, project memory, decisions, schemas, and visual pages.

For public GitHub repositories, a lightweight implementation may read the GitHub Trees API from the browser so the explorer updates automatically as the repository changes.

## Coding-agent start order

```text
README.md
  ↓
MASTER-ROADMAP.md
  ↓
PROJECT_MEMORY.md
  ↓
Accepted decisions / KDRs
  ↓
docs/INDEX.md + relevant canonical docs
  ↓
Relevant schemas/contracts
  ↓
Implementation
```

Agents should not reconstruct accepted architecture from chat history when the repository already contains source-of-truth documents.

## Source-of-truth principle

HTML navigation and visual pages improve discovery; they do not override canonical authority. If a visual page conflicts with an accepted KDR, schema, or canonical Markdown specification, the accepted source-of-truth hierarchy wins.

## Adoption rule

Before substantial implementation begins in a new WytSky / Sky365 repository:

1. Create or validate the minimum orientation files.
2. Publish a repository explorer.
3. Define source-of-truth order.
4. Make Markdown and HTML documentation discoverable together.
5. Only then begin architecture-changing implementation work.

This standard should be copied and adapted across repositories rather than re-designed independently each time.
