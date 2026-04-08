# Spatially Variable Genes

**Pipeline question:** Which genes exhibit expression patterns that are spatially organized rather than randomly distributed across the tissue?

## Overview

Spatially variable gene (SVG) detection identifies genes whose expression varies in a structured way across tissue space. These genes mark spatial domains, gradients, and local microenvironments that would be invisible in dissociated scRNA-seq data. SVG detection typically serves as an exploratory step — surfacing candidate genes for spatial domain identification, ligand-receptor analysis, or biological interpretation.

## Key Methods

### SpatialDE / SpatialDE2
- **Paper:** [Nature Methods, 2018](https://doi.org/10.1038/nmeth.4636) / [bioRxiv, 2021](https://doi.org/10.1101/2021.02.02.429429)
- **Code:** [github.com/PMBio/SpatialDE](https://github.com/PMBio/SpatialDE)
- **Key innovation:** Gaussian process regression to model spatial gene expression, with automatic relevance determination for spatial versus non-spatial variance.
- **Strengths:**
    - Principled statistical framework with well-calibrated p-values
    - SpatialDE2 adds automatic expression histology for grouping genes by spatial pattern
- **Limitations:**
    - Computationally expensive — O(n^3) scaling limits applicability to large datasets
    - SpatialDE2 improves speed but remains slower than non-GP methods
- **Technology compatibility:** Visium, Slide-seq, ST, any spot-level data

### SPARK / SPARK-X
- **Paper:** [Nature Methods, 2020](https://doi.org/10.1038/s41592-019-0701-7) / [Genome Biology, 2021](https://doi.org/10.1186/s13059-021-02404-0)
- **Code:** [github.com/xzhoulab/SPARK](https://github.com/xzhoulab/SPARK)
- **Key innovation:** SPARK uses a spatial generalized linear mixed model; SPARK-X uses non-parametric covariance testing for scalability to millions of cells.
- **Strengths:**
    - SPARK-X is among the fastest SVG methods available
    - Non-parametric approach avoids distributional assumptions
    - Scales to Stereo-seq and other high-resolution platforms
- **Limitations:**
    - SPARK-X trades some statistical power for speed
    - Less sensitive to subtle spatial patterns than GP-based methods
- **Technology compatibility:** Visium, Slide-seq, Stereo-seq, Xenium, MERFISH

### nnSVG
- **Paper:** [Nature Communications, 2023](https://doi.org/10.1038/s41467-023-39748-z)
- **Code:** [github.com/lmweber/nnSVG](https://github.com/lmweber/nnSVG)
- **Key innovation:** Nearest-neighbor Gaussian processes that approximate full GP inference in O(n) time, enabling scalable and accurate SVG detection.
- **Strengths:**
    - Best accuracy in independent benchmarks
    - Scalable through nearest-neighbor approximation
    - Provides effect size estimates (proportion of spatial variance)
- **Limitations:**
    - Slower than SPARK-X despite approximations
    - R/Bioconductor only
- **Technology compatibility:** Visium, Visium HD, Slide-seq, Xenium, MERFISH, Stereo-seq

### SOMDE
- **Paper:** [Bioinformatics, 2021](https://doi.org/10.1093/bioinformatics/btab471)
- **Code:** [github.com/WhittakerLab/SOMDE](https://github.com/WhittakerLab/SOMDE)
- **Key innovation:** Self-organizing map dimensionality reduction of spatial coordinates before Gaussian process testing, dramatically reducing computation time.
- **Strengths:**
    - 5-10x faster than SpatialDE with comparable accuracy
    - Simple to use with minimal parameters
- **Limitations:**
    - SOM-based approximation may miss fine-grained spatial patterns
    - Less well-maintained than alternatives
- **Technology compatibility:** Visium, Slide-seq, ST

### Hotspot
- **Paper:** [Cell Systems, 2021](https://doi.org/10.1016/j.cels.2021.04.005)
- **Code:** [github.com/yoseflab/hotspot](https://github.com/yoseflab/hotspot)
- **Key innovation:** Identifies informative genes and gene modules using local autocorrelation statistics on any cell-cell similarity graph, not just spatial coordinates.
- **Strengths:**
    - Flexible — works with spatial, kNN, or other similarity graphs
    - Identifies gene modules (co-varying gene sets), not just individual SVGs
    - Fast computation
- **Limitations:**
    - Graph-based approach conflates spatial and transcriptomic neighborhoods if not careful
    - Less interpretable spatial pattern decomposition than SpatialDE
- **Technology compatibility:** Any spatial platform, also applicable to scRNA-seq

### SpatialCorr
- **Paper:** [bioRxiv, 2023](https://doi.org/10.1101/2023.08.03.551899)
- **Code:** [github.com/mbernste/SpatialCorr](https://github.com/mbernste/SpatialCorr)
- **Key innovation:** Tests for spatially varying gene-gene correlations, detecting genes whose co-expression structure changes across tissue space.
- **Strengths:**
    - Unique focus on spatial co-expression, not just spatial expression
    - Identifies genes involved in spatially-localized regulatory programs
- **Limitations:**
    - Computationally intensive for large gene panels
    - Conceptually more complex — harder to interpret than standard SVGs
- **Technology compatibility:** Visium, Slide-seq, any spot-level data

### trendsceek
- **Paper:** [Nature Methods, 2018](https://doi.org/10.1038/nmeth.4634)
- **Code:** [github.com/edsgard/trendsceek](https://github.com/edsgard/trendsceek)
- **Key innovation:** Uses marked point process statistics to detect spatial expression trends without assuming specific pattern types.
- **Strengths:**
    - Non-parametric approach detects diverse spatial patterns
    - Among the earliest SVG methods with solid statistical foundations
- **Limitations:**
    - Very slow — impractical for genome-wide testing on large datasets
    - Largely superseded by faster methods
- **Technology compatibility:** Visium, ST, Slide-seq

### PROST
- **Paper:** [Nature Communications, 2024](https://doi.org/10.1038/s41467-024-44835-w)
- **Code:** [github.com/WenMi1/PROST](https://github.com/WenMi1/PROST)
- **Key innovation:** Quantifies spatial expression patterns using a Positive-and-Negative-index (PI) score that captures both the strength and type of spatial autocorrelation.
- **Strengths:**
    - Fast and scalable
    - PI score provides interpretable pattern characterization
    - Also performs spatial domain detection
- **Limitations:**
    - Dual-purpose design (SVG + domains) may not optimize either fully
    - Less independent validation than nnSVG or SPARK-X
- **Technology compatibility:** Visium, Slide-seq, Stereo-seq

## Benchmark Summary

Independent benchmarks consistently rank **nnSVG** as the most accurate SVG detection method, with well-calibrated p-values and the best true-positive recovery rate. **SPARK-X** offers the best speed-accuracy tradeoff, making it the practical choice for large datasets (>50,000 spots/cells) where nnSVG becomes slow. Notably, the classical **Moran's I** statistic remains a competitive baseline — often matching or exceeding more complex methods in power, especially for strong spatial patterns.

!!! tip "Start simple"
    Moran's I (available in squidpy and scanpy) is a reasonable first pass for SVG detection. Move to nnSVG for rigorous analysis or SPARK-X when dataset size demands speed.

## When to Use What

| Your data | Your goal | Recommended | Why |
|-----------|-----------|-------------|-----|
| Visium (<10K spots) | Rigorous SVG detection | nnSVG | Best accuracy, manageable runtime at this scale |
| Large dataset (>50K cells) | Fast SVG screen | SPARK-X | Scalable non-parametric testing |
| Quick exploration | Initial SVG candidates | Moran's I (squidpy) | Fast, no additional installation, competitive accuracy |
| Any data | Gene module discovery | Hotspot | Identifies co-varying gene groups, not just individual SVGs |
| Any data | Spatially varying co-expression | SpatialCorr | Unique capability to detect changing gene-gene correlations |

## Technology Compatibility

| Method | Visium | Visium HD | Xenium | MERFISH | CosMx | CODEX | Stereo-seq |
|--------|--------|-----------|--------|---------|-------|-------|-----------|
| SpatialDE/2 | Yes | - | - | - | - | - | - |
| SPARK-X | Yes | Yes | Yes | Yes | - | - | Yes |
| nnSVG | Yes | Yes | Yes | Yes | - | - | Yes |
| SOMDE | Yes | - | - | - | - | - | - |
| Hotspot | Yes | Yes | Yes | Yes | Yes | - | Yes |
| SpatialCorr | Yes | - | - | - | - | - | - |
| trendsceek | Yes | - | - | - | - | - | - |
| PROST | Yes | - | - | - | - | - | Yes |
