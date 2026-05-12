# CLAUDE.md — LLM Wiki Schema (Template)

> This template is auto-appended to your project's CLAUDE.md during `/wiki init`.
> Customize it to fit your domain after initialization.

## Repository Identity

Personal LLM Wiki — a persistent, compounding knowledge base maintained by LLM.
Human curates sources and asks questions; LLM does summarizing, cross-referencing, and bookkeeping.

## Three-Layer Architecture

| Layer | Path | Owner | Rule |
|-------|------|-------|------|
| Raw Sources | `raw/` | Human | Immutable. LLM reads but never modifies. |
| Wiki | `wiki/` | LLM | LLM creates, updates, and maintains all pages. |
| Archive | `archive/` | Reference | Legacy content. Read-only. |

## Operations

### Ingest
1. Save raw content to `raw/`
2. Create/update relevant `wiki/` pages
3. Add cross-references between related pages
4. Update `wiki/index.md` + append to `wiki/log.md`

### Query
1. Read `wiki/index.md` to locate relevant pages
2. Synthesize answer with citations
3. Offer to save valuable syntheses as new wiki pages

### Lint
1. Check broken links, orphan pages, index consistency
2. Report findings — do NOT auto-fix

## Editing Conventions

- Wiki filenames: lowercase English slug with hyphens (e.g. `harness-engineering.md`)
- Each page starts with `# Title` + one-paragraph summary
- Cross-references: `→ see also: [Page Name](relative-path.md)`
- Pages should be < 80 lines; split if longer
