# Vector Database and AI-Era Systems: Subfield Conventions

This reference captures the specific conventions, terminology, and reviewer expectations for vector DB and AI-era systems papers at SIGMOD, VLDB, ICDE, and CIDR. Read this whenever the user's paper involves ANNS, vector indexes, RAG infrastructure, embedding pipelines, LLM+DB integration, learned indexes, or similar topics.

## Terminology — use these exact terms

- **ANNS** (Approximate Nearest Neighbor Search) — not "approximate NN" or "vector search" when precision matters
- **Recall** @ k, written as $R@k$ or recall@k — the fraction of true top-k returned
- **QPS** (queries per second) — throughput metric; always paired with a recall target
- **Index build time** — not "training time" (reserve that for ML models)
- **Embedding** — preferred over "vector" when the data is from a learned model
- **Query vector** / **data vector** / **base vector** — pick one and stay consistent
- **Recall-latency tradeoff curve** or **recall-QPS curve** — the standard evaluation plot
- **Distance metric** — L2, inner product (IP), cosine; state which and why
- **Dimensionality** $d$ — always state
- **Cardinality** $N$ — number of vectors in the index

## Mandatory comparisons

For any vector DB / ANNS paper, reviewers will expect at minimum:

**Baselines (most recent versions):**
- **FAISS** (Johnson et al.) — IVF, IVFPQ, HNSW variants
- **HNSW** (Malkov & Yashunin) — graph-based, in-memory
- **DiskANN** / **Vamana** (Subramanya et al.) — disk-resident graph index
- **SPANN** (Chen et al.) — hybrid memory-disk
- **ScaNN** (Guo et al.) — Google's anisotropic quantization
- **Milvus** or **Pinecone** or **Weaviate** if the paper targets production systems
- **Any SIGMOD/VLDB paper from the last 2 years on the same sub-problem**

If the paper is about a specific sub-problem (e.g., filtered ANNS, streaming ANNS, GPU ANNS, disk-resident), include sub-problem-specific baselines:
- **Filtered/hybrid search**: Milvus hybrid, NHQ, Filtered-DiskANN, ACORN
- **Streaming/updatable**: FreshDiskANN, SPFresh
- **GPU**: CAGRA, GGNN, SONG
- **Disk-resident**: DiskANN, SPANN, Starling

## Standard datasets

For reviewer acceptance, use at least 3–5 of these:

**Classic benchmarks:**
- **SIFT1M** / **SIFT1B** (128-d, image descriptors)
- **GIST1M** (960-d)
- **DEEP1B** (96-d, image embeddings)
- **MSONG** / **Audio**
- **GloVe** (100-d or 200-d, text)

**Modern benchmarks:**
- **MS MARCO** passage ranking (768-d, BERT/ANCE embeddings)
- **BigANN** (ANN-Benchmarks suite)
- **LAION** (multimodal embeddings, 512-d or 768-d)
- **Cohere** or **OpenAI embeddings** on Wikipedia (modern high-dim: 768, 1024, 1536)

Include a **datasets table** (typically Table 2 or Table 3) with: name, $N$, $d$, type, distance metric, citation.

## Recall-latency curves — the critical plot

The single most important figure in most ANNS papers is the recall-latency (or recall-QPS) tradeoff curve. Standards:

- X-axis: recall@k (usually k=10 or k=100), log scale sometimes
- Y-axis: latency in ms (log scale) OR QPS (log scale, higher is better)
- One curve per system, with markers for different index parameters
- Include error bars or confidence bands
- Usually one subplot per dataset, arranged in a grid

Caption should state: dataset, k value, hardware, number of trials, and confidence interval basis.

## Framing system vs. algorithm papers

**System papers** (e.g., "MicroNN: On-device vector DB"): emphasize end-to-end architecture, engineering challenges (memory, disk, updates, multi-tenancy), integration with other components. Evaluation emphasizes throughput, resource usage, real workloads.

