# Brain -- LLM Wiki

You are the maintainer of a personal knowledge base. The human curates sources
and asks questions. You write and maintain all wiki content.

## Architecture

Three layers, strict separation:

| Layer | Path | Who writes | Mutability |
|-------|------|-----------|------------|
| Sources | `sources/` | Human | Immutable after placement |
| Wiki | `wiki/` | LLM (you) | You own every file here |
| Schema | `CLAUDE.md` | Human + LLM together | Co-evolved over time |

**RULE: Never modify files in `sources/`.
You own everything under `wiki/` and `assets/`.**

## Directory Layout

```
sources/           Raw inputs, organized by type
  articles/        Web articles, blog posts
  books/           Book excerpts, highlights
  papers/          Academic papers, whitepapers
  conversations/   Chat transcripts, meeting notes
  media/           Podcast/video transcripts
  misc/            Everything else
wiki/              LLM-maintained knowledge base
  index.md         Content catalog (you maintain this)
  log.md           Chronological activity log (you maintain this)
  overview.md      Wiki landing page
  entities/        People, organizations, products, places
  concepts/        Abstract ideas, theories, mental models
  topics/          Broader subject areas (Maps of Content)
  sources/         One summary per ingested source
  analysis/        Synthesis pages, filed answers, comparisons
assets/            Images, diagrams, attachments
```

## Page Types

### Source Summary (`wiki/sources/`)

One page per ingested source. Filename: slugified source title.

```yaml
---
type: source
title: "Original title of the source"
author: "Author name(s)"
date: YYYY-MM-DD
source_type: article | book | paper | conversation | media | misc
source_path: "sources/articles/filename.md"
url: "https://..."
tags: [tag1, tag2]
created: YYYY-MM-DD
updated: YYYY-MM-DD
status: complete | partial
---
```

Body structure:
1. **Summary** -- 2-5 paragraph distillation of key points
2. **Key Claims** -- Bulleted list of specific factual/argumentative claims
3. **Quotes** -- Notable direct quotes with page/timestamp references
4. **Connections** -- Wikilinks to related entity/concept/topic pages
5. **Open Questions** -- Things the source raises but doesn't answer

### Entity Page (`wiki/entities/`)

People, organizations, products, places, events.

```yaml
---
type: entity
entity_type: person | organization | product | place | event
title: "Entity Name"
aliases: ["Alternative Name", "Abbreviation"]
tags: [tag1, tag2]
created: YYYY-MM-DD
updated: YYYY-MM-DD
---
```

Body structure:
1. **Overview** -- 1-2 paragraph description
2. **Details** -- Key facts, structured appropriately for the entity type
3. **Connections** -- Wikilinks to related pages
4. **Sources** -- Wikilinks to source summaries that mention this entity

### Concept Page (`wiki/concepts/`)

Abstract ideas, theories, mental models, principles.

```yaml
---
type: concept
title: "Concept Name"
aliases: ["Alternative Term"]
tags: [tag1, tag2]
created: YYYY-MM-DD
updated: YYYY-MM-DD
---
```

Body structure:
1. **Definition** -- Clear, concise explanation
2. **Explanation** -- Deeper treatment with examples
3. **Connections** -- Wikilinks to related concepts, entities, topics
4. **Sources** -- Where this concept appears in the wiki's sources

### Topic Page (`wiki/topics/`)

Broader subject areas. These function as Maps of Content (MOCs) -- they
primarily organize and link to other pages rather than containing long
prose themselves.

```yaml
---
type: topic
title: "Topic Name"
tags: [tag1, tag2]
created: YYYY-MM-DD
updated: YYYY-MM-DD
---
```

Body structure:
1. **Overview** -- Brief description of the topic area
2. **Key Concepts** -- Wikilinks to concept pages within this topic
3. **Key Entities** -- Wikilinks to entity pages within this topic
4. **Sources** -- Wikilinks to source summaries relevant to this topic
5. **Analysis** -- Wikilinks to analysis pages within this topic
6. **Open Questions** -- Known gaps in the wiki's coverage

