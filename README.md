# Personal Knowledge Base

A reusable structure for building a private, LLM-maintained knowledge base in Obsidian.

The core idea is that an LLM should not repeatedly rediscover your knowledge from raw files. Instead, it incrementally maintains a persistent wiki: source pages, concept pages, entity pages, position pages, question pages, an index, and a log. The wiki compounds over time.

This repository contains the public structure only. Your actual knowledge stays private.

## Quick Start

Create or `cd` into the folder you want to use as your vault, then run:

```sh
npx github:ClaudiusMa/personal-knowledge-base init
```

This initializes the current directory. To initialize a different path instead, pass it as an argument:

```sh
npx github:ClaudiusMa/personal-knowledge-base init ~/somewhere-else
```

Then open the vault folder in Obsidian.

## Privacy Model

GitHub holds structure, not content.

Private content lives under:

```text
wiki/
```

That folder is ignored by git. Private content includes:

- raw source files
- source URLs
- source summaries
- personal takes
- sparks
- entity pages
- concept pages
- position pages
- question pages
- `index.md`
- `log.md`

The `.gitignore` uses a whitelist approach: ignore everything by default, then explicitly allow only the public structure files.

## What Is Tracked

```text
.
├── CLAUDE.md
├── README.md
├── LICENSE
├── .gitignore
├── package.json
├── bin/
│   └── personal-knowledge-base.mjs
└── schema/
    ├── gitignore.template
    ├── obsidian/
    │   ├── app.json
    │   └── graph.json
    └── templates/
        ├── index.template.md
        ├── log.template.md
        ├── source.template.md
        ├── entity.template.md
        ├── concept.template.md
        ├── position.template.md
        └── question.template.md
```

## What Is Ignored

```text
wiki/
└── main/
    ├── index.md
    ├── log.md
    ├── raw/          # original source files (the source of truth)
    ├── sources/
    ├── entities/
    ├── concepts/
    ├── positions/
    └── questions/
```

The repo root is also the Obsidian vault root. Obsidian sees the private wiki, but git does not track it.

## Page Types

- `source`: an annotation layer for one ingested source. References the original file in `wiki/main/raw/` via the `file:` frontmatter and a `## Source` embed. Contains a factual `## What's in this source` index, the user's `## My take` (dated, updatable), and `## Sparks`. The original file is the source of truth; the source page never duplicates its content.
- `entity`: a concrete recurring thing
- `concept`: a neutral idea, term, pattern, or framework
- `position`: a personal claim or sustained view
- `question`: an unresolved question being tracked

Position pages are intentionally separate from concept pages. Concepts describe ideas. Positions capture what the owner currently believes about those ideas.

## Workflows

### Ingest

The LLM reads the private index, reads related pages, processes a new source, checks for contradictions with earlier takes and positions, creates or updates pages, updates the index, and appends the log.

### Query

The LLM reads the private index first, then relevant pages, then answers from the wiki. Durable synthesis can be promoted into a concept, position, or question page when the user approves.

### Lint

The LLM periodically checks for stale pages, missing links, contradictions, orphan pages, missing index entries, and takes or sparks that should be promoted.

## Obsidian Defaults

The initializer seeds `.obsidian/app.json` with a `userIgnoreFilters` list so that agent-facing structural files do not pollute the Obsidian UI. By default, these paths are excluded from graph view, quick switcher, search, and link autocomplete:

- `CLAUDE.md`
- `README.md`
- `LICENSE`
- `package.json`
- `.gitignore`
- `schema/`
- `bin/`

To edit this list inside Obsidian, go to **Settings → Files and links → Excluded files**. `.obsidian/` is gitignored, so per-vault edits stay local and are never pushed to GitHub. If `.obsidian/app.json` already exists in a vault, the initializer leaves it untouched.

The initializer also seeds a `.obsidian/graph.json` with a default search filter that hides files in `wiki/main/raw/` from graph view. This prevents duplicate nodes appearing in the graph (one for the original file, one for the source page that annotates it). Raw files remain fully searchable, openable, and embeddable — they are only hidden from the visual graph.

## Using This With an LLM Agent

Point your LLM agent at the vault and tell it to follow `CLAUDE.md`. The schema is intentionally plain markdown. No Obsidian plugins, databases, vector search, or static-site tooling are required at the start.

Start with one broad wiki. Add structure only when real friction appears.

## License

MIT
