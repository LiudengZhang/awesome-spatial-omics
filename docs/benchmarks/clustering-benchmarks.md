# Spatial Domain Detection Benchmarks

**Pipeline question:** Which method best identifies spatially coherent regions (domains) in spatial transcriptomics data?

## Overview

Spatial domain detection -- also called spatial clustering -- identifies regions of tissue that share expression programs while respecting spatial contiguity. Unlike standard scRNA-seq clustering, these methods must balance transcriptomic similarity with spatial coherence. Four major benchmarks have compared methods systematically since 2023, with a consistent finding: graph neural network (GNN) approaches outperform classical methods.

## Key benchmark studies

### Yuan et al., Nature Methods 2024 — 13 methods

- **Paper:** [Benchmarking spatial clustering methods with spatially resolved transcriptomics data](https://doi.org/10.1038/s41592-024-02215-8)
- **Methods tested:** 13 (GraphST, STAGATE, SpaGCN, BayesSpace, stLearn, Louvain, Leiden, mclust, HMRF, DR-SC, BASS, SpaceFlow, DeepST)
- **Datasets:** DLPFC (Visium), mouse brain (MERFISH, Slide-seq, Stereo-seq), mouse embryo (seqFISH)
- **Evaluation metrics:** ARI, NMI, CHAOS (spatial coherence), PAS (percentage of abnormal spots)

**Key findings:**

| Tier | Methods | ARI range (DLPFC) | Notes |
|---|---|---|---|
| Top | GraphST, STAGATE | 0.50--0.60 | Consistently best across datasets |
| Strong | BayesSpace, SpaGCN | 0.40--0.55 | BayesSpace excels on Visium specifically |
| Moderate | DR-SC, BASS, SpaceFlow | 0.30--0.45 | Competitive on some datasets |
| Baseline | Louvain, Leiden, mclust | 0.20--0.35 | No spatial information used |

### Dong et al., Genome Biology 2023 — 10 methods

- **Paper:** [Benchmarking spatial domain identification methods](https://doi.org/10.1186/s13059-023-02984-1)
- **Methods tested:** 10 (GraphST, STAGATE, SpaGCN, BayesSpace, stLearn, Louvain, HMRF, SpaceFlow, SpatialPCA, BASS)
- **Datasets:** DLPFC, mouse olfactory bulb, human breast cancer, mouse hippocampus
- **Additional evaluation:** Sensitivity to hyperparameters, reproducibility across runs

**Key additions to the consensus:**

- STAGATE is more robust to hyperparameter choices than GraphST
- SpatialPCA provides interpretable latent spaces but ranks below GNN methods in clustering accuracy
- Method rankings shift between tissues: what works on cortical layers may not work on tumor microenvironments

### Chen et al., Briefings in Bioinformatics 2023 — 8 methods

- **Paper:** Evaluating spatial domain identification methods for spatial transcriptomics
- **Methods tested:** 8 (focused on computational efficiency and scalability)
- **Additional evaluation:** Runtime scaling with dataset size, GPU vs. CPU performance

**Scalability findings:**

| Method | 5k spots | 50k spots | 500k spots | GPU required |
|---|---|---|---|---|
| GraphST | 2 min | 15 min | 3 h | Yes |
| STAGATE | 3 min | 20 min | 4 h | Yes |
| BayesSpace | 5 min | 45 min | >12 h | No |
| SpaGCN | 1 min | 10 min | 2 h | Yes |
| Louvain | <1 min | 2 min | 15 min | No |

### iMeta 2025

- **Paper:** Recent community benchmarking of spatial clustering methods
- **Methods tested:** Expanded set including newer methods (BANKSY, CellCharter, GraphCompass)
- **Additional datasets:** Xenium, CosMx, MERSCOPE

**Emerging findings:**

- BANKSY shows strong performance by combining spatial and non-spatial features via a mixing parameter
- Newer methods handle multi-scale domains (both large regions and small niches) better than first-generation GNN approaches
- Performance gaps between methods shrink on high-resolution imaging-based data where spatial information is inherently richer

## Consensus findings

### GNN-based methods are the current standard

GraphST and STAGATE consistently outperform classical clustering methods by 10--20 ARI points across benchmarks. Their advantage comes from encoding spatial relationships directly into the graph structure, allowing the model to learn representations that are both transcriptomically informative and spatially coherent.

!!! tip "GraphST vs. STAGATE"
    GraphST tends to achieve slightly higher peak accuracy. STAGATE tends to be more robust and reproducible across runs. For most applications, either is a reasonable choice. STAGATE is often preferred when reproducibility matters (e.g., clinical applications) due to lower variance between runs.

### BayesSpace remains strong on Visium

BayesSpace uses a Bayesian statistical framework specifically designed for the hexagonal grid geometry of Visium spots. On Visium data, it is competitive with GNN methods while requiring no GPU. However, it does not generalize well to imaging-based platforms with irregular cell geometries.

### Performance drops on imaging-based data

All benchmarks report lower absolute performance on imaging-based platforms (MERFISH, seqFISH, Xenium) compared to Visium. This likely reflects both the increased complexity of single-cell-resolution data and the reduced gene coverage of imaging panels (hundreds of genes vs. genome-wide). Methods must handle larger cell counts, more irregular spatial arrangements, and noisier per-cell expression profiles.

### The number-of-clusters problem

!!! warning "A hidden source of benchmark inflation"
    Most benchmarks provide the correct number of clusters (K) as input to methods that require it. In practice, K is unknown and must be estimated. Methods like BayesSpace and mclust are sensitive to K, while graph-based approaches (Louvain, Leiden) and some GNN methods can estimate K automatically via resolution parameters. Benchmark performance may overstate real-world accuracy for K-dependent methods.

### Spatial coherence vs. biological accuracy

High spatial coherence (smooth domains with few isolated spots) is not always desirable. Infiltrating immune cells in a tumor should appear as scattered spots within a tumor domain, not be smoothed away. Methods that aggressively enforce spatial contiguity may sacrifice biological accuracy for visual cleanliness. The CHAOS and PAS metrics used in benchmarks capture spatial smoothness but not necessarily biological correctness.

## Practical recommendations

| Scenario | Recommended method | Rationale |
|---|---|---|
| Visium, < 50k spots | STAGATE or GraphST | Best accuracy, manageable compute |
| Visium, no GPU available | BayesSpace | Strong accuracy without GPU |
| Imaging-based (MERFISH, Xenium) | STAGATE or BANKSY | Better generalization to irregular geometries |
| Very large datasets (> 500k cells) | Leiden + spatial smoothing | GNN methods may be too slow |
| Exploratory analysis | Leiden/Louvain baseline first | Quick baseline, then refine with spatial methods |

## Further reading

- [Benchmark Synthesis](benchmark-synthesis.md) for cross-category findings
- [SVG Benchmarks](svg-benchmarks.md) for identifying genes that drive spatial domains
- [Datasets](../datasets/datasets.md) for benchmark dataset descriptions
