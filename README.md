# db-paper-writer

A [Claude Code skill](https://docs.anthropic.com/en/docs/claude-code/skills) for writing and revising database research papers targeting **SIGMOD, VLDB/PVLDB, ICDE, and CIDR** — with specific depth for vector database and AI-era systems papers (ANNS, RAG infrastructure, LLM+DB, learned indexes, embedding pipelines).

---

## What it does

| Task | Description |
|---|---|
| Draft from scratch | Abstract, Introduction, System Overview, Component sections, Evaluation, Conclusion |
| Revise & polish | Diagnose common weaknesses (vague motivation, buried contributions, weak baselines) with before/after diffs |
| Structure audit | Check section proportions, subsection count, and contribution-to-experiment mapping against the canonical layout |
| Vector DB specialization | ANNS terminology, mandatory baseline checklist, recall-latency curve standards, standard datasets |
| Venue formatting | SIGMOD (ACM acmart), PVLDB (vldb class), ICDE (IEEEtran), CIDR — formatting and submission details for each |

---

## Installation

**Personal install (available in all your projects):**

```bash
git clone https://github.com/Wangyibo321/VDB-paper-writer-skill ~/.claude/skills/db-paper-writer
```

**Project-level install (available only in the current project):**

```bash
git clone https://github.com/Wangyibo321/VDB-paper-writer-skill .claude/skills/db-paper-writer
```

The skill is immediately available — no restart needed. Claude Code discovers skills automatically from these directories.

**Keeping up to date:**

```bash
cd ~/.claude/skills/db-paper-writer && git pull
```

---

## Usage

The skill triggers automatically when you describe a paper-writing task:

```
Write the Introduction for my SIGMOD paper on filtered vector search.
```

```
Revise my VLDB Evaluation section — the structure is unclear.
```

```
Here is my abstract, please review it: [paste content]
```

You can also invoke it explicitly:

```
/db-paper-writer  Draft a SIGMOD paper framework for a disk-resident vector index.
```

---

## Repository structure

```
db-paper-writer/
├── README.md
├── CONTRIBUTING.md
├── SKILL.md                           # Skill entry point (workflow, triggers, house style)
└── references/
    ├── structure.md                   # Canonical paper layout — loaded first for every task
    ├── drafting.md                    # Section-by-section drafting templates and LaTeX skeletons
    ├── revising.md                    # Weakness diagnostics and fix patterns (W1–W12)
    ├── venues.md                      # SIGMOD / VLDB / ICDE / CIDR formatting and submission details
    └── vector-db-conventions.md       # Vector DB subfield conventions (terminology, baselines, datasets)
```

---

## Reference files

### `references/structure.md` — Canonical Layout (loaded first for every task)

Defines:
- Standard 8-section layout with page budgets for each section
- Component section rules (default 3, max 4)
- Global subsection cap (strictly fewer than 6 across §4–§6 combined)
- Internal writing logic for each section: motivation → mechanism → analysis → example
- Common layout failure modes and fixes

### `references/drafting.md` — Drafting Templates

Loaded for drafting tasks. Contains:
- Abstract 5-sentence formula + LaTeX template
- Introduction 5-paragraph structure + paragraph-level LaTeX template
- Templates for related work, problem formulation, system design, core algorithms, evaluation, and conclusion
- Algorithm block LaTeX template
- Recommended macro definitions

### `references/revising.md` — Revision Diagnostics

Loaded for revision tasks. Contains:
- Diagnostic reading order (abstract + contributions list first, then intro, then eval section titles)
- Fix priority: structure > argument > prose
- W1–W12 weakness patterns, each with before/after examples

| ID | Weakness | Core fix |
|---|---|---|
| W1 | Vague motivation | Replace with a specific workload + concrete number |
| W2 | Overclaiming novelty | Reframe as a differentiation claim |
| W3 | Weak baselines | Add latest FAISS / DiskANN / HNSW comparisons |
| W4 | Buried contributions | Contribution list on page 1–2; each bullet is an insight, not an activity |
| W5 | Evaluation without a story | Rename subsections to Q1/Q2...; end each with an italicized takeaway |
| W6 | Related work as a list | Group by approach family; each paragraph ends with the limitation you address |
| W7 | Missing / weak Figure 1 | Page 1–2 must have an architecture diagram or motivating example |
| W8 | Unsupported claims | Every qualitative claim in abstract/intro/conclusion must carry a number |
| W9 | Tense drift | System behavior/contributions: present tense; experiments: past tense |
| W10 | Wordiness and LaTeX slop | Cut hspace hacks; reduce content rather than shrinking font size |
| W11 | Missing problem statement | Add `\begin{definition}` block in §2.1 or §3.1 |
| W12 | Contribution / experiment mismatch | Match every intro contribution to an evaluation subsection; fill gaps |

### `references/venues.md` — Submission Formatting

Loaded when the user asks about venue-specific formatting. Contains:
- LaTeX class, page limits, anonymity requirements, and special tracks for each venue
- PVLDB monthly deadline and revision round mechanics
- Quick venue picker guide

### `references/vector-db-conventions.md` — Vector DB Specialization

Loaded for any task involving vector databases or ANNS. Contains:
- Canonical terminology (ANNS, Recall@k, QPS, index build time, etc.)
- Mandatory baselines: FAISS, HNSW, DiskANN, SPANN, ScaNN + sub-problem-specific baselines
- Standard datasets: SIFT1M, GIST1M, DEEP1B, MS MARCO, BigANN, LAION, etc.
- Recall-latency curve standards (axes, error bars, caption requirements)
- System paper vs. algorithm paper vs. hybrid paper framing
- AI-era systems themes (2024–2026)
- LLM-related paper considerations at DB venues
- Reproducibility expectations and experimental hygiene checklist

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

Key contribution areas:
- Keep `references/vector-db-conventions.md` current — baseline list and dataset list need periodic updates
- Update `references/venues.md` when venue formatting rules change
- Add new weakness patterns to `references/revising.md`
- Add new reference files (e.g., `references/rebuttal.md`, `references/ea-and-b.md`)

---

## License

MIT
