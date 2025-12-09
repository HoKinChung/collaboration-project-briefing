# Pod 2: Clustering-Enhanced Fuzzy Matching

## Background & Motivation

Fuzzy matching is a foundational operation in data linkage, deduplication, and cross-organizational collaboration. However, naive pairwise comparison approaches scale quadratically (O(n²))—making them infeasible for datasets with millions of records. While cryptographic protocols like Fuzzy PSI (Pod 1) can preserve privacy during matching, they inherit this computational bottleneck unless the candidate search space is drastically reduced. Clustering-based pre-filtering offers a promising path to sub-quadratic complexity by grouping similar records and limiting comparisons to within-cluster or near-cluster pairs. This project aims to design a novel, high-performance fuzzy matching algorithm that leverages clustering to accelerate matching while maintaining—or even improving—accuracy.

## Project Context

This task directly advances our broader goal of building scalable, privacy-preserving data collaboration infrastructure. By developing a clustering-enhanced fuzzy matching scheme, we:

- **Reduce computational load** on downstream PET protocols (e.g., Pod 1’s Fuzzy PSI), enabling real-time performance at scale  
- **Improve matching accuracy** through semantic-aware grouping (e.g., using embeddings or token co-occurrence)  
- **Enable future integration** with federated or privacy-preserving clustering methods  
- **Create publishable research** that positions our organization as a leader in efficient, privacy-conscious data science  

The output will feed directly into Pod 1’s pipeline as an optional pre-processing stage and may inform Pod 3’s FL use cases involving entity resolution.

## Technical/Research Context

Existing fuzzy matching techniques include:
- **MinHash + Locality-Sensitive Hashing (LSH)** for Jaccard similarity approximation  
- **TF-IDF + cosine similarity** with blocking keys  
- **Phonetic encoding** (e.g., Soundex, Metaphone) for name variants  
- **Embedding-based methods** (e.g., Sentence-BERT) for semantic similarity  

However, most are designed for centralized settings and do not consider integration with secure computation. Recent work (e.g., *Secure Approximate Matching via LSH*, NDSS ’22) explores privacy-preserving approximate matching but lacks optimization via intelligent clustering.

We aim to solve: **How can we design a clustering strategy that minimizes false negatives while maximizing candidate reduction, and that is compatible with privacy-preserving execution?**

## Threat Model Relevance

This project operates under a **trusted pre-processing** assumption: clustering is performed either:
- **Locally** (each party clusters its own data before secure matching), or  
- **Via a privacy-preserving clustering protocol** (future extension)

In the current phase, we assume clustering does **not leak sensitive information** because it occurs within a single trusted domain. Thus, the threat model aligns with Pod 1’s semi-honest setting—clustering is a performance optimization layer that must not compromise the end-to-end privacy guarantees of the full matching pipeline.

---

## Objectives & Success Criteria

### Primary Objective

Design and evaluate a novel clustering-based pre-filtering algorithm that significantly accelerates fuzzy matching for containment and overlap semantics while preserving high recall, with the goal of publishing the method in a top-tier venue.

### Functional Objectives

- Implement a modular clustering pre-processor for tokenized string records (e.g., `["A", "B", "C"]`)  
- Support both **containment** and **overlap** matching semantics in candidate generation  
- Generate candidate pairs with ≥95% recall compared to exhaustive pairwise matching  
- Reduce candidate pair count by ≥70% versus baseline blocking strategies (e.g., first-token blocking)  
- Provide clean APIs for integration with Pod 1’s Fuzzy PSI system  

### Privacy/Scientific Objectives

- Demonstrate that clustering improves **computational efficiency without sacrificing utility**  
- Formulate a theoretical bound (or empirical estimate) on **recall loss vs. speedup trade-off**  
- Ensure the algorithm is **agnostic to downstream privacy mechanisms** (i.e., works with any secure matcher)  
- Propose a pathway to **privacy-preserving clustering** (e.g., via secure k-means or FL-based embedding)

### Success Criteria (Measurable)

- **≥95% recall** on synthetic and real-world name-like datasets (vs. ground-truth matches)  
- **≥70% reduction** in candidate pairs compared to simple blocking baselines  
- End-to-end matching (clustering + Pod 1 PSI) completes **≤8 minutes** on 3M-record datasets  
- Algorithm accepted for publication at a **top-tier conference** (e.g., KDD, NeurIPS, USENIX Security, VLDB)  
- Code passes internal review for **modularity, documentation, and reproducibility**

---

## Expected Outcomes & Deliverables

### Code & Tests

- Open-source Python implementation (with optional Rust acceleration)  
- Repository: `git@github.com:org/pets/clustering_fuzzy.git`, branch: `main`  
- Unit tests covering clustering logic, candidate generation, and edge cases  
- Integration test with Pod 1’s Fuzzy PSI interface  
- Test coverage ≥80% for core algorithmic components  

### Documentation

- Algorithm design document with pseudocode and complexity analysis  
- API specification for candidate generator interface  
- Internal knowledge base article: *“Clustering for Scalable Fuzzy Matching”*  
- Reproducibility guide (dependencies, dataset links, hyperparameters)

### Data/Results

- Benchmark results on synthetic datasets (100K–3M records) and public name datasets (e.g., Febrl, NCVR)  
- Plots comparing: recall vs. candidate reduction, runtime vs. dataset size, accuracy vs. baseline methods  
- Ablation studies on clustering parameters (e.g., number of clusters, similarity thresholds)  
- Logs of candidate pair counts and matching outcomes across configurations  

### Analysis

- Draft of a research paper (8–10 pages) targeting a top-tier venue  
- Comparative analysis against 3+ baseline methods (e.g., LSH, sorted-neighborhood, TF-IDF blocking)  
- Discussion of limitations and extensions (e.g., handling typos, multilingual names)  
- Presentation deck for internal R&D seminar and external submission

---

## Validation & Verification Plan

### Privacy Verification Method

- Confirm that clustering is **local-only** in this phase (no cross-party data exchange)  
- Document assumptions for future secure clustering integration  
- Evaluate potential leakage if clustering outputs were exposed (risk assessment memo)

### Functional Test Plan

- **Correctness**: Verify candidate sets include all true matches (on labeled datasets)  
- **Scalability**: Test on 100K, 500K, 1M, and 3M record datasets  
- **Robustness**: Validate performance under noise (e.g., random token deletion/insertion)  
- **Integration**: End-to-end test with Pod 1’s Fuzzy PSI prototype  
- **Reproducibility**: Dockerized environment with fixed seeds and dependencies

### Performance Benchmarks

- **Metrics**:  
  - Recall @ candidate set  
  - Candidate pair reduction (%)  
  - Clustering + candidate generation time  
  - End-to-end time (with Pod 1 PSI)  
- **Tools**:  
  - `scikit-learn`, `faiss`, or `datasketch` for baseline comparisons  
  - `pytest-benchmark` for timing  
  - Custom evaluation harness for recall/precision  
- **Target Environment**: AWS `m5.2xlarge` (8 vCPU, 32 GB RAM), Ubuntu 22.04  
- **Baseline Comparison**: First-token blocking, MinHash LSH, sorted neighborhood

---

This project bridges systems optimization and machine learning to solve a critical bottleneck in privacy-preserving data collaboration. By making fuzzy matching scalable without sacrificing accuracy, it unlocks practical deployment of PETs in enterprise settings—and contributes original research to the scientific community.