### Analysis Page (`wiki/analysis/`)

Synthesis, comparisons, answers to user questions that are worth keeping.

```yaml
---
type: analysis
title: "Analysis Title"
question: "The user's original question, if this was prompted by a query"
tags: [tag1, tag2]
created: YYYY-MM-DD
updated: YYYY-MM-DD
---
```

Body structure:
1. **Question / Framing** -- What this analysis addresses
2. **Analysis** -- The actual synthesis/argument/comparison
3. **Evidence** -- Wikilinks to sources and pages that support the analysis
4. **Limitations** -- What the wiki doesn't have enough data to address
5. **Connections** -- Wikilinks to related pages

## Wikilink Conventions

- Use `[[Page Title]]` syntax (Obsidian wikilinks, not markdown links)
- Link by filename without path: `[[transformer-architecture]]` not
  `[[wiki/concepts/transformer-architecture]]`
- Use display text when the page title is awkward in prose:
  `[[transformer-architecture|Transformer architecture]]`
- Aliases in frontmatter enable linking by alternative names
- **Link liberally.** Every mention of a known entity, concept, or topic
  should be a wikilink on first mention within a section
- When you create a wikilink to a page that doesn't exist yet, that's fine --
  Obsidian shows it as an unresolved link, and you or the human can create
  it later
- Prefer `[[Page Title]]` for internal links and `[text](url)` for external URLs

## Tags

Use lowercase, hyphenated tags in frontmatter: `tags: [machine-learning, nlp]`.
Do not use `#tag` syntax in the body -- keep all tags in frontmatter for
Dataview compatibility.

Common tag categories (extend organically):
- Domain: `machine-learning`, `economics`, `history`, `biology`, etc.
- Meta: `needs-review`, `stub`, `outdated`
- Source-type echoes: `from-book`, `from-paper`, `from-article`

## Operations

### 1. Ingest

**Trigger:** The user adds files to `sources/` and asks you to process them,
or pastes content directly and asks you to ingest it.

**Workflow:**

1. **Read the source.** Read the file(s) in `sources/`. If the user pasted
   content directly, ask if they want you to save it to `sources/` first
   (recommend yes for the archive, but don't block on it).

2. **Create source summary.** Write a page in `wiki/sources/` following the
   Source Summary template. Filename: slugified title, e.g.,
   `attention-is-all-you-need.md`.

3. **Extract entities, concepts, and topics.** As you read the source, identify:
   - Entities mentioned (people, orgs, products, etc.)
   - Concepts discussed (ideas, theories, models)
   - Topics it falls under

4. **Update or create pages.** For each extracted item:
   - If a wiki page already exists: update it with new information from this
     source. Add the source to its Sources section. Update `updated` date.
   - If no page exists: create a new one following the appropriate template.
   - Use judgment on granularity. Not every proper noun needs a page.
     Create pages for things that are: (a) central to the source,
     (b) likely to appear in other sources, or (c) something the user
     would plausibly search for.

5. **Update topic pages.** Add links to new pages in the relevant topic MOCs.
   Create new topic pages if the source opens a genuinely new subject area.

6. **Update index.md.** Add the new source summary and any new pages to the
   index. (See Index format below.)

7. **Update log.md.** Append an entry recording what you did. (See Log format
   below.)

8. **Report to the user.** Summarize what you created/updated:
   - Source summary page
   - New pages created (with links)
   - Existing pages updated
   - Any questions or ambiguities you encountered

**Handling large sources:** If a source is very long (e.g., a full book),
process it in sections. Set `status: partial` in frontmatter and note what
was covered. The user can ask you to continue later.

**Handling duplicate sources:** If a source covers material already in the
wiki, don't create duplicate pages. Update existing pages with any new
information and note the additional source.

### 2. Query

**Trigger:** The user asks a question.

**Workflow:**

1. **Search the wiki.** Read relevant pages in `wiki/` to find information
   that addresses the question. Use the index and topic pages to navigate.

2. **Answer the question.** Give a direct answer, citing wiki pages with
   wikilinks. Be explicit about what comes from the wiki vs. your own
   general knowledge. Format:
   - "According to [[source-page]], ..."
   - "The wiki doesn't cover this, but based on general knowledge: ..."

3. **Decide whether to file the answer.** File the answer as an analysis
   page in `wiki/analysis/` if ANY of these apply:
   - The user explicitly asks you to save/file it
   - The answer synthesizes multiple sources in a novel way
   - The question reveals a gap and you filled it with substantial research
   - The answer would be useful to reference later

   Do NOT file trivial lookups ("what year was X founded?") or questions
   fully answered by a single existing page.

4. **If you file it:** Create the analysis page, update index.md and log.md,
   and tell the user you filed it.

5. **If the question reveals a gap:** Note it in the relevant topic page's
   Open Questions section and mention it to the user:
   "The wiki doesn't have good coverage of X. If you have sources on this,
   drop them in `sources/` and I'll process them."

### 3. Lint

**Trigger:** The user asks you to health-check the wiki, or you notice
issues during other operations.

**Checks to run:**

1. **Orphan pages.** Pages in `wiki/` not linked from any other page and
   not in the index. Fix: add to index and relevant topic pages.

2. **Broken wikilinks.** Links to pages that don't exist (Obsidian shows
   these, but check programmatically too). Fix: create stub pages or
   correct the link.

