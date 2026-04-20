---
name: db-paper-writer
description: Draft and revise database research papers for SIGMOD, VLDB/PVLDB, ICDE, and CIDR, with specific focus on vector database and AI-era systems papers (ANNS, learned indexes, RAG infrastructure, LLM+DB systems, embedding pipelines). Use whenever the user asks to write, draft, outline, revise, polish, tighten, restructure, or improve any LaTeX paper targeting these venues — even when they just say "help me write the intro," "rewrite this section," "fix my motivation," or paste a .tex snippet without naming a venue. Also use when they mention "SIGMOD," "VLDB," "PVLDB," "ICDE," "CIDR," or describe a database/vector-search/AI-systems paper.
---

# Database Research Paper Writer (SIGMOD / VLDB / ICDE / CIDR)

This skill helps write and revise papers for top database venues, with a specific focus on vector databases and AI-era systems (ANNS, RAG infrastructure, LLM-powered DB tools, learned indexes, embedding-centric systems). It encodes the structural, rhetorical, and evaluation-framing conventions that reviewers at these venues expect.

## When to use

Use this skill any time the user is working on a LaTeX paper for a database venue. Triggers include:

- "Write/draft the intro/abstract/evaluation for my SIGMOD/VLDB paper"
- "Revise this section," "polish my draft," "tighten the motivation"
- Pasting a `.tex` snippet and asking for improvements
- Describing a paper about vector search, ANNS, vector DBs, RAG systems, LLM+DB, embeddings, learned indexes, or similar AI-era DB topics — even without naming a venue
- Requests for help with specific sections (abstract, intro, related work, system design, evaluation, conclusion)

Default assumption: the user has their own `.tex` file and template. Do not generate boilerplate ACM/PVLDB templates. Work with what they provide.

## Core workflow

### 1. Identify what the user needs

Before writing anything, figure out:

1. **Task type**: drafting from scratch, revising a draft, or targeted fix (one section, one paragraph)?
2. **Venue**: SIGMOD (ACM format, PACMMOD journal-style), VLDB/PVLDB (monthly rolling review, 12-page limit + unlimited refs), ICDE (IEEE format), or CIDR (visionary, shorter)? If unknown, ask. See `references/venues.md` for specifics.
3. **Paper type**: full research paper, industry paper, experiment/analysis/benchmark ([EA&B]), vision paper, or demo? These have different structures and expectations.
4. **Topic + contribution shape**: Is it a new system, a new algorithm, a new index, a benchmark, an analysis? The rhetorical template differs.
5. **Existing material**: Has the user provided their draft, an outline, experiment results, related work, or just an idea?

Read anything the user has uploaded (`.tex`, figures, result CSVs) before drafting. If they uploaded a `.tex` file, read it first to match their voice, notation, and macros.

### 2. Load the right reference

**Always read `references/structure.md` first** — it defines the single prescribed paper layout, section lengths, and subsection budget that every task in this skill must respect. This is non-negotiable.

Then, based on task type:

- **Drafting from scratch** → also read `references/drafting.md` for section-by-section templates and the intro formula.
- **Revising existing content** → also read `references/revising.md` for the common-weakness checklist and fix patterns.
- **Vector DB / AI-era systems topic** (any task) → also read `references/vector-db-conventions.md` for subfield-specific framing (ANNS terminology, baseline choices, metric conventions).
- **Venue-specific formatting or structural questions** → also read `references/venues.md`.

Read multiple reference files when the task spans multiple concerns (e.g., drafting a vector DB paper: read `structure.md` + `drafting.md` + `vector-db-conventions.md`).

### 3. Write or revise

Output LaTeX (`.tex` content) by default. Match the user's existing macros, notation, and labels when revising. When drafting new content, use clean standard LaTeX that assumes ACM `acmart` or PVLDB `vldb` class but doesn't depend on exotic packages.

**Output format preferences:**

- For full sections or papers: produce a `.tex` file in `/mnt/user-data/outputs/` and present it.
- For short revisions (a paragraph, a sentence, a caption): respond inline with the LaTeX, no file needed.
- For structured feedback on a draft: inline response with before/after snippets.

### 4. Apply the house style

Regardless of task, the writing must follow these DB-venue conventions:

