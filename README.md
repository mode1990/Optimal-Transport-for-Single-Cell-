
---

# Optimal Transport Analysis for scRNA-seq Data

## Overview

This project analyzes the similarity between dopaminergic neuronal populations across genetic backgrounds using **Optimal Transport (OT)**. Specifically, it compares:

* **Mutant vs. Isogenic Controls**
* **Mutant vs. Healthy Controls**
* **Isogenic Controls vs. Healthy Controls**

By applying OT to UMAP embeddings of scRNA-seq data, and weighting cells by quality and cluster size, this approach quantifies how transcriptomic profiles differ due to genetic mutations in **LRRK2**, **GBA1**, and **SNCA** with their CRISPR-corrected isogenic lines and healthy controls

---

## Data Input

* **File:** `all.h5ad`
* **Format:** AnnData object containing single-cell RNA-seq data.
* **Expected fields in `obs`:**

  * `BroadCellType`: should contain values like `'DAN'`, `'IDN'` for dopaminergic lineage filtering.
  * `Mutation`: labels for genetic background (e.g., `'LRRK2'`, `'isoLRRK2'`, `'HC'`, etc.)

---

## Dependencies

Install via pip:

```bash
pip install scanpy numpy matplotlib seaborn pandas scikit-learn pot
```

---

## Steps Performed

1. **Data Subsetting**:

   * Focuses on dopaminergic cells (`DAN` and `IDN` types).

2. **Quality Control & Embedding**:

   * Computes QC metrics if not present (`n_counts`).
   * Computes PCA and UMAP if not present.

3. **Population Weighting**:

   * Combines quality-based weights (log-transformed UMI counts) and cluster-based weights (Leiden clusters on UMAP).

4. **Optimal Transport**:

   * Computes OT distances and transport plans between pairs of cell populations using Sinkhorn algorithm.

5. **Comparative Results**:

   * Reports OT distances.
   * Evaluates whether isogenic controls are more similar to mutants or healthy controls.

---

## Output

The script prints:

* Number of cells per group.
* OT distances between group pairs.
* Which populations are most similar.
* Whether the isogenic control is transcriptomically closer to mutant or healthy controls.

Additionally, the `results` dictionary stores:

* Coordinates, weights, cost matrices, OT distances, and transport plans for all comparisons.

---
