---
name: distill-to-vault
description: >
  Distills lecture scripts, textbooks, papers, and MOOCs into structured,
  deduplicated Markdown notes for an Obsidian vault or plain-Markdown
  knowledge base. Use when the user wants to process course material,
  compile study notes, extract knowledge from a textbook or paper, build
  a personal knowledge vault from academic sources, or add new material
  to an existing vault without creating duplicates. Covers curriculum
  courses, research papers, online courses, and book distillation.
license: MIT
---

# Distill a source into a knowledge vault

Turn a body of study material into durable, trustworthy notes — instead of a pile of transcribed
slides.

## The architecture this builds

Two layers per source, kept apart:

```
<Vault>/<Area>/<Source>/
├── _<Source>.md   ← index note / MOC: what this source is, what was distilled, what was skipped
├── Distilled/     ← ★ the authoritative layer — what you produce
├── Personal/      ← the user's own artifacts (exercises, solutions) — never authoritative
└── Sources/       ← the original PDFs/slides, kept for provenance
```

**Prime directive:**

> **Each concept is stored ONCE, at its deepest treatment.** Never write a shallower duplicate of
> something the vault already covers better. Cross-link instead.

Most of the work below happens *before* you write a single note. That is the point: an unchecked
distillation quietly grows a second, worse copy of knowledge the vault already had.

---

## Before you start: learn the vault

**Do not impose the conventions in this file. Discover the vault's own and match them.**

1. **Locate the vault.** From the user, the cwd, or context. Ask if ambiguous. Confirm it is a git
   repo (or ask how they version it) before writing.
2. **Read 2–3 existing notes** from its authoritative layer, plus one index/MOC note. Extract:
   - the frontmatter schema actually in use (field names, controlled values),
   - section heading style and language,
   - how references/citations are formatted,
   - how the index note is organised,
   - the folder taxonomy and naming (translated? original-language? by subject? by degree?).
3. **Match what you find.** If the vault writes headings in a given language or pairs two languages,
   do the same. If it names folders in the source's original language, do the same. Your notes should
   be indistinguishable in style from the ones already there.
4. **An empty vault?** Then propose a convention set, show the user *one* note as a sample, and get
   agreement before producing the rest.

---

## Pipeline

**One source at a time.** Finish it, commit, stop, report. Do not chain sources.

### 1. Verify what the source ACTUALLY covers — never trust its title

Read the source's own table of contents / lecture list / chapter headings first. Write down what it
really teaches **before** planning anything.

This is the step people skip, and it is the one that most often invalidates everything downstream.
Titles mislead constantly. Cases seen in practice:

- a course titled "Theoretical Computer Science I" that was, in fact, an **algorithms and data
  structures** course — nothing about automata;
- a "Data Security" course that turned out to be pure cryptography, with **none** of the legal/privacy
  material the title implies, and not even the classic cipher everyone assumes;
- a "Computational Geometry" course that was **analytic and differential geometry** for graphics, not
  the algorithmic subject of that name;
- a folder named after a subject that actually contained a **thesis**, not coursework;
- material filed under the wrong institution entirely.

If the real content contradicts the title, the folder name, the index note, or the user's own
description — **say so and stop for a decision.** Do not quietly distill the thing you found instead.

### 2. Check existing coverage BEFORE writing

For each major topic the source covers, search the vault for existing treatment.

- **Prefer semantic search** if the vault has a search MCP or RAG server attached — conceptual
  overlaps hide behind different vocabulary, and keyword search finds those only by luck. Use 2–4
  query reformulations per topic; mix languages if the vault is multilingual.
- **Otherwise `grep -r`** the authoritative layer for the key terms, and tell the user coverage
  checking was weaker than it could be.
- If a search index is involved, know its staleness: an index built from pushed/committed state will
  not contain notes you wrote this session.

Overlaps are not rare and not always where you expect them. Two different courses teaching the same
formal method under different names is a normal outcome, not an edge case.

### 3. Classify: delta, standalone, or skip

- **A deeper treatment already exists** → distill **only the delta**:
  1. prerequisite concepts the deeper note *assumes but never defines*,
  2. subtopics the deeper source omits entirely,
  3. rare, exceptionally clear explanations worth preserving.
- **Delta ≈ 0 → do NOT distill.** Keep the source archived, record the decision in the index note, and
  tell the user. A skipped source is a successful outcome, not a failure.
- **No existing counterpart** → **full standalone distillation**: this source is the vault's deepest
  coverage of the topic.

