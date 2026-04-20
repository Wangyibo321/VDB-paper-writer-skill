# Paper Structure: The Canonical Layout

Read this whenever the user asks about section organization, paper layout, length budgets, or is drafting a paper from scratch. This is the **canonical structure** for DB papers produced by this skill.

## The layout

This skill uses **one** layout: a system paper with multiple components. Do not propose alternatives unless the user explicitly asks for one.

```
   Abstract                         ~0.25 page
1. Introduction                     ~1 page
2. Background & Related Work        ~1–1.5 pages
3. System Overview & Design Goals   ~0.9 pages  (figure ~0.3p + text ~0.4–0.6p)
4. Component A                      ~1.25 pages
5. Component B                      ~1.25 pages
6. Component C                      ~1.25 pages
7. Evaluation                       ~2.5–4 pages (figures + tables included)
8. Conclusion                       ~0.25 page
                                    ──────────────
                                    ~10–12 pages + unlimited references
```

**Page lengths throughout this document are references, not strict constraints.** They describe the typical shape of a well-proportioned DB paper. Adapt to the material — a simple component doesn't need to be padded to 1.25 pages, and a rich evaluation can run longer. The hard constraints are the paper-wide page budget (12 pages), the section ordering, and the subsection cap (fewer than 6 across §4–§6). Everything else is guidance.

**No separate Implementation section.** Implementation details belong inside the component sections (where the design is described) or in the evaluation setup (hardware, libraries, open-source URL). A standalone "Implementation" section is a structural smell in this skill's output.

## Rules for components

- **Component count**: 3 is typical and the default. 2 or 4 are acceptable when the paper genuinely demands it. Never exceed 4. Never use 1 (if there's only one component, it's an algorithm paper, not a system paper — flag this to the user and ask if they want to rethink the framing).
- **Component length**: ~1.25 pages each. A component substantially shorter than 1 page is either trivial (merge it) or under-described (expand it). A component longer than 1.75 pages needs subsection breakdown or trimming.
- **Component ordering**: order by dependency. If B uses machinery defined in A, A comes first. If components are independent, order by centrality to the contribution (most central first).

## Rules for subsections

**Hard constraint**: across all component sections combined (§4 + §5 + §6), there must be **fewer than 6 subsections total**. Each component gets 0–3 subsections. Average is therefore ~1–2 subsections per component.

This means:
- A component can have no subsections (just a flowing section body). This is fine and often best for a tight component.
- A component with 3 subsections is the ceiling. If you find yourself wanting 4, something is wrong — either the subsections are too granular, or the component should be split into two components.
- Count carefully. If §4 has 2 subsections, §5 has 2, and §6 has 2, that's 6 total — **over the limit**. Cut one.

**Why this rule matters**: reviewers skim the table of contents. If §4–§6 has 9+ subsections, the paper looks cluttered and the contribution story gets lost in administrative headings. Fewer subsections force tighter prose.

**Subsection length**: if a subsection exists, it should be at least ~0.4 pages. Anything shorter is a paragraph, not a subsection — promote it to a paragraph break with a bolded lead-in (`\textbf{Key idea.}` or `\paragraph{Key idea.}`) instead.

## Length discipline — how to hit the budget

DB venue page limits are 12 pages + references. This layout totals ~10–12 pages, which gives a small buffer. The budget is tight, and exceeding any single section usually forces cuts elsewhere. Typical failure modes and fixes:

| Failure mode | Symptom | Fix |
|---|---|---|
| Intro bloat | >1.5 pages | Move general background to §2; keep intro focused on motivation + contributions |
| §2 bloat | >1.5 pages | Cut broad background; related work paragraphs should be tight (1 per approach family) |
| §3 bloat | >1 page | The overview repeats the intro — tighten or merge |
| Component bloat | >1.75 pages | Break into subsections (but mind the 6-total cap) or move details to appendix |
| Evaluation too short | <2.5 pages | Add ablations, scaling studies, or more baselines — a thin eval is a rejection risk |
| Conclusion bloat | >0.25 page | Cut future work or recap; conclusion is a coda, not a second intro |

## Section-by-section writing logic

