# Pod 1: Privacy-Preserving Fuzzy Private Set Intersection (Fuzzy PSI)

## Background & Motivation

In modern data collaboration scenarios, organizations frequently need to identify matching entities (e.g., customers, employees, or partners) across datasets without revealing sensitive information. Traditional approaches require data sharing, creating privacy risks and compliance challenges. Privacy-enhancing technologies (PETs) offer a solution, but existing private set intersection (PSI) protocols do not adequately handle the fuzzy nature of real-world name matching (e.g., `"John A. Smith"` vs. `"J A Smith"`). This project addresses the critical need for privacy-preserving fuzzy matching to enable secure data collaboration while maintaining strict compliance with global data protection regulations.

## Project Context

This task directly advances our overall goal of establishing a secure, privacy-preserving data collaboration framework. By enabling private fuzzy matching, we empower organizations to:

- Collaborate on customer data matching without exposing raw data  
- Comply with GDPR, CCPA, and other privacy regulations  
- Enable secure cross-organization analytics while protecting sensitive information  
- Establish a foundation for future PET-integrated data sharing use cases  

## Technical/Research Context

Current private set intersection protocols (e.g., PSI with Bloom filters, OT-based PSI) are designed for **exact matching only**. Fuzzy matching is essential for real-world name data, where variations in spacing, abbreviations, and formatting are common.

The two matching semantics required are:

- **Containment matching**: e.g., `"A B C"` ⊆ `"A B C D"` → match  
- **Overlap matching**: e.g., `"A B C"` ∩ `"A C D"` = `{A, C}` → match if overlap ≥ threshold  

These are critical for name matching in financial, healthcare, and customer relationship management domains. Our solution must address the computational complexity of fuzzy matching at scale (up to **3 million records**) while maintaining strong privacy guarantees.

## Threat Model Relevance

This project assumes a **semi-honest adversary** threat model where:

- Both parties (data owners) follow the protocol correctly  
- But may attempt to infer information about the other party’s dataset from protocol execution  
- The protocol must prevent leakage of any information beyond the matching results  
- No collusion between parties is assumed (standard in PET literature)

## Objectives & Success Criteria

### Primary Objective

Design and implement a privacy-preserving fuzzy matching protocol that supports containment and overlap matching on datasets up to 3 million records, completing each task within 10 minutes while maintaining strict privacy guarantees.

### Functional Objectives

- Implement two fuzzy matching rules: **containment matching** and **overlap matching**  
- Support datasets of up to **3 million records per party**  
- Achieve end-to-end matching completion time **≤ 10 minutes** on standard cloud infrastructure  
- Provide an API for integration with existing data processing pipelines  
- Handle tokenized names where each character represents a word (e.g., `"A B C"` → `["A", "B", "C"]`)

### Privacy/Scientific Objectives

- Ensure **zero information leakage** beyond the matching results (formal privacy guarantee)  
- Implement cryptographic protocols secure against **semi-honest adversaries**  
- Maintain computational efficiency while preserving privacy (**avoid O(n²) complexity**)  
- Support both containment and overlap matching within the same protocol architecture

### Success Criteria (Measurable)

- **≤ 10 minutes** for matching 3 million records on AWS `m5.xlarge` (4 vCPU, 16 GB RAM)  
- **100% accuracy** on containment matching (no false negatives)  
- **≥ 95% accuracy** on overlap matching (with configurable threshold)  
- **Zero data leakage**, as verified through security analysis  
- Code passes internal security review with **no critical vulnerabilities**

## Expected Outcomes & Deliverables

### Code & Tests

- Complete implementation in **Rust** (for performance) with **Python bindings**  
- Comprehensive test suite covering edge cases (empty datasets, single-token names, etc.)  
- Test coverage **≥ 85%** for core matching logic  
- Repository: `git@github.com:org/pets/fuzzy_psi.git`, branch: `fuzzy_psi_v1`  
- Integration tests with mock data at scales: **100K, 500K, 1M, 3M records**

### Documentation

- Technical design document detailing protocol architecture  
- API reference for system integration  
- Privacy guarantee proof (formal or empirical)  
- Internal knowledge base article: *“Implementing Fuzzy PSI in Production”*

### Data/Results

- Benchmark results across dataset sizes (**100K → 3M records**)  
- Performance metrics: **CPU, memory, network usage** per run  
- Comparison against baseline implementations (e.g., exact PSI)  
- Log files showing execution times and resource consumption

### Analysis

- Written report on **privacy guarantees** and **security considerations**  
- Comparative analysis of implementation choices vs. alternatives  
- Trade-off discussion: **accuracy vs. performance vs. privacy**  
- Presentation slides for internal technology review

## Validation & Verification Plan

### Privacy Verification Method

- Formal security proof using standard PSI security models  
- Implementation of a **privacy audit tool** to measure potential leakage  
- Empirical evaluation using privacy metrics (e.g., mutual information between input/output)  
- Independent **security review** by internal security team

### Functional Test Plan

- Unit and integration tests for all matching logic (containment + overlap)  
- Edge case validation: empty strings, single-token inputs, duplicate tokens  
- Scalability testing at 100K → 3M record increments  
- Integration testing with mock pipeline components  
- Stress testing at maximum dataset size (3M × 3M)

### Performance Benchmarks

- **Metrics**: Execution time, peak memory usage, network I/O  
- **Tools**: `criterion.rs` (Rust benchmarking), `perf` (system profiling)  
- **Baseline**: Exact PSI runtime for equivalent dataset sizes  
- **Target**: ≤ 10 minutes for 3M-record matching on `m5.xlarge`  
- **Environment**: AWS `m5.xlarge` instance with SSD storage, Ubuntu 22.04 LTS

---

This project delivers a foundational capability for secure, compliant data collaboration—enabling real-world fuzzy entity resolution without compromising privacy. The resulting system will serve as a core building block for future PET-enabled workflows across the organization.