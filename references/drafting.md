# Drafting Database Papers: Section-by-Section Guide

Use this reference when drafting a new paper or a new section from scratch. Each section below gives the rhetorical template and a concrete LaTeX skeleton.

## Table of contents

This reference follows the canonical layout defined in `structure.md`:

1. Abstract
2. Introduction
3. Background and related work (§2)
4. Problem formulation (goes inside §2.1 or opens §4)
5. System overview and design goals (§3)
6. Component sections (§4–§6)
7. Evaluation (§7)
8. Conclusion (§8)

Implementation details are folded into the component sections or §7.1 — there is no standalone Implementation section. Related work always lives in §2; there is no late-placement variant.

---

## 1. Abstract

**Formula** (roughly 150–250 words):

1. One sentence on the area and why it matters (no fluff — tie to a concrete trend or workload).
2. One or two sentences on the specific problem / gap in existing work.
3. One sentence stating the paper's approach in plain terms.
4. Two or three sentences on key technical ideas (not all details — the headlines).
5. One sentence with concrete quantitative results ("up to X× faster," "Y% improvement," "reduces Z by W%").

**Template:**

```latex
\begin{abstract}
[Area sentence]. [Specific problem sentence with concrete pain point].
Existing approaches [brief characterization] but [limitation].
In this paper, we present \sysname{}, a [system/algorithm/index] for [task].
\sysname{} introduces [key idea 1], [key idea 2], and [key idea 3].
[Optional: one more sentence elaborating the most important idea].
Experiments on [N] datasets show \sysname{} achieves up to [X]× speedup over
[baseline] while [maintaining accuracy / reducing memory / etc.].
\end{abstract}
```

**Common abstract mistakes:**
- Starting with "In recent years..." (cliché, cut it)
- Not naming the system
- Vague results ("significant improvement") — always give numbers
- Listing too many contributions (max 3-4 in abstract)

---

## 2. Introduction

The intro is where papers are accepted or rejected. Reviewers decide in the first 2 pages. Follow this 5-paragraph structure:

**Paragraph 1 — The world.** Set up the problem area concretely. Use a workload, application, or trend. One or two citations. End with why this matters (e.g., latency, cost, scale).

**Paragraph 2 — Existing approaches and their gap.** Name the families of prior approaches (e.g., "graph-based indexes [A,B,C]", "cluster-based indexes [D,E,F]"). Describe what they do well. Then identify a *specific, concrete gap* — not just "more work is needed." Ideally this gap is backed by a number or observation (e.g., "cluster-based indexes spend 99.7% of query time on distance computations").

**Paragraph 3 — Our approach and key insight.** Introduce the system name. State the key insight or technique in plain English. Don't go into mechanics yet. Often this includes a small motivating figure reference (Figure 1).

**Paragraph 4 — Challenges.** List 2–3 non-trivial technical challenges that had to be solved. This shows the problem is hard and the paper has substance. Each challenge should have a one-sentence teaser of how the paper addresses it.

**Paragraph 5 — Contributions and results.** Bulleted contributions list (3–5 bullets), followed by one sentence on headline results. This paragraph often ends with a roadmap sentence ("The rest of this paper is organized as follows...") — this is optional and increasingly omitted in tight 12-page papers.

**LaTeX template:**

```latex
\section{Introduction}
\label{sec:intro}

% Paragraph 1: The world
[Concrete workload/trend with citation]. For example, [specific application]
generates [scale number]~\cite{X}. This has driven rapid adoption of
[technology], where [specific requirement like low latency / high recall]
is critical.

% Paragraph 2: Existing approaches and gap
Existing [technology] systems fall into two main families:
\textit{[family 1]}~\cite{A,B,C} and \textit{[family 2]}~\cite{D,E,F}.
The former [what it does well], while the latter [what it does well].
However, both suffer from [specific gap]. As we show in Section~\ref{sec:motivation},
[concrete observation with number].

% Paragraph 3: Our approach
In this paper, we present \sysname{}, a [system type] that addresses this gap
through [one-sentence key insight]. Figure~\ref{fig:overview} illustrates the
high-level architecture. The core idea is [plain-English version of technique].

% Paragraph 4: Challenges
Realizing this idea presents three key challenges.
\textbf{First}, [challenge 1]. We address this by [teaser of technique 1].
\textbf{Second}, [challenge 2]. We tackle this through [teaser of technique 2].
\textbf{Third}, [challenge 3]. Our solution [teaser of technique 3].

% Paragraph 5: Contributions
We summarize our contributions as follows:
\begin{itemize}
    \item We identify [insight or gap], backed by [analysis/measurement].
    \item We propose \sysname{}, which [main technical contribution in one clause].
    \item We design [specific technique or algorithm] that [what it enables].
    \item We conduct extensive experiments on [N] datasets, demonstrating
          [headline result: X× speedup / Y\% improvement] over state-of-the-art
          baselines including [Baseline1]~\cite{} and [Baseline2]~\cite{}.
\end{itemize}
```

