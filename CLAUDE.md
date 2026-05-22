# CLAUDE.md

This repo is an Obsidian vault and an open-source schema for a private LLM-maintained knowledge system.

The public repo contains structure only: this file, `README.md`, `.gitignore`, `package.json`, `bin/`, and templates under `schema/`.
The private wiki content lives under `wiki/` and is gitignored. Treat every source, URL, summary, take, spark, index entry, and log entry as private knowledge.

## Core Rules

- Obsidian is the frontend. The LLM edits markdown files.
- GitHub stores structure, not knowledge.
- Use one wiki at launch: `wiki/main/`.
- Use one root `CLAUDE.md` for all behavior.
- Do not add tooling unless the user asks for it or repeated pain justifies it.
- Before creating or editing any file, ask: structure or knowledge?
  - Structure: tracked.
  - Knowledge: gitignored.
- Never ingest private content into tracked files.
- `wiki/main/raw/` holds the original source files. These are the source of truth for every ingested source. The LLM reads them but never modifies or deletes them.
- Source pages are annotation layers, not content copies. They reference the original file via the `file:` frontmatter field and a `## Source` embed. They never duplicate the file's content.
- Do not produce LLM-generated prose summaries on source pages. The `## What's in this source` section is a factual index only — bullet points naming sections, entities, scope, format. The user's interpretation belongs in `## My take`, dated and updatable.

## Page Types

- `source`: an annotation layer for one ingested source. References the original file in `wiki/main/raw/` via the `file:` frontmatter and a `## Source` embed. Contains a factual `## What's in this source` index, optional dated takes, and optional sparks.
- `entity`: a concrete recurring thing: person, organization, product, place, tool, book, project, etc.
- `concept`: a descriptive idea, theme, pattern, term, or framework.
- `position`: an opinionated personal claim or sustained view. Positions link to supporting, conflicting, and related pages.
- `question`: an unresolved question being tracked.

Use lowercase kebab-case filenames.

Examples:

- `wiki/main/sources/2026-05-22-article-title.md`
- `wiki/main/concepts/ai-agents.md`
- `wiki/main/positions/agents-are-workflow-tools-not-coworkers.md`
- `wiki/main/questions/what-makes-a-note-worth-promoting.md`

## Required Wiki Files

Each wiki has:

- `index.md`: content catalog. Read this before any ingest or query.
- `log.md`: chronological append-only record.

Log headings must use:

```md
## [YYYY-MM-DD] ingest | Title
```

Other operation types may use:

```md
## [YYYY-MM-DD] query | Topic
## [YYYY-MM-DD] lint | Scope
```

## Ingest Workflow

When ingesting a source:

1. Read `wiki/main/index.md` first.
2. Read related pages suggested by the index.
3. Read the new source.
4. Identify likely related entities, concepts, positions, questions, and past sources.
5. Check prior dated takes and position pages for:
   - direct contradictions
   - softer tensions
   - evidence that complicates an earlier view
6. Before writing, surface any contradictions or tensions to the user.
7. Keep the original file in `wiki/main/raw/`. Create the source page as an annotation layer:
   - Set the `file:` frontmatter to the path of the original.
   - Add a `## Source` section with `![[../raw/<filename>]]` so the original is embedded/linked from the source page.
   - Populate `## What's in this source` with factual bullets only (sections covered, entities mentioned, scope, format).
   - Do not embed the file's content. Do not delete the file.
8. Prompt for `My take` when useful. The user may skip.
9. Prompt for `Sparks` when useful. The user may skip.
10. Update relevant entity, concept, position, and question pages.
11. Promote a take or spark only if it has grown beyond one source.
12. Update `index.md`.
13. Append `log.md`.
14. Show changed files.
15. Run `git status` and verify private content remains untracked.

A source page must contain these headings:

```md
## Source
## What's in this source
## My take
## Sparks
```

(`## Related pages` and `## Source notes` may also appear but are not required.)

`What's in this source` is a factual index of the source, not a synthesis. Bullet points naming sections, entities, scope, and format. No prose interpretation — that belongs in `## My take`.

`My take` is the user's reaction, dated by month when present:

```md
### 2026-05
```

`Sparks` are brief dated ideas that may point outside the source's domain:

```md
### 2026-05-22
```

If a take or spark is skipped, keep the heading but write:

```md
_None yet._
```

## Promotion Rules

Keep source-page takes scoped to the source.

Create or update a `position` page when the user's view becomes reusable across sources.

Create or update a `concept` page when an idea needs neutral explanation.

Create or update a `question` page when uncertainty remains important.

Example:

- Source take: "This article makes me skeptical that agents should be framed as coworkers."
- Concept: `[[ai-agents]]`
- Position: `[[agents-are-workflow-tools-not-coworkers]]`
- Question: `[[when-do-agents-need-human-review]]`

## Position Pages

Positions are personal claims, not neutral topic summaries.

They should include links to:

- related concepts
- supporting sources
- conflicting or complicating sources
- open questions

This makes them useful in Obsidian graph view as synthesis nodes.

## Query Workflow

When answering a question:

1. Read `wiki/main/index.md`.
2. Read relevant pages.
3. Answer from the wiki.
4. Link to wiki pages used.
5. If the answer creates durable synthesis, suggest filing it as a concept, position, or question page.
6. Do not file query output unless the user approves.

## Lint Workflow

Default cadence: manual, with a suggested lint every 10 ingests.

Check for:

- pages missing from `index.md`
- stale index summaries
- orphan pages
- source pages missing required headings
- contradictions between takes and positions
- repeated concepts without concept pages
- takes or sparks ready for promotion
- inconsistent frontmatter
- filename or link drift

## Privacy Checklist

Before every commit or status report:

- `git status` must not show private source files or wiki content as tracked.
- `wiki/` must remain ignored.
- URLs are private.
- `index.md` and `log.md` are private.
- Source summaries, takes, sparks, and page links are private.
