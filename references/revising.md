# Revising Database Papers: Weaknesses and Fix Patterns

Use this reference when the user hands you an existing draft and asks for revision, polish, or restructure. The workflow is: read → diagnose → prioritize → fix.

## Diagnostic reading order

1. Read the **abstract** and **contributions list** first. Can you state the paper's main contribution in one sentence? If not, the paper has a clarity problem at the top.
2. Read the **intro** end-to-end. Note where it drags or where you'd stop caring.
3. Skim the **evaluation section's questions/subsection titles**. Do they map to the claimed contributions? If not, there's a structural mismatch.
4. Read the **conclusion**. Does it match the abstract? Mismatches between abstract, intro, and conclusion are a common signal that contributions shifted during writing.
5. Then read the body in order.

## Priority order for fixes

Always fix in this order:

1. **Structural** issues (wrong section order, missing problem statement, unclear contribution) — fixing these invalidates smaller edits, so do them first.
2. **Argument** issues (weak motivation, missing ablation, unconvincing baseline choice).
3. **Prose** issues (wordiness, tense inconsistency, awkward phrasing).

Don't polish prose in a paragraph that needs to be cut or moved.

## Common weaknesses and fixes

### W1: Vague motivation

**Symptoms:**
- Intro says "X is important" or "X has become crucial" without specifics.
- No numbers, no concrete application, no named workload.
- Motivation could apply to any paper in the area.

**Fix pattern:** Force specificity. Replace abstract claims with:
- A specific workload (e.g., "RAG pipelines serving 10K queries/sec at a 100ms SLO")
- A concrete number (latency target, scale, cost)
- A named application or system that exhibits the pain

**Before:**
> Vector search has become increasingly important for modern applications. However, existing methods have limitations.

**After:**
> Production RAG pipelines routinely serve billion-scale embedding indexes with sub-100ms latency SLOs~\cite{X}. At this scale, graph-based indexes such as HNSW~\cite{Y} achieve high recall but exhibit memory footprints 2–3× larger than the raw vectors, making them prohibitively expensive to host.

### W2: Overclaiming novelty

**Symptoms:**
- "First to...", "novel", "revolutionary", "paradigm shift"
- Claims of "new problem" when the problem has been studied
- No acknowledgment of close prior work

**Fix pattern:** Replace novelty claims with *differentiation* claims. State what prior work did and what you do differently, with a specific contrast.

**Before:**
> We are the first to address this problem.

**After:**
> While prior work~\cite{A,B} addresses this problem for the uniform-distribution case, our approach is the first to handle skewed distributions without loss of recall.

Even better: sometimes the right move is to drop the novelty claim entirely and let the contribution stand on its merits.

### W3: Weak or missing baselines

**Symptoms:**
- Only textbook baselines (e.g., linear scan, basic k-d tree)
- Baselines from 2015 when the topic has had 5 SIGMOD/VLDB papers since
- Missing the most obvious competitor

**Fix pattern:** For vector DBs / ANNS in particular, reviewers will expect at minimum FAISS (IVF, IVFPQ, HNSW variants), DiskANN, and one or two recent venue papers relevant to the contribution. If the user doesn't have these, flag it explicitly as a high-risk issue for reviewer acceptance and suggest running the experiments.

### W4: Buried contribution

**Symptoms:**
- Contribution only becomes clear on page 4
- Contribution list is missing or weakly worded
- Contributions are implementation details, not insights ("we implemented X in C++")

**Fix pattern:** Rewrite the contribution list so each bullet is an *insight* or *technique*, not an implementation activity.

**Before:**
> - We implement our system in C++.
> - We run experiments on three datasets.
> - We show our system works well.

**After:**
> - We identify that [specific phenomenon], which causes [specific inefficiency] in existing cluster-based indexes (Section 3).
> - We propose [technique], the first method to [what it does] without [cost/loss] (Section 4).
> - We conduct experiments on 10 datasets, showing up to 10× speedup over FAISS at the same recall (Section 7).

### W5: Evaluation without a story

**Symptoms:**
- Subsections titled "Experiment 1", "Experiment 2"
- Graphs dumped without clear takeaways
- No ablation or unclear ablation
- No variance reported for latency metrics