**Algorithm papers** (e.g., "Tribase: triangle-inequality pruning"): emphasize the core technique, theoretical properties (lossless, complexity bounds), micro-benchmarks isolating the technique. Evaluation emphasizes per-query metrics and ablation of the technique's components.

**Hybrid papers** (most common): do both, but lead with one. Pick a lead based on the main contribution.

## AI-era systems themes (2024–2026)

Recent SIGMOD/VLDB papers in this space cluster around:

1. **Filtered / hybrid vector search** — combining vector similarity with structured predicates
2. **Multi-vector retrieval** — ColBERT-style late interaction, multi-vector indexes
3. **RAG system infrastructure** — caching, routing, retrieval quality, cost
4. **LLM for DB** — text-to-SQL, query optimization with LLMs, LLM-assisted tuning
5. **DB for LLM** — vector DBs, prompt caches, KV caches as a storage problem
6. **Streaming / updatable indexes** — supporting inserts and deletes at scale
7. **Disk-resident and tiered-memory indexes** — beyond in-memory assumptions
8. **GPU-accelerated vector search** — batched queries, graph traversal on GPUs
9. **Learned index components** — for partitioning, routing, pruning decisions
10. **Benchmarking and analysis (EA&B)** — experimental evaluation of the state-of-the-art

Frame the paper against these themes. Reviewers want to see awareness of where the field is.

## Framing the contribution — three common archetypes

**Archetype 1: "Existing indexes have X inefficiency, we fix it."** Common for algorithm papers. Identify a specific source of inefficiency (redundant distance computations, coarse granularity, suboptimal layout), quantify it, propose the fix.

**Archetype 2: "A new setting requires new techniques."** Common for system papers. The setting (e.g., on-device, streaming, filtered) is new or under-studied; existing approaches were designed for a different setting and don't transfer.

**Archetype 3: "A component within a larger pipeline is the bottleneck."** Common for AI-era systems. The RAG/LLM/recommender pipeline exists; you're optimizing a specific component (index, cache, router).

Make the archetype explicit in the intro. Reviewers map papers to archetypes to evaluate novelty.

## Handling "LLM-related" papers at DB venues

Papers using or for LLMs face additional scrutiny at DB venues because reviewers worry about:
- **Contribution being ML rather than DB** — anchor the contribution in a DB problem (storage, retrieval, indexing, query execution). Don't just present a new prompt or fine-tuned model.
- **Reproducibility** — closed LLMs make results hard to reproduce. Use open models where possible; if using closed ones, pin versions and include cost/latency numbers.
- **Evaluation beyond accuracy** — DB reviewers care about latency, throughput, cost, scalability. Don't just show accuracy.

Common save: frame the contribution as a *system* or *technique*, with LLMs as a component, and evaluate as a DB paper would (latency, throughput, resource usage, recall-at-cost tradeoff).

## Reproducibility expectations

PVLDB EA&B papers **require** reproducibility artifact submission. Other tracks strongly encourage it. Expectations:
- Open-source code (GitHub link in the paper)
- Public datasets (or instructions to obtain them)
- Scripts to reproduce every experiment
- Dockerfile or conda environment spec

Mention reproducibility explicitly in the paper. Reviewers look for this.

## Experimental hygiene checklist

Use this when reviewing or drafting the evaluation section:

- [ ] Hardware fully specified (CPU model, cores, RAM, disk type, GPU if used)
- [ ] Cold vs. warm cache behavior addressed
- [ ] Number of query trials stated; error bars or CIs included
- [ ] Parameter sweeps for key knobs shown
- [ ] Index build time reported alongside query performance
- [ ] Memory footprint reported (not just latency)
- [ ] At least one scaling experiment (vs. $N$, vs. $d$, vs. $k$)
- [ ] Comparison at equal recall (not cherry-picked operating points)
- [ ] Ablation for each claimed technique contribution
- [ ] Real workload in addition to synthetic (for system papers especially)