- **Voice**: Mixed — "we" is standard and expected; don't avoid it, but don't overuse it either. Use active voice where it reads better. Passive is fine for describing system behavior ("queries are routed to the nearest cluster").
- **Tense**: Present tense for contributions and system behavior ("Tribase leverages...", "our system achieves..."). Past tense for experiments ("we evaluated...", "we observed..."). Present tense for related work ("FAISS supports...").
- **Avoid semicolons and dashes**: Do not use semicolons (`;`) or em/en dashes (`—`, `–`) in prose. Rewrite as separate sentences or use commas and conjunctions instead.
- **Precision over flourish**: No marketing language. No "revolutionary," "novel paradigm," "unprecedented." Reviewers hate it. Say what the thing does and how much better it is with numbers.
- **Numbers up front**: Claims of improvement should carry concrete numbers ("up to 10× speedup", "99.4% pruning ratio") in the abstract, intro, and conclusion.
- **Clear problem statement**: Every paper needs a precise problem statement, usually in Section 2 or early Section 3, using formal notation.
- **Explicit contributions list**: End the intro with a bulleted contributions list. This is non-negotiable at SIGMOD/VLDB.
- **Related work placement**: Per the canonical layout (`structure.md`), related work lives in §2 (Background & Related Work). Do not propose late placement unless the user explicitly requests it.

## Common pitfalls to actively prevent

When drafting or revising, watch for and fix these — they are the most common reasons DB papers get dinged:

1. **Vague motivation**: "Vector search is important" is not a motivation. The motivation must be a *specific gap* — what existing systems fail at, with a concrete example or number.
2. **Overclaiming novelty**: Don't say "first to..." unless truly first. "To the best of our knowledge" is acceptable but use sparingly. Reviewers will find prior art you missed.
3. **Weak baselines**: DB reviewers expect comparison against the strongest, most recent systems — not just textbook methods. For vector DBs this means FAISS, DiskANN, HNSW, SPANN, and any recent SIGMOD/VLDB/NeurIPS work directly relevant.
4. **Missing ablations**: If you claim N contributions, you typically need N ablations showing each one matters independently.
5. **Buried contributions**: The intro's contribution list should appear on page 1 or very early page 2. Not page 3.
6. **No Figure 1 on page 1**: DB papers almost always have an architecture or motivating-example figure on the first page. Include one if drafting from scratch.
7. **Evaluation without a story**: Results sections should be organized around *questions* ("Q1: Does Tribase outperform FAISS across datasets?"), not just dump-all-the-graphs.
8. **Underselling results**: If your system is 10× faster, the abstract should say "up to 10× faster," not "significantly faster."
9. **Related work as a list**: Don't just describe each prior paper. Group by approach and contrast with your work. Every related-work paragraph should end implicitly or explicitly with "...but [this limitation], which we address by [our approach]."

## Revising workflow

When revising an existing draft:

