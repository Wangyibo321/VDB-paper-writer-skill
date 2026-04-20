# Contributing Guide

## What to contribute

### High-frequency maintenance (update as the field evolves)

**`references/vector-db-conventions.md`**
- **Mandatory baselines**: when a new SIGMOD/VLDB paper becomes a standard comparison point, add it
- **Standard datasets**: add widely adopted new benchmarks
- **AI-era systems themes**: refresh the hot topics list annually

**`references/venues.md`**
- Page limits and anonymity requirements occasionally change — verify against the current call before each submission cycle

### Extensions (add when there is a real need)

**New reference files**

Currently missing files that would be useful to add:

| File | Content |
|---|---|
| `references/rebuttal.md` | Reviewer response / rebuttal writing guide |
| `references/ea-and-b.md` | Experiments, Analysis & Benchmark paper structure |
| `references/vision-paper.md` | Vision / position paper writing conventions (primarily for CIDR) |
| `references/figures.md` | Figure design standards (recall-latency curves, ablation bar charts, etc.) |

When adding a new reference file:
1. Place it in `references/`
2. Add load logic in the "Load the right reference" section of `SKILL.md`
3. Add a description row in the "Reference files" table in `README.md`

**New weakness patterns in `references/revising.md`**

Follow the existing format:

```markdown
### W13: [Weakness name]

**Symptoms:**
- [Observable symptom]

**Fix pattern:** [How to fix]

**Before:**
> [Typical bad example]

**After:**
> [Corrected version]
```

Increment the number from the current maximum.

**Changes to `SKILL.md` workflow**

Before editing, understand the existing design:
- "Load the right reference" controls which references are loaded for which tasks — keep the logic consistent
- "Common pitfalls" and `revising.md` W1–W12 are paired — update both when adding a new pattern
- Every item in "Final checklist" should trace back to a rule in one of the reference files

---

## Workflow

```bash
# 1. Clone and create a branch
git clone https://github.com/Wangyibo321/VDB-paper-writer-skill
git checkout -b improve/update-baselines

# 2. Edit references/ or SKILL.md
vim references/vector-db-conventions.md

# 3. Build and test locally
make build
make install   # installs to ~/.claude/skills/
# Test in Claude Code with the prompts below

# 4. Commit
git add references/vector-db-conventions.md
git commit -m "update: add ACORN and NHQ to filtered-ANNS baselines"
git push origin improve/update-baselines

# 5. Open a PR with a short description of what changed and why
```

---

## Testing

After any change, verify the skill with these prompts:

```
# Test drafting flow
Draft a SIGMOD paper on disk-resident vector indexing. The system is called DiskVec.
The core contribution is a new graph pruning strategy.

# Test revision flow
[Paste a flawed intro paragraph] — review this intro and identify the problems.

# Test vector DB specialization
My paper is about filtered ANNS. Which baselines do I need to compare against?

# Test venue formatting
I am submitting to PVLDB. What is the page limit? Is it double-blind?
```

---

## PR checklist

- [ ] The change is justified by a real development in the field or a real mistake encountered during paper writing
- [ ] No AI-generated filler or marketing language introduced
- [ ] If a new reference file was added, both `SKILL.md` and `README.md` are updated
- [ ] The change has been tested in Claude Code if possible

---

## Reporting issues

Open a GitHub issue with:
1. The task that was triggered (drafting / revising / formatting check)
2. What the skill produced vs. what was expected
3. The input you provided (redact confidential content if needed)