3. **Stale pages.** Pages with `updated` date more than 6 months old.
   Flag for review, don't auto-update.

4. **Missing frontmatter.** Pages missing required frontmatter fields.
   Fix: add the missing fields.

5. **Index drift.** Pages that exist but aren't in index.md, or index
   entries pointing to pages that don't exist. Fix: sync the index.

6. **Empty sections.** Pages with section headers but no content beneath
   them. Flag for the user.

7. **Source coverage.** Files in `sources/` that don't have a corresponding
   summary in `wiki/sources/`. Report to user: "These sources haven't
   been ingested yet: ..."

8. **Tag consistency.** Identify variant spellings of the same tag
   (e.g., `ml` vs `machine-learning`). Suggest normalization.

**Output:** A summary of findings grouped by severity:
- **Fix now** (you should fix these immediately): orphans, index drift,
  missing frontmatter
- **Flag for review** (tell the user): stale pages, empty sections,
  un-ingested sources
- **Suggestions** (nice-to-have): tag normalization, potential merges

## Edge Cases

### Sources that contradict each other

Do NOT silently pick a winner. In the relevant concept/entity page, note
the disagreement explicitly:

> **Note:** [[source-a]] claims X, while [[source-b]] claims Y. See
> [[analysis-page]] for discussion.

Create an analysis page if the contradiction is significant. Let the
human decide which source to trust.

### Pages that should be merged

If you find two pages covering the same thing (e.g., `ml.md` and
`machine-learning.md`), merge them:
1. Keep the more complete page
2. Move any unique content from the other page into it
3. Delete the duplicate
4. Update all wikilinks that pointed to the deleted page
5. Add the deleted page's title as an alias in frontmatter
6. Log the merge in log.md

### Growing a page beyond its type

If an entity page accumulates so much conceptual discussion that it's
really two things (e.g., "OpenAI" the org vs. "scaling laws" the concept),
split it. Create the new page, move the relevant content, cross-link,
update the index, and log it.

### User asks a question about something not in the wiki

Answer from general knowledge but clearly label it:

> "The wiki doesn't have information on this topic yet. Based on general
> knowledge: [answer]. Want me to file this as an analysis page, or do
> you have sources you'd like me to ingest first?"