**Figure 1 conventions.** Almost every accepted SIGMOD/VLDB paper has a figure on page 1 — either:
- An **architecture overview** of the system, or
- A **motivating example** (small scenario showing the problem visually), or
- A **headline result** chart (rare but sometimes used for experimental papers).

If drafting from scratch, plan for one of these. For vector DB papers, a motivating example showing "query vector + clusters + what gets pruned" is a classic choice.

---

## 3. Background and related work

Per the canonical layout (`structure.md`), background and related work both live in §2. Structure:

- **§2.1 Background**: minimum necessary concepts and notation
- **§2.2+ Related work**: one subsection per approach family

**Related work principles:**

1. **Group by approach, not by paper.** Don't write a paragraph per cited paper. Write a paragraph per family of techniques.
2. **Contrast, don't summarize.** Each paragraph should end with what's missing or suboptimal, implicitly setting up what your paper does.
3. **Cite recent work.** For vector DBs and AI-era systems, the field moves fast — related work more than 3 years old is often considered foundational, and the paper needs 2024–2025 citations to show current awareness.
4. **Don't badmouth.** Describe limitations neutrally ("does not address X", "assumes Y"), never dismissively.

**Template:**

```latex
\subsection{[Approach family name]}
[Approach family] methods~\cite{A,B,C} [core description of what they do and
key mechanism]. [Prior work 1]~\cite{A} [specific technique]. [Prior work 2]
~\cite{B} extended this by [extension]. These methods work well for [scenario]
but [limitation relevant to your work]. In contrast, our approach [brief
distinction — one sentence, don't go deep here].
```

---

## 4. Problem formulation

Include this as a short subsection (often Section 3.1 or end of Section 2). DB reviewers expect formal definitions.

**Template:**

```latex
\subsection{Problem statement}
\label{sec:problem}

Let $\mathcal{D} = \{v_1, v_2, \ldots, v_N\}$ denote [data description],
where each $v_i \in \mathbb{R}^d$ [any relevant properties].
Given a query $q \in \mathbb{R}^d$ and an integer $k$, the [task name]
problem is to find [formal definition of the output].

\begin{definition}[\textbf{[Task name]}]
Given $\mathcal{D}$, $q$, and $k$, return the set
$\mathrm{Top}_k(q, \mathcal{D}) = \arg\min_{S \subseteq \mathcal{D}, |S|=k}
\sum_{v \in S} \mathrm{dist}(q, v)$.
\end{definition}

In practice, exact [task] is infeasible at scale, so we consider approximate
[task] with recall $R@k = \frac{|S \cap S^*|}{k}$, where $S^*$ is the exact
top-$k$ set.
```

Use `definition`, `theorem`, `lemma`, `proposition` environments consistently. Include a **notation table** (often Table 1) for papers with many symbols.

---

## 5. System design / overview

Section 3 or 4 is usually the high-level design.

**Structure:**
- **3.1 Overview.** One or two paragraphs. Refer to an architecture figure. Describe the components and data flow.
- **3.2 Design goals / requirements.** What the system must achieve, often as numbered goals (G1, G2, ...).
- **3.3 Key ideas.** Preview of the techniques that will be detailed in subsequent sections. Each should be a paragraph or short subsection.

**Architecture figure conventions:**
- Boxes for components, arrows for data flow
- Label components with names that match the paper's terminology exactly
- Color coding is fine but should work in grayscale
- Caption should name every component referenced in the text

---

