# Spatially Variable Gene Benchmarks

**Pipeline question:** Which method best identifies genes whose expression varies across tissue space?

## Overview

Spatially variable gene (SVG) detection identifies genes whose expression patterns exhibit spatial structure beyond what random placement would produce. These genes are biologically interesting because they mark tissue domains, gradients, or localized cell states. Two systematic benchmarks have compared SVG methods, revealing a trade-off between statistical rigor and computational speed that becomes acute at large dataset scales.

## Key benchmark studies

### Weber et al., Genome Biology 2023 — 7 methods

- **Paper:** [nnSVG for the scalable identification of spatially variable genes using nearest-neighbor Gaussian processes](https://doi.org/10.1038/s41467-023-39748-z)
- **Methods tested:** nnSVG, SpatialDE, SpatialDE2, SPARK, SPARK-X, Moran's I, Geary's C
- **Datasets:** DLPFC (Visium), mouse brain (Visium), mouse olfactory bulb (Slide-seq), simulated data with known ground truth
- **Evaluation metrics:** True positive rate, false positive rate, runtime, memory usage, rank correlation with known marker genes

**Key findings:**

| Method | Accuracy (TPR) | FPR control | Runtime (10k spots) | Runtime (50k spots) | Scalability |
|---|---|---|---|---|---|
| nnSVG | Best | Well-calibrated | 10--30 min | 1--3 h | Good |
| SpatialDE2 | Good | Well-calibrated | 30--60 min | 5--10 h | Moderate |
| SPARK | Good | Well-calibrated | 20--60 min | Often fails | Poor |
| SPARK-X | Good | Slightly liberal | 1--5 min | 10--30 min | Best |
| Moran's I | Moderate | Liberal | <1 min | 2--5 min | Excellent |
| SpatialDE | Good | Well-calibrated | 1--3 h | >24 h | Poor |
| Geary's C | Moderate | Liberal | <1 min | 2--5 min | Excellent |

### Briefings in Bioinformatics 2023 — 9 methods

- **Paper:** Systematic evaluation of spatially variable gene detection methods
- **Methods tested:** 9 (adds SOMDE, scGCO to the Weber set)
- **Datasets:** Multiple Visium datasets (DLPFC, mouse brain, breast cancer), seqFISH, simulated
- **Additional evaluation:** Sensitivity to spot density, robustness to dropout noise, pattern-type specificity (hotspot vs. gradient vs. periodic)

**Pattern-specific findings:**

| Method | Hotspots | Gradients | Periodic patterns | Mixed patterns |
|---|---|---|---|---|
| nnSVG | Best | Best | Good | Best |
| SpatialDE2 | Good | Good | Best | Good |
| SPARK-X | Good | Moderate | Moderate | Good |
| Moran's I | Good | Moderate | Poor | Moderate |
| SOMDE | Good | Good | Moderate | Good |
| scGCO | Moderate | Poor | Good | Moderate |

## Consensus findings

### nnSVG: best accuracy-scalability balance

nnSVG uses nearest-neighbor Gaussian processes to model spatial autocorrelation, providing a principled statistical framework that scales to tens of thousands of spots. It consistently identifies known marker genes with well-calibrated p-values and detects all major pattern types (hotspots, gradients, periodic patterns).

!!! tip "When to use nnSVG"
    nnSVG is the recommended default for datasets up to ~50,000 spots. It provides statistically rigorous results with reasonable runtime. For larger datasets, consider running nnSVG on a subsample or switching to SPARK-X.

### SPARK-X: fastest for large datasets

SPARK-X uses non-parametric kernel tests instead of Gaussian process models, achieving dramatically faster runtimes at the cost of slightly less precise p-value calibration. On datasets exceeding 50,000 spots (common for Slide-seq, Stereo-seq, and Visium HD), SPARK-X may be the only feasible option among principled statistical methods.

!!! tip "When to use SPARK-X"
    Choose SPARK-X when the dataset has >50,000 spots, when rapid iteration is needed, or when the goal is filtering to a candidate gene set rather than precise statistical testing.

### SpatialDE: historically important but slow

SpatialDE was the first dedicated SVG detection method and remains widely cited. However, its Gaussian process implementation scales poorly: runtime is cubic in the number of spots. SpatialDE2 addresses some scalability issues but remains slower than nnSVG with comparable accuracy. For new analyses, nnSVG or SPARK-X are preferred.

### Moran's I: surprisingly competitive

Moran's I is a classical spatial autocorrelation statistic from geography, requiring no specialized spatial transcriptomics software. Despite its simplicity, it identifies many of the same genes as more complex methods, particularly for strong hotspot patterns. Its p-values tend to be liberal (anti-conservative), leading to more false positives, but as a ranking statistic for gene prioritization it performs well.

!!! note "Moran's I as a quick filter"
    Moran's I can be computed in seconds for any dataset size using standard spatial statistics libraries (e.g., `squidpy.gr.spatial_autocorr`). It serves as a useful first-pass filter before applying more rigorous methods to a reduced gene set.

### Pattern type matters

Not all spatial patterns are equally detectable. Hotspot patterns (localized expression in a small region) are detected by all methods. Gradient patterns (smooth expression changes across tissue) are best captured by Gaussian process methods (nnSVG, SpatialDE2). Periodic patterns (repeating expression motifs) are the hardest and are best captured by SpatialDE2's periodic kernel. The choice of method should consider what types of spatial patterns are biologically expected.

## Practical considerations

**Gene filtering before SVG testing.** Running SVG detection on all genes (20,000+) is computationally wasteful and inflates multiple testing corrections. Pre-filtering to genes detected in >5--10% of spots, with reasonable expression levels, reduces the gene set to 2,000--5,000 candidates without losing biologically relevant SVGs.

**Multiple testing correction.** All methods return p-values that require multiple testing correction (typically Benjamini-Hochberg). The stringency of the FDR threshold depends on the application: 0.05 for discovery, 0.01 for high-confidence gene sets.

**Effect size vs. statistical significance.** A gene can be statistically significantly spatially variable but biologically uninteresting (e.g., a housekeeping gene with minor spatial variation due to capture efficiency differences). Ranking by effect size (e.g., nnSVG's sigma.sq parameter, or Moran's I value) in addition to p-value helps prioritize biologically meaningful SVGs.

**Technology considerations.** SVG methods designed for spot-based data (Visium, Slide-seq) may not perform optimally on imaging-based single-cell data (MERFISH, Xenium) where the "spot" concept does not apply. For imaging-based data, aggregating to a grid or using cell-level coordinates as input is necessary.

## Method selection guide

| Scenario | Recommended method | Rationale |
|---|---|---|
| Standard Visium (<20k spots) | nnSVG | Best overall accuracy |
| Large dataset (>50k spots) | SPARK-X | Only feasible option at scale |
| Quick exploration | Moran's I via Squidpy | Seconds to run, good ranking |
| Periodic pattern detection | SpatialDE2 | Periodic kernel captures repeats |
| Imaging-based (MERFISH, Xenium) | nnSVG on binned data | Bin to pseudo-spots first |
| Benchmarking or methods paper | Multiple methods | Report concordance across methods |

## Further reading

- [Benchmark Synthesis](benchmark-synthesis.md) for cross-category findings
- [Clustering Benchmarks](clustering-benchmarks.md) for using SVGs to inform spatial domain detection
- [Datasets](../datasets/datasets.md) for benchmark dataset details
- [Glossary: SVG](../glossary.md) for definition