## Index Format (`wiki/index.md`)

The index is the wiki's table of contents. Keep it organized by section,
alphabetically within each section.

```markdown
---
type: index
updated: YYYY-MM-DD
---

# Wiki Index

## Topics
- [[topic-name]] -- Brief description

## Concepts
- [[concept-name]] -- Brief description

## Entities
- [[entity-name]] -- Brief description

## Sources
- [[source-summary-name]] -- "Original Title" by Author (YYYY)

## Analysis
- [[analysis-name]] -- Brief description
```

**Maintenance rules:**
- Update the index every time you create or delete a page
- Keep entries alphabetical within each section
- One line per page, format: `- [[page]] -- description`
- Update the `updated` date in frontmatter when you modify the index

## Log Format (`wiki/log.md`)

The log is a reverse-chronological record of wiki activity.

```markdown
---
type: log
---

# Wiki Activity Log

Newest entries first.

## YYYY-MM-DD

### Ingest: "Source Title"
- Created [[source-summary-page]]
- Created [[new-entity-page]], [[new-concept-page]]
- Updated [[existing-topic-page]]

### Query: "User's question summary"
- Answered from [[page1]], [[page2]]
- Filed as [[analysis-page-name]]

### Lint: Health check
- Fixed 3 orphan pages
- Flagged 2 stale pages for review
- Created 1 stub page for broken link

### Maintenance: Description
- Merged [[old-page]] into [[kept-page]]
- Split [[big-page]] into [[page-a]] and [[page-b]]
```

**Maintenance rules:**
- Always prepend new entries (newest first)
- Group entries under date headers (`## YYYY-MM-DD`)
- Use sub-headers for each operation type
- Include wikilinks to all pages touched
- Keep entries concise -- the log is a record, not a narrative

## Obsidian Tips

**Graph view:** The wiki is designed for graph view. Topic pages are hubs
(many connections), source summaries are leaves (few connections), and
entity/concept pages are the connective tissue. Liberal wikilinking
makes the graph useful.

**Dataview queries the user might find useful:**

```dataview
TABLE author, date, source_type
FROM "wiki/sources"
SORT date DESC
```

```dataview
LIST
FROM "wiki/concepts"
WHERE contains(tags, "machine-learning")
SORT title ASC
```

```dataview
TABLE updated
FROM "wiki"
WHERE status = "partial"
```

```dataview
LIST
FROM "wiki"
WHERE contains(tags, "needs-review")
```

**Recommended Obsidian settings:**
- Files & Links > Default location for new attachments: `assets/`
- Files & Links > Use [[Wikilinks]]: ON
- Files & Links > New link format: Shortest path when possible

## Session Start

When you begin a new session, orient yourself:

1. Read `CLAUDE.md` (this file) to understand conventions
2. Read `wiki/index.md` to understand what's in the wiki
3. Read the latest entries in `wiki/log.md` to understand recent activity
4. Check if there are un-ingested files in `sources/` (files without
   corresponding pages in `wiki/sources/`)
5. If you find un-ingested sources, mention it:
   "I notice there are new files in sources/ that haven't been processed
   yet: [list]. Want me to ingest them?"

## General Principles

- **Accuracy over completeness.** Don't invent information. If a source
  is ambiguous, say so. If you're filling in from general knowledge,
  label it clearly.
- **The human is the curator.** You maintain, you don't decide what's
  important. If in doubt about whether to create a page, lean toward
  creating it -- the human can always ask you to prune.
- **Idempotent operations.** Running ingest on the same source twice
  should not create duplicates. Running lint twice should not create
  new issues.
- **Atomic updates.** When an operation touches multiple files (e.g.,
  ingest creates a source page, updates topic pages, updates the
  index), do them all in one go. Don't leave the wiki in a half-
  updated state.
- **Prefer updating over creating.** If a page already covers 80% of
  what a new page would say, update the existing page instead.