Ask the user how generously to fill in prerequisite foundations — completeness and minimalism are both
defensible, and it is their call, not yours.

### 4. Plan the note set

One note per **unit of the source's own structure** (lecture / chapter / topic cluster) — not per
arbitrary slice you invent. Typically 4–12 notes. For a large source or a non-obvious classification,
show the plan and get agreement before writing.

### 5. Write, grounded

- **Ground every claim in the source. Never fabricate.** This layer's whole value is that it can be
  trusted without re-reading the original.
- **Flag gaps rather than filling them.** If part of the source is missing or unclear, say so *in the
  note* ("grounded on the exercise sheets; the lecture deck for this block is absent from the source").
  A flagged gap is honest; an invented bridge is a landmine.
- If the source's math or diagrams exist only as images, use the canonical formulation and say that is
  what you did.
- Cite textbooks **only as the source itself names them**. If it names none, write that explicitly.
  Never invent a bibliography.
- Keep everything portable: standard Markdown, LaTeX, fenced code, Mermaid. No plugin-specific syntax
  — the vault should survive its tooling.

### 6. Frontmatter and references

Use **the vault's existing schema** (from "Before you start"). A typical shape:

```yaml
---
title: "<note title>"
aliases: [<terms someone would actually search — every language the vault uses>]
type: distilled
area: <area>
course: "<source name>"
level: <depth>
source: "<precise unit: lecture/chapter numbers + author>"   # be exact
tags: [...]
status: draft
created: <date>
---
```

Two things carry disproportionate weight:

- **`aliases`** — search engines and humans both find notes through them. Include synonyms, acronyms,
  and the term in each language the vault uses.
- **A precise `source:` and a references section** — cite the *specific* lectures/chapters, not the
  work as a whole. "ch. 3, §3.2" is a reference; the book's title alone is a gesture. Every note in a
  trustworthy layer should let a reader go verify it in one step.

Match the vault's reference formatting. If the vault has one universal section (most do — check), your
note must have it too.

### 7. Update the index note (`_<Source>.md`)

The index note should let a reader understand the source without opening a single note:

- **What this source is** — a short positioning paragraph: what it covers, where it sits relative to
  neighbouring sources in the vault.
- **The distilled notes** — a list, each with a one-line summary **and the source units it came from**.
- **What was deliberately NOT distilled, and why** — this section matters more than it looks. Record
  which topics were dropped because another note owns them, which were deferred to a deeper source,
  which are cross-linked, and which were out of scope. **Without this, a future reader (or agent)
  reads your dedup decisions as gaps and re-adds the duplicates you carefully avoided.**
- **A pointer to the personal layer**, if any.

Cross-link bidirectionally with the notes you deferred to or borrowed from. When two notes genuinely
share a topic, resolve it by **division of labour plus links** — one note owns the formal treatment,
the other keeps its own lens and links across — rather than by merging them.

### 8. Commit and stop

Commit this source alone, with a message naming it. Push if the vault has a remote — any search index
downstream only sees pushed state. Then **stop and report to the user.**

---

## Non-course sources (books, papers, MOOCs)

Same pipeline; only the vocabulary shifts:

| Course | Book | Paper | MOOC |
|---|---|---|---|
| lecture | chapter / section | section | module / lecture |
| `source: "Lecture 3, 8–9 (<lecturer>)"` | `source: "<Author>, <Title>, ch. 9"` | `source: "<Author> et al. <year>, §3.2"` | `source: "<Course>, lecture 5 (<provider>)"` |
| script/slides → `Sources/` | book PDF → `Sources/` | paper PDF → `Sources/` | slides → `Sources/` |

- Drop schema fields that stop making sense (a paper has no `level`); let `course:`/`university:`
  become the venue or publisher, or omit them.
- A book or paper earns its own folder + index note only if it yields **≥3 notes**. Fewer → add the
  note(s) to an existing source's authoritative layer, or a general/misc area.
- **Steps 1–3 matter *more* here, not less.** A paper overlapping an existing note is the single most
  likely case to end at "delta ≈ 0 → don't distill, just cross-link."

---

## Discipline

1. **One source at a time.** Finish, commit, stop for review. Never batch.
2. **Never fabricate.** Missing or unclear source material gets flagged in the note, not filled in.
3. **Ask when classification is genuinely ambiguous** — especially delta-vs-standalone, and any time
   step 1 contradicts the label.
4. **Prefer showing a plan over silently producing ten notes.** Distillation is judgment-heavy; a
   wrong call is expensive to unwind and easy to miss in review.