This section defines what each section must do. Use this when drafting or when auditing an existing paper.

### Abstract (~0.25 page, 150–250 words)

Single paragraph. No headings. See `drafting.md` Section 1 for the 5-sentence formula.

Opens with concrete area/workload, names the gap, introduces the system, lists 2–3 key techniques, ends with a headline number.

### §1 Introduction (~1 page)

This is tight — ~1 page means roughly 5 paragraphs, each ~0.2 page:

- **P1 — The world** (the problem area with a concrete workload and why the scale or latency matters)
- **P2 — Existing approaches and the gap** (two or three approach families, with a specific numerical gap)
- **P3 — Our approach** (name the system, state the key insight, reference Figure 1)
- **P4 — Challenges** (2–3 challenges with one-line teaser fixes)
- **P5 — Contributions** (bulleted list, 3–5 bullets, each an insight not an activity)

**Figure 1 is non-negotiable** and appears on page 1 or very early page 2. Usually the system architecture or a motivating example.

### §2 Background & Related Work (~1–1.5 pages)

Dual-purpose: give the minimum background needed to read the paper, and position it among related work. Structure:

- **2.1 Background (~0.3–0.5 page)**: Core concepts and notation. Only what the reader needs. No textbook material.
- **2.2 Related work (~0.7–1 page)**: Group by approach family, one paragraph per family. Each paragraph: name family → describe mechanism → note strengths → identify limitation your paper addresses.

Typical families to cover in a vector DB paper: graph-based indexes (HNSW/DiskANN), quantization-based indexes (IVFPQ/ScaNN), partition/cluster-based indexes (IVF variants), and one paragraph on the sub-problem your paper targets (filtered search, streaming, disk-resident, etc.).

Do not list papers one at a time. Never write "Author et al. [X] did Y." — instead: "Graph-based indexes [A, B, C] construct proximity graphs and traverse via greedy search..."

### §3 System Overview & Design Goals (~0.9 page)

The architecture figure (~0.3 page) does most of the work here. Text should be ~0.4–0.6 page and cover:

- **Architecture**: one or two paragraphs walking through the figure. Name every component. Describe data flow.
- **Design goals**: numbered list (G1, G2, ...) — typically 3 goals that motivate the component choices. Each goal in one sentence.
- **Roadmap**: a single sentence pointing to the component sections: "Sections §4–§6 describe [Component A], [Component B], and [Component C] in turn."

Do not rehash the intro. This section is more detailed and concrete than the intro's paragraph 3 — it names components, interfaces, assumptions. If it feels repetitive of the intro, the intro is too long or the overview is too shallow.

### §4–§6 Components (~1.25 pages each, 3 by default)

Every technical section follows **motivation → mechanism → analysis → example**, in that order. The page lengths below are **references, not strict constraints** — adapt to the material.

1. **Motivation** (~0.2 page, **mandatory**): Why does this technique exist? What inefficiency does it attack? What does this component solve that §3 couldn't?
2. **Mechanism** (~0.5–1 page, **mandatory**): How does it work, formally? Pseudocode, definitions, and formulas are **optional** — use them when they clarify, omit them when prose is clearer.
3. **Analysis** (~0.25 page, **optional**): What can we prove about it? Complexity? Correctness? Include when the technique has non-obvious cost or correctness properties; skip when analysis would be filler.
4. **Example** (small, **optional**): A worked example or a figure tying back to the earlier motivating scenario. Include when the mechanism is abstract enough that a concrete walk-through cements understanding; skip when the mechanism is already concrete.

A minimal component has just motivation + mechanism. A fuller component adds analysis and/or example. The ~1.25 page target is a reference point, not a ceiling — some components will be tighter, some longer, and that's fine as long as the paper-wide page budget holds.

**When to include analysis**: any time the technique has (a) non-trivial complexity worth bounding, (b) a correctness property that isn't self-evident, or (c) a guarantee that anchors the contribution (e.g., "bounded recall loss," "lossless pruning"). If the component's contribution claim in the intro relies on a property, analysis is where that property gets discharged.

