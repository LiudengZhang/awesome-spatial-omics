# nnSVG

**Verdict:** The best accuracy-scalability balance for spatially variable gene detection -- the right default for most datasets.

**Citation:** Weber LM, Saha A, Datta A, Hansen KD, Hicks SC. "nnSVG for the scalable identification of spatially variable genes using nearest-neighbor Gaussian processes." *Nature Communications* 14, 4059 (2023). [DOI: 10.1038/s41467-023-39748-z](https://doi.org/10.1038/s41467-023-39748-z)

## Problem Setup

Identifying spatially variable genes (SVGs) -- genes whose expression varies across tissue space in a structured way rather than randomly -- is a foundational analysis step in spatial transcriptomics. Early methods like SpatialDE and SPARK used Gaussian process (GP) models that captured spatial autocorrelation accurately but scaled quadratically with the number of spots (O(n^3) for exact GPs). As datasets grew from hundreds to tens of thousands of spots, these methods became computationally prohibitive. Faster alternatives like SPARK-X sacrificed some accuracy for speed. nnSVG aims to occupy the middle ground.

## Method

nnSVG uses nearest-neighbor Gaussian processes (NNGPs), an approximation to full GPs that restricts the covariance structure to each spot's k nearest neighbors rather than all pairwise distances. This reduces computational complexity from O(n^3) to O(n * k^3), where k is typically 10--15. Since k is fixed and small, the method scales linearly with dataset size while preserving the GP framework's ability to model complex spatial correlation patterns.

For each gene, nnSVG fits an NNGP model with a Gaussian spatial covariance function, estimating the proportion of variance explained by spatial structure versus noise. Genes are ranked by a likelihood ratio test comparing the spatial model to a non-spatial null. The method also estimates the spatial range parameter for each gene, providing information about the length scale of spatial variation -- whether a gene varies at fine (cellular neighborhood) or coarse (tissue region) scales.

The implementation uses the BRISC package for efficient NNGP fitting and processes genes independently, enabling straightforward parallelization across cores.

## Evaluation

Benchmarked on the 10x Visium DLPFC (dorsolateral prefrontal cortex) dataset and mouse olfactory bulb data, nnSVG achieves detection accuracy comparable to exact GP methods (SpatialDE, SPARK) while running orders of magnitude faster on large datasets. On a dataset with ~3,500 spots, nnSVG completed in minutes versus hours for SpatialDE. The method correctly identified known layer marker genes in DLPFC as top-ranked SVGs.

Compared to SPARK-X (the fastest alternative), nnSVG shows higher sensitivity for genes with gradual spatial gradients, where SPARK-X's nonparametric approach can miss subtle patterns. However, SPARK-X remains faster for datasets exceeding 50,000 spots, where even the linear scaling of nnSVG becomes noticeable.

## Honest Assessment

**Strengths:**

- Best accuracy-scalability tradeoff in the SVG detection space: retains the statistical rigor of full GP methods while scaling to tens of thousands of spots.
- Provides per-gene spatial range estimates, giving biological insight into the length scale of spatial variation -- not just a binary "spatially variable or not" call.
- Gene-level parallelization is trivial, making effective use of multi-core machines.
- Statistically principled framework with proper likelihood-based testing, unlike heuristic approaches.

**Limitations:**

- Still slower than SPARK-X for very large datasets (>50,000 spots), and Visium HD data with millions of bins remains out of reach without aggregation (see [Visium HD](visium-hd.md)).
- The Gaussian covariance function assumes smooth spatial variation -- periodic or discontinuous spatial patterns (e.g., sharp layer boundaries) may be underdetected.
- Implemented in R, which creates friction for Python-centric spatial omics workflows built around [Squidpy](squidpy.md) and [SpatialData](spatialdata.md).
- Detection of SVGs is only as meaningful as the downstream interpretation -- the method identifies *what* varies spatially but not *why*.

**Design Decision:** The key bet is that nearest-neighbor approximation preserves enough of the GP's spatial modeling power to justify the complexity over simpler nonparametric tests. The benchmark results validate this bet for moderate-sized datasets, but the field's trajectory toward million-spot datasets may eventually favor faster approximate methods.