**Fix pattern:** Rewrite evaluation subsection headers as questions (Q1, Q2, ...). End each subsection with a one-sentence italicized takeaway. Ensure each claimed contribution has a corresponding ablation.

### W6: Related work as a list

**Symptoms:**
- Paragraphs shaped "Author et al. [X] did Y. Author et al. [Z] did W."
- No grouping by approach
- No contrast with the current paper

**Fix pattern:** Reorganize into categories. Each category gets one paragraph that: (a) names the approach family, (b) describes the mechanism, (c) notes strengths, (d) identifies the limitation that motivates the current paper's approach. Never just list.

### W7: Missing Figure 1 / weak Figure 1

**Symptoms:**
- No figure on page 1 or 2
- Figure 1 is a decorative block diagram that could describe any paper
- Architecture figure but no motivating example

**Fix pattern:** For vector DB / systems papers, a motivating example showing "here's the pain point" on page 1 or 2 lets reviewers visualize the contribution immediately. If missing, suggest adding one and offer to describe what it should show.

### W8: Unsupported claims

**Symptoms:**
- "significantly faster" without a number
- "our approach scales" without showing scaling curves
- "low overhead" without quantifying the overhead

**Fix pattern:** Every qualitative claim in the abstract, intro, and conclusion must have a number attached. When revising, grep the draft for weak qualifiers: "significantly", "dramatically", "substantially", "efficiently", "effectively" — each one is a candidate for replacement with a number.

### W9: Tense and voice drift

**Symptoms:**
- Mixing "we proposed" and "we propose" for the same contribution
- Passive voice everywhere making it hard to identify actors
- Related work in past tense ("FAISS supported...") when it's a live system

**Fix pattern:** Normalize:
- **Present tense** for: contributions, system behavior, related work (for living systems), figure/table captions
- **Past tense** for: experiments conducted, observations made ("we measured", "we found")
- **Present tense** for: equations, formal statements

### W10: Wordiness and LaTeX slop

**Symptoms:**
- Sentences over 40 words
- "In order to" instead of "to"
- "Due to the fact that" instead of "because"
- Manual line breaks / hspace hacks to fit page limits
- Inconsistent use of `~\cite{}` vs `\cite{}` (citations should have non-breaking space)

**Fix pattern:** Cut ruthlessly. If page-limit hacks are present, flag them — reviewers notice and it signals the paper is over budget. Cutting content is better than shrinking margins.

### W11: Missing problem statement

**Symptoms:**
- No formal definition of inputs/outputs
- Ambiguity about what "correctness" means for the system
- Reviewer has to infer what the paper is solving

**Fix pattern:** Add a subsection (typically 3.1 or end of Section 2) titled "Problem statement" with a `\begin{definition}` block. See `drafting.md` Section 4 for a template.

### W12: Contribution/evaluation mismatch

**Symptoms:**
- Intro claims 4 contributions but evaluation only validates 2
- Ablation skips one of the claimed techniques
- A contribution is claimed but never shown to matter

**Fix pattern:** List the contributions from the intro and match each to an experiment subsection. Identify gaps. Either add an experiment or remove the contribution (both are valid moves).

## How to deliver revisions

When the user wants revision help, deliver in this format:

1. **Overall assessment** (2–4 sentences): what's working, what's the top problem.
2. **Prioritized list of issues** (labeled by weakness W1–W12 above): one line each, referencing the section/paragraph.
3. **Concrete edits** for the top 2–3 issues: show before/after in LaTeX.
4. **Quick wins**: small prose fixes the user can accept or reject individually.
5. **Flag anything you can't fix without more info** (e.g., "you'll need to add an ablation for technique X — I can draft the subsection structure if you share the experimental data").

Don't silently rewrite the paper. Show the user what you're changing and why. They need to preserve their voice and technical judgment.

## When to push back

If the user asks you to do something that will hurt the paper:
- "Add more adjectives to make it sound impressive" → push back; reviewers dislike it.
- "Remove the ablation to save space" → push back; explain the reviewer risk.
- "Add a 'first to' claim" when there's close prior work → push back firmly.

Explain the risk in terms of reviewer response, not your preferences.