**When to include an example**: when the mechanism involves indices, references, or indirection that's hard to follow in prose. A tie-back to Figure 1's motivating scenario is usually the cheapest option — it reuses visual context the reader already has.

A component can have 0–3 subsections. If the component is small and clean, no subsections — just the parts above as flowing paragraphs. If the component has multiple sub-techniques, each sub-technique can be a subsection, but remember the **global cap of 5 total subsections across §4–§6** (strictly less than 6).

**Subsection naming**: use descriptive names, not generic ones. "Selectivity estimation" is good; "Approach" or "Method" is bad.

### §7 Evaluation (~2.5–4 pages including figures and tables)

This is the largest single section and the most scrutinized. Structure:

- **§7.1 Experimental setup (~0.5 page)**: hardware, software, datasets (table), baselines (named with citations), metrics, workloads. This is also where minor implementation details go (language, libraries, open-source URL) — **there is no separate Implementation section**.
- **§7.2 through §7.N Experiments organized by QUESTION**: each subsection titled with a question (Q1, Q2, ...). Subsection opens with the question in bold, runs the experiment, ends with an italicized takeaway.

**The experiment questions should map 1-to-1 with the contributions**. If the intro lists 4 contributions, the evaluation should have ~4 main questions plus supporting ablation/scaling questions. Typical question set:

- Q1: Does the system outperform state-of-the-art baselines end-to-end? (main comparison)
- Q2: How does each component contribute? (ablation — one per component)
- Q3: How does the system scale? (vs. N, d, k, or workload parameter)
- Q4: What is the index build / memory overhead? (resource cost)
- Q5: Does the system generalize across workloads / datasets? (robustness)

**Evaluation rules**:
- Every latency number must have error bars or CI across ≥3 trials
- Main comparison must be at matched recall (not cherry-picked operating points)
- Every claimed contribution needs an ablation
- Lead with the strongest result (main comparison), then ablations, then scaling, then microbenchmarks

Figures and tables count toward the page budget. A typical eval has 4–6 figures and 1–2 tables.

### §8 Conclusion (~0.25 page)

Tight. 3–5 sentences:
1. One sentence on the problem.
2. One sentence on the approach.
3. Two sentences on key results (with numbers, matching the abstract).
4. Optional: one sentence on future work.

Never introduce new content. Never apologize or hedge. Do not write "In this paper we have presented..." — just state what the paper did.

## Audit checklist

When revising or finalizing a paper, check it against this layout:

- [ ] Abstract is ~0.25 page, ends with a headline number
- [ ] §1 is ~1 page, 5 paragraphs, contribution list present
- [ ] Figure 1 appears on page 1 or early page 2
- [ ] §2 is ~1–1.5 pages, related work grouped by family (not listed per paper)
- [ ] §3 is ~0.9 page total, architecture figure included, does not rehash intro
- [ ] §3 includes numbered design goals
- [ ] 2–4 component sections, ~1.25 pages each
- [ ] Each component has motivation → mechanism → analysis → example
- [ ] **Total subsections across component sections is strictly less than 6**
- [ ] No separate Implementation section (fold details into components or §7.1)
- [ ] §7 is 2.5–4 pages, organized by question, each Q has a takeaway
- [ ] Every contribution has a matching evaluation question
- [ ] §8 is ≤0.25 page, no new content
- [ ] Total ~10–12 pages before references

## When the user pushes for a different layout

If the user asks for a layout that deviates from this (e.g., "I want a separate Implementation section" or "I have 5 components"), don't silently accept. Note the deviation and its cost:

- 5+ components → "This usually means the paper is trying to claim too many contributions. Consider grouping into 3–4 components, or flagging the weaker ones as 'extensions' inside a component section."
- Separate Implementation section → "Implementation details usually fit better inside the component sections (where the design is described) or in §7.1 (setup). A standalone Implementation section tends to be padding. Is there a specific reason you want it separate?"
- Related work moved to §7 or §8 (late placement) → "This works for algorithm papers with heavy theory, but for system papers the reader needs related work early to understand the design space. Keep it in §2 unless you have a specific reason."

Apply the user's preference if they confirm, but make the tradeoff explicit.
