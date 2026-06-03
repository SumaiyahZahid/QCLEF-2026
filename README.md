# QCLEF 2026 — FAST-NUCES

**Quantum Computing for Information Retrieval**
Submission by **Sumaiyah Zahid**, FAST-NUCES, **Dr Muhammad Atif Tahir**, IBA, Muhammad Rabeet Sagri, GhangorCloud Inc.
[QCLEF 2026 Lab](https://qclef.dei.unipd.it/clef2026-lab) @ CLEF 2026

---

## Overview

This repository contains the code and submissions for the **QCLEF 2026** shared task on quantum computing applied to information retrieval problems. The framework leverages both **Quantum Annealing (QA)** via D-Wave and **Simulated Annealing (SA)** for feature selection, instance selection, and clustering tasks.

The core idea across all tasks is to formulate each problem as a **Quadratic Unconstrained Binary Optimisation (QUBO)** problem and solve it using quantum or classical annealing. Feature importance is estimated using a **KNN-based Mutual Information estimator** (sklearn) and redundancy is penalised using **Pearson correlation**, replacing the histogram-based approach used in prior work.

---

## Results

### Task 1A — Feature Selection (MQ2007)

Evaluation metric: **nDCG@10** via LambdaMART. Lower feature count with competitive ranking quality is the goal.

| Submission ID | nDCG@10 | Annealing Time (μs) | Type | Features |
|---|---|---|---|---|
| 1A_MQ2007_SA_FAST-NUCES_k12 | **0.4412** | 4,322,921 | SA | 12 |
| 1A_MQ2007_QA_FAST-NUCES_k12 | **0.4472** | 154,243 | QA | 18 |
| BASELINE_ALL | 0.4473 | — | — | 46 |
| RFE_BASELINE_HALF | 0.4450 | — | — | 23 |

**Key finding:** QA achieves near-baseline nDCG@10 (0.4472 vs 0.4473) using only 18 features — a 61% reduction — while SA achieves competitive performance with just 12 features. QA also runs in significantly less annealing time than SA (154,243 μs vs 4,322,921 μs).

---

### Task 1B — Feature Selection (ICM Recommendation)

Evaluation metric: **nDCG@10** via Item-Based KNN (cosine similarity, shrinkage=5, k=100).

#### ICM 100 (100 features)

| Submission ID | nDCG@10 | Annealing Time (μs) | Type | Features |
|---|---|---|---|---|
| 1B_100_ICM_SA_FAST-NUCES_Ensemble | **0.0229** | 15,485,673 | SA | 75 |
| 1B_100_ICM_QA_FAST-NUCES_Ensemble | 0.0218 | 456,086 | QA | 72 |
| ALL_FEATURES | 0.0226 | — | — | 100 |

SA surpasses the all-features baseline (0.0229 vs 0.0226) using 75 features. QA achieves comparable performance with significantly lower annealing time.

#### ICM 400 (400 features)

| Submission ID | nDCG@10 | Annealing Time (μs) | Type | Features |
|---|---|---|---|---|
| 1B_400_ICM_SA_FAST-NUCES_Ensemble | 0.0289 | 129,408,361 | SA | 200 |
| 1B_100_ICM_QA_FAST-NUCES_Ensemble | **0.0307** | 36,735 | QA | 150 |
| ALL_FEATURES | 0.0328 | — | — | 400 |

QA outperforms SA at this scale (0.0307 vs 0.0289) using fewer features (150 vs 200) and dramatically less annealing time (36,735 μs vs 129,408,361 μs) — demonstrating QA's scalability advantage on larger problems.

---

### Task 2 — Instance Selection (LLM Fine-tuning)

Evaluation metric: **Macro F1** on noisy label setting. Goal is high F1 with maximum instance reduction.

| Submission ID | Macro F1 | Avg Reduction | Avg Fine-tuning Time (s) | Annealing Time (μs) | Type |
|---|---|---|---|---|---|
| FAST-NUCES_Algorithm1 | 57.9 (±8.8) | 75.03% | 541.7 (±0.3) | 167,180,028 | SA |
| FAST-NUCES_DivideMix (SA) | 56.3 (±9.1) | 70.04% | 642.4 (±28.6) | 2,918,988,448 | SA |
| FAST-NUCES_DivideMix (QA) | **58.5 (±11.9)** | 60.98% | 797.4 (±11.1) | 2,643,107 | QA |
| Baseline (no noise) | 88.9 (±0.8) | — | 1,997.3 (±5.7) | — | — |
| Baseline (with noise) | 84.3 (±2.5) | — | 1,904.3 (±1.4) | — | — |

**Key finding:** QA-based DivideMix achieves the best Macro F1 (58.5) with 60.98% instance reduction and far lower annealing time than the SA variant (2,643,107 μs vs 2,918,988,448 μs) — a 1,100× speedup. Our methods trade absolute accuracy for substantial computational efficiency and dataset compression.

---

### Task 3 — Clustering (Document Embeddings)

Evaluation metrics: **nDCG@10** and **Davies-Bouldin Index (DBI)**. Lower DBI = better cluster compactness.

#### 10 Centroids

| Submission ID | nDCG@10 | DBI | Annealing Time (μs) | Type |
|---|---|---|---|---|
| FAST-NUCES_KMEDOIDS | 0.5489 | 6.9336 | 2,277,560 | SA |
| FAST-NUCES_KMEDOIDS_1 | **0.5901** | 7.2754 | 7,881,661 | SA |
| FAST-NUCES_KMEDOIDS_QA | **0.5901** | 7.2754 | 7,979,288 | QA |
| FAST-NUCES_FPS-QUERYAWARE | 0.5709 | **6.6164** | 11,819,054 | SA |
| FAST-NUCES_FPS-QUERYAWARE | 0.5497 | 7.0623 | **16,035** | QA |
| Baseline | 0.5509 | 7.9892 | — | — |

#### 25 Centroids

| Submission ID | nDCG@10 | DBI | Annealing Time (μs) | Type |
|---|---|---|---|---|
| FAST-NUCES_KMEDOIDS_1 | **0.5263** | **5.7743** | 7,766,515 | SA |
| FAST-NUCES_FPS-QUERYAWARE | 0.5173 | 5.4729 | 14,502,851 | SA |
| FAST-NUCES_FPS-QUERYAWARE | 0.4793 | 5.6272 | **16,028** | QA |
| Baseline | 0.5284 | 6.1201 | — | — |

#### 50 Centroids

| Submission ID | nDCG@10 | DBI | Annealing Time (μs) | Type |
|---|---|---|---|---|
| FAST-NUCES_FPS-QUERYAWARE (SA) | 0.5411 | **4.3671** | 18,007,884 | SA |
| FAST-NUCES_FPS-QUERYAWARE (QA) | **0.5461** | 4.5136 | **16,045** | QA |
| Baseline | 0.4656 | 5.3679 | — | — |

**Key finding:** FPS-QUERYAWARE with QA achieves the best nDCG@10 at 50 centroids (0.5461 vs baseline 0.4656 — a 17% improvement) while running in only 16,045 μs annealing time. Performance gains become more pronounced as centroid count increases, demonstrating the scalability of the query-aware approach.

---

**Sumaiyah Zahid**
FAST-NUCES
QCLEF 2026 Participant