1. Read the entire draft first (don't skim). Form an overall impression before making edits.
2. Identify the paper's central contribution in one sentence. If you can't, that's the top problem — the draft isn't clear enough.
3. Check the pitfall list above systematically.
4. Propose edits in priority order: structural > argument > prose. A perfectly worded paragraph in the wrong place helps nobody.
5. When delivering edits, show diff-style before/after for key paragraphs. Don't silently rewrite.
6. Preserve the user's voice and technical choices. Don't substitute your preferred terminology for theirs if theirs is correct.

## Drafting workflow

When drafting from scratch:

1. Make sure you have: core idea, contribution shape (system with 2–4 components is the default), target venue, key results (even preliminary), and names of main baselines.
2. Confirm the paper fits the canonical system-paper layout in `structure.md`. If the user describes something that doesn't fit (e.g., pure theory paper, benchmark-only paper), flag this explicitly before drafting.
3. **Search for recent SIGMOD/VLDB papers on the same topic** (see "Searching for style exemplars" below). This grounds the draft in current venue conventions.
4. Draft in this order: abstract → contributions list → intro → §3 system overview → components (§4–§6) → evaluation → related work → conclusion. The abstract and contributions list come first because they discipline everything else.
5. For each section, follow the length budget and internal pattern defined in `structure.md`, using `references/drafting.md` for prose templates.
6. After drafting, audit against the `structure.md` checklist and the pitfall list.

## Searching for style exemplars

When drafting from scratch, automatically search for 2–3 recent SIGMOD/VLDB papers on the same topic **before writing**, to model structure and phrasing on current venue conventions. This step is mandatory for drafting tasks and skipped for revisions / small targeted fixes (the user already has a draft whose voice we should preserve).

**Scope rules:**

- **When to search**: drafting from scratch only (drafting an abstract, intro, full paper, or a complete section). **Do not** search for revisions, polishing, paragraph-level fixes, or when the user has uploaded their own draft — in those cases the user's voice is the reference.
- **Primary goal**: find recent exemplars whose structure, sentence rhythm, and framing the draft can emulate. The goal is **style and structure modeling**, not citation harvesting and not baseline discovery. Baselines come from `vector-db-conventions.md` and from the user.
- **Where to search**: venue proceedings only. SIGMOD: `https://sigmod.org/sigmod-record`, `https://dl.acm.org/conference/sigmod`, the annual proceedings pages (e.g., `https://YYYY.sigmod.org/sigmod_papers.shtml`). VLDB/PVLDB: `https://www.vldb.org/pvldb/`, `https://vldb.org/`. Do **not** search arXiv, Google Scholar, or broad web sources for this step — the goal is accepted, published venue exemplars.
- **Recency**: prefer the last 2 years. Older papers are acceptable only if they are canonical for the subfield (e.g., Faiss, HNSW, DiskANN foundational papers).
- **Volume**: 2–3 papers is the target. More is not better — the skill loses focus trying to emulate too many voices.

**Search procedure:**

1. Extract 2–3 short topic keywords from the user's description (e.g., for a tiered-memory vector DB: "vector database tiered memory," "ANNS disk-resident," "vector search CXL").
2. Run `web_search` with queries of the form `SIGMOD 20XX <keyword>` or `VLDB <keyword>` to find the accepted-papers pages.
3. When a relevant paper is found, `web_fetch` the PDF or abstract page. Skim the intro's opening paragraphs, the system-overview section, and any component-section openers to extract: how motivation is phrased, what kind of Figure 1 is used, how design goals are enumerated, and whether pseudocode/analysis is used.
4. Do **not** read full papers end-to-end; the skill is looking for style signal, not a literature review.

**How to use the results:**

- Match the **opening formula** of the intro (e.g., many recent vector DB papers open with a workload+scale sentence; match that register).
- Match the **Figure 1 style** (architecture vs. motivating example) if similar papers converge on one.
- Match the **contribution-list phrasing** — recent papers may favor "We identify..." / "We propose..." / "We design..." / "We evaluate..." or a different verb set; the draft should follow the venue's current rhythm.
- Match the **component-section opener** style — first-person vs. third-person, declarative vs. question-led.

**What to avoid:**

- Do **not** copy phrasing, sentence structure, or distinctive expressions from the exemplars. Modeling style ≠ paraphrasing. Reviewers often serve on PCs and will recognize lifted phrasing.
- Do **not** let the search produce a new set of baselines that overrides what the user specified.
- Do **not** pad the response by describing the exemplars back to the user. Mention them briefly (1–2 sentences on what the draft is emulating) and proceed to the draft.

**When the search finds nothing useful**: if no recent venue papers match the topic closely, proceed using the reference files alone. Note this to the user: "I didn't find close exemplars in recent SIGMOD/VLDB proceedings — drafting based on general venue conventions instead." Do not expand the search to arXiv or general web unless the user explicitly asks.

## Response letters and rebuttals

If the user asks for help with a response to reviewers, this skill can help — but the user said this wasn't a priority when the skill was designed. Handle inline: address each reviewer point, quote the concern verbatim, respond concretely (what changed, where, and why), and keep it under the venue's word limit (usually 500-1000 words per reviewer for VLDB revisions).

## Final checklist before delivering any draft

- [ ] Layout matches the canonical structure in `structure.md` (§1 intro, §2 bg+related, §3 overview, §4–§6 components, §7 eval, §8 conclusion)
- [ ] No separate Implementation section (details folded into components or §7.1)
- [ ] Fewer than 6 subsections total across §4–§6
- [ ] Contribution stated clearly in abstract, intro, and conclusion (and the three should agree)
- [ ] Contributions list appears in the intro, typically last paragraph before §2
- [ ] Numbers in abstract
- [ ] Figure 1 exists and is on page 1
- [ ] Every claim of improvement has a supporting number
- [ ] Related work contrasts (not just summarizes) prior systems
- [ ] Evaluation is organized by questions, each with an italicized takeaway
- [ ] Every claimed contribution has a matching evaluation question
- [ ] No marketing language
- [ ] Tense is consistent per the house-style rules above

---

## Reference files

- `references/structure.md` — **Canonical paper layout, section lengths, and subsection budget (read first)**
- `references/drafting.md` — Section-by-section prose templates for drafting from scratch
- `references/revising.md` — Common weaknesses and fix patterns
- `references/vector-db-conventions.md` — Vector DB / AI-era systems subfield conventions
- `references/venues.md` — Venue-specific formatting, length, and style notes
