---
name: llm-wiki
description: |
  Build and maintain a personal LLM Wiki — a persistent, compounding knowledge base.
  Three operations: ingest (add knowledge), query (ask questions), lint (health check).
  Use when: user wants to build a knowledge base, ingest articles, query accumulated knowledge, or check wiki health.
  Init: "/wiki init", "初始化知识库", "setup wiki"
version: 0.1.0
author: gogolyq-new
---

# SKILL: llm-wiki

> LLM 维护的个人知识库。人类策展来源、提出问题；LLM 做摘要、交叉引用、归档。

---

## Trigger

- `/wiki init` — 初始化目录骨架
- `/wiki ingest <source>` — 摄入新素材
- `/wiki query <question>` — 查询知识库
- `/wiki lint` — 健康检查

---

## Workflow

### /wiki init

1. Create directory structure:
   ```
   raw/articles/  raw/projects/  raw/assets/
   wiki/concepts/  wiki/entities/  wiki/comparisons/  wiki/syntheses/
   ```
2. Create `wiki/index.md` (empty catalog template)
3. Create `wiki/log.md` (empty, with header)
4. Append Wiki maintenance rules to project's `CLAUDE.md`
   (read `references/schema-template.md` for the content)
5. Output:
   ```
   → 操作: init
   → 创建: raw/ wiki/ 目录骨架 + index.md + log.md
   ✓ 完成: 知识库已初始化，可以开始 ingest
   ```

### /wiki ingest <source>

Source can be: URL, pasted text, or file path.

1. Save raw content → `raw/articles/` or `raw/projects/` (choose by type)
2. Extract key concepts, entities, relationships
3. For each concept/entity:
   - If wiki page exists → update it (add new info, cross-references)
   - If not → create new page in appropriate subdirectory
4. Update `wiki/index.md` — add/update entry with one-line summary
5. Append to `wiki/log.md`:
   ```
   ## [YYYY-MM-DD] ingest | <source title>
   - <what was added/updated>
   ```
6. Output:
   ```
   → 操作: ingest
   → 来源: <source title>
   → 变动: 新建 N 页, 更新 M 页
   ✓ 完成: <one-line summary>
   ```

Completion criteria: index updated + log appended + pages created/updated.

### /wiki query <question>

1. Read `wiki/index.md` → locate relevant pages
2. Read those pages → synthesize answer with citations
3. If answer is a valuable cross-source synthesis → offer to save as `wiki/syntheses/<slug>.md`
4. Output: answer with `→ see: [page](path)` citations

### /wiki lint

1. Check all internal links (→ see also) → report broken ones
2. Check orphan pages (no inbound links from other pages)
3. Check `wiki/index.md` consistency with actual files
4. Check `Sources` references point to existing files
5. Report as checklist, do NOT auto-fix
6. Output:
   ```
   → 操作: lint
   → 结果: N 项通过, M 项需处理
   <checklist of issues>
   ```

---

## Behavior Constraints

### DO
- Keep wiki pages concise (< 80 lines; split if longer)
- Add cross-references: `→ see also: [Page](relative-path.md)`
- Every wiki page starts with `# Title` + one-paragraph summary
- Match existing naming style (lowercase-slug.md)

### AVOID
AVOID modifying files in raw/ — they are immutable source of truth
AVOID auto-fixing lint issues — report and wait for user approval
AVOID ingest without updating both index.md and log.md
AVOID writing wiki pages over 80 lines — split into sub-pages instead
AVOID "improving" unrelated wiki pages during ingest — only touch pages directly related to the new source
AVOID speculative content — wiki pages reflect what sources actually say, not extrapolation

---

## Architecture (for user reference)

```
project/
├── CLAUDE.md          # Schema: tells LLM how to maintain this wiki
├── raw/               # Immutable sources (human curates)
│   ├── articles/      # External articles, clippings
│   ├── projects/      # Project snapshots, gists
│   └── assets/        # Images, binaries
├── wiki/              # LLM-maintained layer (LLM writes, human reads)
│   ├── index.md       # Full catalog (updated every ingest)
│   ├── log.md         # Append-only operation log
│   ├── concepts/      # One concept per file
│   ├── entities/      # Tools, people, frameworks
│   ├── comparisons/   # A-vs-B structured comparisons
│   └── syntheses/     # Cross-source insights from queries
└── archive/           # Optional: legacy notes (read-only)
```
