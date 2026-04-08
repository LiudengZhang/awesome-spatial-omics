# Deconvolution Benchmarks

**Pipeline question:** Given a spatial transcriptomics spot containing multiple cells, which method best estimates the cell-type composition?

## Overview

Spatial deconvolution -- inferring cell-type proportions within each spot or voxel -- is one of the most benchmarked tasks in spatial omics. Three independent large-scale comparisons have been published since 2022, collectively evaluating over 20 methods. The consensus is clear on the top performers, though the best choice depends on dataset size, reference quality, and computational budget.

## Key benchmark studies

### Li et al., Nature Methods 2022 — 16 methods

- **Paper:** [Benchmarking spatial and single-cell transcriptomics integration methods](https://doi.org/10.1038/s41592-022-01480-9)
- **Methods tested:** 16 (Cell2location, RCTD, Tangram, SPOTlight, Stereoscope, DestDE, DSTG, STRIDE, SpatialDWLS, MuSiC, BayesTME, STdeconvolve, Seurat, SCDC, AutoGeneS, and others)
- **Datasets:** Simulated pseudo-spots from scRNA-seq, Visium DLPFC, mouse brain
- **Evaluation metrics:** RMSE, JSD, Pearson correlation of predicted vs. true proportions

**Key findings:**

| Tier | Methods | Notes |
|---|---|---|
| Top | Cell2location, RCTD, Tangram | Consistently best across metrics |
| Strong | SPOTlight, Stereoscope, SpatialDWLS | Good performance, some dataset-specific weaknesses |
| Variable | STdeconvolve, STRIDE, DestDE | Performance depends heavily on reference quality |
| Weak | AutoGeneS, SCDC | High variance, poor rare cell detection |

### Nature Communications 2023 — 18 methods

- **Paper:** Comprehensive evaluation of spatial transcriptomics deconvolution methods
- **Methods tested:** 18 (adds CARD, SpaCET, SpatialDecon to the Li et al. set)
- **Datasets:** Expanded to include Slide-seq, simulated multi-technology data
- **Additional evaluation:** Robustness to reference batch effects, sensitivity to rare cell types

**Key additions to the consensus:**

- CARD performs well on datasets with smooth spatial patterns but struggles with sharp boundaries
- SpaCET shows promise for tumor microenvironment deconvolution specifically
- Reference quality matters more than method choice: all methods degrade substantially with mismatched or incomplete references

### Briefings in Bioinformatics 2023 — 12 methods

- **Paper:** Systematic comparison of spatial transcriptomics deconvolution methods
- **Methods tested:** 12 (subset of above, focused on practical usability)
- **Additional evaluation:** Runtime, memory usage, ease of installation, documentation quality

**Practical findings:**

| Method | Accuracy | Speed | Memory | Ease of use |
|---|---|---|---|---|
| Cell2location | Best | Slow (GPU recommended) | High | Moderate |
| RCTD | Very good | Fast | Low | Easy |
| Tangram | Good (mapping) | Moderate | Moderate | Easy |
| SPOTlight | Good | Fast | Low | Easy |
| STdeconvolve | Variable | Moderate | Moderate | Easy (reference-free) |

## Consensus findings

### Cell2location: best overall accuracy

Cell2location consistently ranks first or second across all three benchmarks. Its hierarchical Bayesian model effectively handles overdispersion in spatial count data and provides uncertainty estimates for each cell-type proportion. The main drawbacks are computational cost (GPU strongly recommended, hours per dataset) and sensitivity to reference preparation.

!!! tip "When to use Cell2location"
    Choose Cell2location when accuracy is the priority and a high-quality, well-annotated scRNA-seq reference is available. It excels on Visium data with 5--20 cell types per spot.

### RCTD: best speed-accuracy trade-off

RCTD (Robust Cell Type Decomposition) runs in minutes on datasets that take Cell2location hours. It uses a supervised statistical model with platform-specific error modeling. Accuracy is slightly lower than Cell2location but competitive with all other methods.

!!! tip "When to use RCTD"
    Choose RCTD for large datasets, rapid iteration, or when GPU resources are limited. It is the pragmatic default for most Visium analyses.

### Tangram: best for spatial mapping

Tangram optimizes a different objective than traditional deconvolution: it maps individual cells from a reference to spatial locations rather than estimating proportions. This makes it particularly useful when the goal is to project a rich scRNA-seq atlas onto spatial coordinates.

### Rare cell types remain challenging

All benchmarks agree that detecting rare cell types (< 5% of a spot's composition) is unreliable across all methods. Cell2location and RCTD detect rare types more reliably than alternatives, but false-negative rates remain high. This is a fundamental limitation of spot-level resolution rather than a methodological failure.

### Reference quality dominates method choice

The single most important factor is not the deconvolution method but the quality and completeness of the scRNA-seq reference. A mediocre method with an excellent reference outperforms an excellent method with a poor reference. Key reference requirements:

- The reference must contain all cell types present in the tissue
- Batch effects between reference and spatial data must be addressed
- Cell-type annotations must be at the appropriate granularity
- Reference-free methods (STdeconvolve) avoid this dependency but sacrifice accuracy

## Technology-specific considerations

| Technology | Best approach | Notes |
|---|---|---|
| Visium (55 um) | Cell2location or RCTD | 5--20 cells per spot; classic deconvolution target |
| Visium HD (2 um) | Often unnecessary | Near single-cell resolution; direct annotation may suffice |
| Slide-seq (10 um) | RCTD | 1--3 cells per bead; deconvolution is simpler |
| Stereo-seq (subcellular) | Bin-then-deconvolve | Aggregate to pseudo-spots first, then apply standard methods |
| Imaging-based (MERFISH, Xenium) | Not applicable | Single-cell resolution; segmentation replaces deconvolution |

## Further reading

- [Benchmark Synthesis](benchmark-synthesis.md) for cross-category findings
- [Clustering Benchmarks](clustering-benchmarks.md) for spatial domain detection after deconvolution
- [Datasets](../datasets/datasets.md) for benchmark dataset details
