# LLM Wiki Skill

> Build a persistent, compounding personal knowledge base — maintained by LLM, curated by you.

Inspired by [Andrej Karpathy's LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) and [Karpathy Skills](https://github.com/forrestchang/andrej-karpathy-skills).

## The Problem

Most AI + documents workflows are RAG: upload files, retrieve chunks at query time, generate answer. Nothing accumulates. Ask the same question next month, the LLM rediscovers everything from scratch.

## The Solution

Instead of retrieving from raw documents every time, the LLM **incrementally builds a structured wiki** — with cross-references, contradiction flags, and evolving synthesis. Knowledge compounds over time.

**You** curate sources and ask questions. **The LLM** does summarizing, cross-referencing, filing, and maintenance.

## 5-Minute Setup

### Prerequisites
- Claude Code (or any AI IDE that supports Skills)
- A git-initialized project directory

### Install

```bash
/plugin marketplace add gogolyq-new/llm-wiki-skill
/plugin install llm-wiki
```

### Initialize

```
/wiki init
```

This creates the directory structure and appends wiki maintenance rules to your `CLAUDE.md`.

## Commands

| Command | What it does |
|---------|-------------|
| `/wiki init` | Create directory skeleton + schema |
| `/wiki ingest <source>` | Add new material → extract → update wiki pages + index + log |
| `/wiki query <question>` | Answer from wiki with citations; offer to save synthesis |
| `/wiki lint` | Health check: broken links, orphan pages, stale content |

## How It Works

```
You provide source ──→ LLM ingests ──→ Wiki grows
                                         ↓
You ask question ───→ LLM reads wiki ──→ Answer (may become new wiki page)
                                         ↓
You say "lint" ─────→ LLM scans ───────→ Health report
```

Each ingest can touch 5-15 wiki pages. Knowledge compounds with every source and every question.

## Directory Structure (after init)

```
your-project/
├── CLAUDE.md          # Schema (auto-configured)
├── raw/               # Your sources (immutable)
│   ├── articles/
│   ├── projects/
│   └── assets/
├── wiki/              # LLM-maintained knowledge
│   ├── index.md       # Full catalog
│   ├── log.md         # Operation history
│   ├── concepts/      # Concept pages
│   ├── entities/      # Tool/person/framework pages
│   ├── comparisons/   # A-vs-B comparisons
│   └── syntheses/     # Cross-source insights
└── archive/           # Optional legacy content
```

## Design Principles

This skill follows [Karpathy Skills](https://github.com/forrestchang/andrej-karpathy-skills) behavior constraints:

- **Simplicity** — Wiki pages are concise, no padding
- **Surgical** — Ingest only touches related pages, not drive-by improvements
- **Goal-Driven** — Each operation has explicit completion criteria

And applies these knowledge management principles:

- **raw/ is immutable** — Source of truth, never modified by LLM
- **Wiki is the compilation layer** — Not a copy of sources, but a synthesis
- **index.md is the entry point** — LLM reads it first to navigate
- **log.md is the timeline** — Append-only, parseable with `grep "^## \[" wiki/log.md`

## Verification

The skill is working if:
- `wiki/index.md` grows with each ingest
- Wiki pages have cross-references (`→ see also`)
- `/wiki lint` reports mostly green
- Queries cite wiki pages, not raw sources directly

## License

MIT