## 6. Core algorithm or technique

Sections 4–6 dive into the components. Each component section follows **motivation → mechanism → analysis → example**, in that order. Per `structure.md`, only the first two are mandatory; analysis and example are optional.

1. **Motivation** (~0.2 page, mandatory). Why does this technique exist? What inefficiency does it attack?
2. **Mechanism** (~0.5–1 page, mandatory). How does it work, formally? Pseudocode, definitions, and formulas are optional tools — use them when they clarify, omit when prose is clearer.
3. **Analysis** (~0.25 page, optional). Correctness, complexity, or guarantees. Include when the technique has non-obvious cost/correctness properties or when the contribution relies on a provable property.
4. **Example** (small, optional). A worked example or tie-back to Figure 1's scenario. Include when the mechanism is abstract enough that a concrete walk-through helps.

Page lengths above are references, not strict constraints — see `structure.md` for the length-discipline framing.

**Algorithm block template** (only when pseudocode aids clarity):

```latex
\begin{algorithm}[t]
\caption{\textsc{[AlgorithmName]}}
\label{alg:name}
\KwIn{[inputs with types]}
\KwOut{[outputs with types]}
[pseudocode lines]
\end{algorithm}
```

Use the `algorithm2e` or `algorithm` + `algorithmic` packages — whichever the template uses.

---

## 7. Implementation details

**There is no standalone Implementation section** in the canonical layout. Implementation details go in one of two places:

- **Inside the relevant component section** when the detail is specific to that component (e.g., "we use SIMD intrinsics for the distance computation in Component B")
- **In §7.1 Experimental setup** for cross-cutting details: language, libraries, integration points (e.g., "built on top of FAISS"), open-source availability URL

If you find yourself wanting a separate Implementation section, the details are probably either (a) important enough to live with the component they describe, or (b) not important enough to waste a section on — in which case they belong in §7.1 or can be cut entirely.

---

## 8. Evaluation

This is where DB papers succeed or fail at the review stage. Structure:

**§7.1 Experimental setup.**
- Hardware (CPU model, RAM, disk type, GPU if used)
- Software stack + implementation details (language, libraries, integration points, open-source URL)
- Datasets: table with name, size, dimension, cardinality, source citation
- Baselines: list each with citation and brief description
- Metrics: latency, throughput, recall, memory, index build time, etc.
- Workloads: query distributions, parameter sweeps

**§7.2+ Experiments organized by QUESTION.** This is critical. Don't just list experiments — frame each as a question.

```
Q1: Does \sysname{} outperform state-of-the-art on end-to-end query latency?
Q2: How does each component contribute to the overall speedup?  (ablation)
Q3: How does \sysname{} scale with dataset size / dimension / k?
Q4: How does \sysname{} perform under varying recall targets?
Q5: What is the index construction overhead?
```

Each Q gets a subsection with figures, tables, and a paragraph of findings. End each with an explicit **takeaway** sentence in italics or bold: "*Takeaway: \sysname{} achieves consistent speedups across all datasets, with the largest gains on high-dimensional data where pruning is most effective.*"

**Evaluation pitfalls:**
- Graphs without error bars on latency (always include variance)
- Missing baselines (the strongest, most recent baseline must be there)
- Cherry-picking datasets — use standard benchmarks (SIFT1M, GIST1M, DEEP1B, MS MARCO for text, BigANN, LAION for multimodal)
- Missing ablations — every contribution needs an ablation showing it matters

---

## 9. Conclusion

Short (~0.25 page):
1. One sentence recapping the problem.
2. One sentence recapping the approach.
3. Two sentences on key results (numbers again).
4. One sentence on future work (optional — many DB papers skip this).

Do not introduce new content in the conclusion. Do not apologize or hedge. Confidence here.

---

## Macros to suggest

If the user's template allows, these macros reduce effort and improve consistency:

```latex
\newcommand{\sysname}{\textsc{Tribase}}   % replace with actual system name
\newcommand{\eg}{e.g.,\xspace}
\newcommand{\ie}{i.e.,\xspace}
\newcommand{\topk}{top-$k$\xspace}
\newcommand{\vect}[1]{\mathbf{#1}}        % bold vectors
```

(Requires `\usepackage{xspace}`.)
