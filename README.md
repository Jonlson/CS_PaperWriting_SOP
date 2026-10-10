<div align="center">

# Paper Writing & Typesetting Optimization SOP

### Standard Operating Procedure for turning raw material into a journal-ready paper

English / [中文](README_CN.md)

<a href="https://github.com/Jonlson/CS_PaperWriting_SOP">GitHub</a> · <a href="https://github.com/Jonlson/CS_PaperWriting_SOP/issues">Issues</a> · <a href="https://github.com/Jonlson/CS_PaperWriting_SOP/tree/main/SOP">SOP Documents</a>

<p>
  <a href="https://github.com/Jonlson/CS_PaperWriting_SOP/stargazers"><img src="https://img.shields.io/github/stars/Jonlson/CS_PaperWriting_SOP?labelColor&style=flat-square&color=ffcb47" alt="stars"></a>
  <a href="https://github.com/Jonlson/CS_PaperWriting_SOP/forks"><img src="https://img.shields.io/github/forks/Jonlson/CS_PaperWriting_SOP?labelColor=black&style=flat-square&color=369eff" alt="forks"></a>
  <a href="https://github.com/Jonlson/CS_PaperWriting_SOP/issues"><img src="https://img.shields.io/github/issues/Jonlson/CS_PaperWriting_SOP?labelColor=black&style=flat-square&color=ff80eb" alt="issues"></a>
  <a href="https://github.com/Jonlson/CS_PaperWriting_SOP/commits/main"><img src="https://img.shields.io/github/last-commit/Jonlson/CS_PaperWriting_SOP?color=c4f042&labelColor=black&style=flat-square" alt="last commit"></a>
</p>

</div>

***

> **Scope**: The client supplies a "hybrid pile of complete content" (a thesis, project report, informal notes, or other raw material) and needs a journal paper that is **accurate in content, standardized in layout, academic in style, and complete in scientific logic** (Chinese version first).
>
> This SOP was distilled from the ESDS/ETD-Net paper project (2026-10; image enhancement + object detection for power-warehouse scenes; all 7 chapters completed and passed value-by-value verification). All parameters and commands have been verified in practice.

---

## 1. Overall Workflow (Six Stages)

```
Stage 1 Material Parsing   Stage 2 Environment &      Stage 3 Chapter-by-Chapter Writing (loop)
Read all raw material,     Template                  Per chapter: extract → plan → decide →
build mapping, log         Verify LaTeX env, build   write → compile → report
source issues              template, get compile
                           chain running
        │                       │                       │
        └───────────────────────┴───────────────────────┘
                                │
        Stage 4 Layout Governance  ←  Stage 5 Full-Text Verification  →  Stage 6 Revision Loop
                                                          (handled by category of user feedback)
```

| Stage | Document | Output | Gate to Pass |
| ----- | -------- | ------ | ------------ |
| 1 Material Parsing | [SOP-01](SOP/SOP-01-素材解析.md) | Mapping table, issue log | Client confirms handling of contradictions |
| 2 Environment & Template | [SOP-02](SOP/SOP-02-环境与模板.md) | Compilable empty template | First compile with 0 errors |
| 3 Chapter Writing | [SOP-03](SOP/SOP-03-写作规程.md) | Per-chapter `.tex` files | Each chapter compiles with 0 errors and 0 undefined references |
| 4 Layout Governance | [SOP-04](SOP/SOP-04-排版规范.md) | Full text in journal layout | 0 Overfull boxes, 0 float warnings |
| 5 Full-Text Verification | [SOP-05](SOP/SOP-05-校验体系.md) | Verification report | Every table and value consistent |
| 6 Revision Loop | SOP-03 §5 + SOP-06 | Revised version | User visual acceptance |

## 2. File Index

### Core Workflow (01–06 + Checklist)

| File | Content | When to Read |
| ---- | ------- | ------------ |
| [SOP-01-素材解析.md](SOP/SOP-01-素材解析.md) | Raw-material inventory, how to build the mapping table, logging source-text issues | At project kickoff |
| [SOP-02-环境与模板.md](SOP/SOP-02-环境与模板.md) | Compile environment, template skeleton, macro definitions, compile recipes | When building the template |
| [SOP-03-写作规程.md](SOP/SOP-03-写作规程.md) | Writing iron rules, per-chapter loop, checkpoint alignment, revision handling | Before writing each chapter |
| [SOP-04-排版规范.md](SOP/SOP-04-排版规范.md) | Layout parameter table, figure/table/equation conventions, numbering system | Layout stage; per-chapter self-check |
| [SOP-05-校验体系.md](SOP/SOP-05-校验体系.md) | Executable command set for data-alignment verification | Mandatory before finalizing |
| [SOP-06-陷阱库.md](SOP/SOP-06-陷阱库.md) | Quick reference of environment pitfalls and fixes (12 field-tested items) | On errors or hangs |
| [checklist-定稿检查清单.md](SOP/checklist-定稿检查清单.md) | Tick-box final checklist | Before delivery |

### Infrastructure & Domain Knowledge (07–10)

| File | Content | When to Read |
| ---- | ------- | ------------ |
| [SOP-07-LaTeX环境与平台规范.md](SOP/SOP-07-LaTeX环境与平台规范.md) | Full workflow for Windows/macOS/Linux/Overleaf: install, use, compile, failure detection & recovery, token-saving | When setting up the environment; on any environment anomaly |
| [SOP-08-期刊会议风格分类.md](SOP/SOP-08-期刊会议风格分类.md) | Consolidated analysis of layout, length, citation, and submission requirements for Chinese and international CS journals/conferences | When choosing the target venue; when adapting a paper version |
| [SOP-09-论文叙事方法.md](SOP/SOP-09-论文叙事方法.md) | Gap-Insight-Design-Evidence skeleton, narrative strategies by paper type/venue, scientific-logic templates | Before writing each chapter; when self-checking the narrative |
| [SOP-10-SOP使用指南.md](SOP/SOP-10-SOP使用指南.md) | Supported platforms, six-step startup for new tasks, session/memory management, Git collaboration & CI conventions, command cheat sheet | **First read for any new user / new session** |

### Recommended Reading Order

- **New user / new session**: README → SOP-10 (how to use) → the SOP matching your current stage
- **Taking a new task**: SOP-10 §2 six-step startup → SOP-01 → SOP-08 (choose venue) → SOP-09 (choose narrative) → chapter-by-chapter loop
- **Environment trouble**: SOP-06 → SOP-07 §3–4

## 3. Roles & Discipline

- **AI executor**: Content **must** originate from the client's source material. Only extract, condense, and normalize into LaTeX; do not improvise or introduce external facts. When encountering contradictions, gaps, or trade-offs, **raise a checkpoint for the client to decide** rather than deciding unilaterally.
- **Client**: Provides the content source, reference list, and figures; makes decisions at checkpoints; visually accepts the PDF (the AI can only verify via text and cannot visually inspect rendering, so **rendering quality must be checked by a human flipping through the PDF**).
- **Verification discipline**: Always verify PDF state via text files `main.aux` / `main.log` (chapter page numbers, figure/table labels, error counts); never try to read the PDF visually.

